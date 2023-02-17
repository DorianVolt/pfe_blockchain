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

Sources :
  * [Article 4 PFE](https://medium.com/coinmonks/cross-chain-bridge-vulnerability-summary-f16b7747f364)
  * [DeFi Hacks Analysis - Root Cause](https://web3sec.notion.site/web3sec/ba459372dc434341b99ec92a932f98dc?v=7fceca7b3da74aa8a99b49c44a2a3916)
  * [Slowmist Hacked - Summary of blockchain attack events](https://hacked.slowmist.io/?c=Bridge)
  * [Coinbase -  Nomad bridge incident analysis](https://www.coinbase.com/blog/nomad-bridge-incident-analysis)
  * [Rekt - Le layer 2](https://rekt.news/fr/the-second-layer/)
  * [Rekt - Wormhole](https://rekt.news/fr/wormhole-rekt/)

A fouiller : 

* [Ethereum - Bridges](https://ethereum.org/fr/developers/docs/bridges/)
* [Github - Blockchain bridge simplified](https://github.com/chainstack/blockchain-bridge-simplified)
* [Github - EVM Bridge](https://github.com/mineables/EVMBridge)
* [Ethereum - ERC 20, 5164 & 6170 cross-chain](https://eips.ethereum.org/erc)
* [Chain Agnostic Improvement Proposals](https://chainagnostic.org/CAIPs/caip-1)
* [Github - Chain Agnostic Improvement Proposals](https://github.com/ChainAgnostic/CAIPs)

