# Notes Louis

## Enumération des attaques sur les bridges

![Attaques sur les bridges](bridge_attacks.png)

## Résumé Nomad (article 3)

Nomad est un protocole permettant d'établir des bridges entre Ethereum et d'autres blockchains.

2 types de contrats dans Nomad : 
* [Home](https://docs.nomad.xyz/the-nomad-protocol/smart-contracts/home)
* [Replica](https://docs.nomad.xyz/the-nomad-protocol/smart-contracts/replica)

Un contrat 'Home' est le contrat principal où la dApp (decentralized application) est déployée et stockée. 
Chaque blockchain déploie un contrat 'Replica' qui valide et stocke des messages dans une structure de donnée de type ["Merkle tree"](https://academy.binance.com/en/glossary/merkle-tree)

Nomad a déployé un contrat permettant de gérer la réclamation des utilisateurs sur leurs actifs ("bridged assets"). Dans ce contrat, des paramètres d'initialisation ont été indiqués.

```
function initialize(
    // ...
    bytes32 _committedRoot,
    // ...
) public initializer {
    // ...
    confirmAt[_committedRoot] = 1;//warning!
    // ...
}
```

Lors d'un déploiement initial, le 'Merkle tree' est vide, avec une valeur égale à 0x0 * 32 et confirmAt[bytes32(0)] est égal à 1.

Puis il y a eu un changement dans l'implémentation de la fonction process()

```
function process(bytes memory _message) public returns (bool _success) {
    // ...
    require(acceptableRoot(messages[_messageHash]), "!proven");
    // ...
}
```

Il y a ajout d'un appel de la fonction acceptableRoot() qui fait elle-même appel au mapping "confirmAt[_committedRoot]".

```
uint256 _time = confirmAt[_root];
if (_time == 0) {
    return false;
}
return block.timestamp >= _time;
```

Pour tout nouveau message, root = bytes32(0) => confirmAt[bytes32(0)] = 1

acceptableRoot(bytes32(0)) retourne 'True' et permet à des messages d'être traités sans avoir été prouvés au préalable.

## Résumé Wormhole (article 5)

Wormhole, le bridge de Solana, a été manipulé pour créditer 120k ETH comme ayant été déposés sur Ethereum, ce qui a permis au pirate de frapper ("mint") l'équivalent en wETH (Wormhole ETH) sur Solana.

1) En utilisant un SignatureSet créé lors d'une transaction précédente, le pirate a d'abord pu contourner les gardiens de Wormhole mis en place pour vérifier les transferts entre les chaînes et appeler "verify_signatures" sur le bridge principal.

2) La fonction "verify_signatures" du contrat délègue ensuite la vérification du "SignatureSet" à un programme Secp256k1 distinct. En raison d'une divergence entre 'solana_program::sysvar::instructions' (une sorte de précompilation) et le 'solana\_program' utilisé par Wormhole, le contrat n'a pas vérifié correctement l'adresse fournie, et l'attaquant a pu fournir une adresse contenant seulement 0,1 ETH.

3) En utilisant un compte créé quelques heures plus tôt avec une seule instruction sérialisée correspondant au contrat Sep256k1, l'attaquant a pu falsifier le "SignatureSet", appeler "complete\_wrapped" et frapper frauduleusement 120k ETH sur Solana en utilisant la vérification VAA qui avait été créée dans une transaction précédente.

## Wormhole

Token bridge entre Ethereum et Solana. (initialement)

```
struct WormholeMsg {

  uint8 version;
  uint32 timestamp;
  uint32 nonce;
  uint16 emitterChainId;
  bytes32 emitterAddress;
  uint64 sequence;
  uint8 consistencyLevel;
  bytes payload;
  uint32 guardianSetIndex;
  Signature[] signatures;
  bytes32 hash;
}
```

5 payloads :
* Transfer (déclenche la libération de jetons verrouillés)
* TransferWithPayload (pareil que ci-dessus avec un payload supplémentaire spécifique au domaine)
* AssetMeta (requis avant le premier transfert, atteste les metadatas de l'actif)
* RegisterChain (enregistre le contrat bridge for une chaîne étrangère)
* UpgradeContract

```
PayloadID uint8 = 1
// Amount being transferred (big-endian uint256)
Amount uint256
// Address of the token. Left-zero-padded if shorter than 32 bytes
TokenAddress bytes32
// Chain ID of the token
TokenChain uint16
// Address of the recipient. Left-zero-padded if shorter than 32 bytes
To bytes32
// Chain ID of the recipient
ToChain uint16
// Amount of tokens (big-endian uint256) that the user is willing to pay as relayer fee. Must be <= Amount.
Fee uint256
```

## ERC 5164

Exécution cross-chain (14/06/2022)

Création d'une interface d'éxécution crosschain pour les blockchain basées sur EVM.

Permet à un contrat hébergé sur une chaîne 1 d'appeler des contrats sur une chaîne 2 en envoyant un message cross-chain.

2 composants :
* Message Dispatcher : chaîne d'origine / diffuse message via une couche de transport à un ou plusieurs contrats de type "MessageExecutor"
* Message Executor : chaîne de destination / exécute les messages "dispatchés"

```
struct Message {
  address to;
  bytes data;
}

interface MessageDispatcher {
  event MessageBatchDispatched(
    bytes32 indexed messageId,
    address indexed from,
    uint256 indexed toChainId,
    Message[] messages
  );
}
```

Sources :
  * [Article 4 PFE](https://medium.com/coinmonks/cross-chain-bridge-vulnerability-summary-f16b7747f364)
  * [DeFi Hacks Analysis - Root Cause](https://web3sec.notion.site/web3sec/ba459372dc434341b99ec92a932f98dc?v=7fceca7b3da74aa8a99b49c44a2a3916)
  * [Slowmist Hacked - Summary of blockchain attack events](https://hacked.slowmist.io/?c=Bridge)
  * [Coinbase -  Nomad bridge incident analysis](https://www.coinbase.com/blog/nomad-bridge-incident-analysis)
  * [Rekt - Le layer 2](https://rekt.news/fr/the-second-layer/)
  * [Rekt - Wormhole](https://rekt.news/fr/wormhole-rekt/)
  * [Wormhole whitepaper](https://github.com/wormhole-foundation/wormhole/blob/main/whitepapers/0003_token_bridge.md)
  * [ERC 5164, 6170](https://eips.ethereum.org/erc)
  * [ERC 5164 Thread](https://ethereum-magicians.org/t/eip-5164-cross-chain-execution/9658/13)
  * [ERC 5164 Implementation](https://github.com/pooltogether/ERC5164)

A fouiller : 

* [Ethereum - Bridges](https://ethereum.org/fr/developers/docs/bridges/)
* [Github - Blockchain bridge simplified](https://github.com/chainstack/blockchain-bridge-simplified)
* [Github - EVM Bridge](https://github.com/mineables/EVMBridge)

