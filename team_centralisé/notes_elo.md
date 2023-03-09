1) Access to hot/cold wallet with stolen private key
	Exemple: BitMart
	Source: https://support.bitmart.com/hc/en-us/articles/4411998987419-BitMart-Security-Breach-Update
	Solution utilisée : Remplacement des adresses de déposit et mise à jour des clefs

2) Compromised system/servers 
	Exemple: Bitpoint


3) Data leak (private key) 
	Exemple : KuCoin
	Source : https://www.kucoin.com/fr/news/en-kucoin-ceo-livestream-recap-latest-updates-about-security-incident
	Solution utilisée : Abandon des hot wallet


4) Malware/Phising 
	Exemple: Bitstamp
	Source: https://www.reddit.com/r/Bitcoin/comments/3bpdb4/bitstamp_incident_report_22015/
	Solution utilisée : Ne pas ouvrir des documents word provenants de sources inconnus (le doc contenait un script VBScript qui téléchargeait un malware)

5) Vulnerability in protocol
	Exemple: Balancer
	Source: https://cryptopotato.com/rising-defi-protocol-balancer-loses-500000-to-hacker-in-pool-exploit/
	
6) Bugs and Re-entrancy attack 
	Exemple: Lendf.me, Uniswap
	Mots clefs : Reentrancy attacks, token ERC777
	GitHub exploit : https://github.com/OpenZeppelin/exploit-uniswap
	Source: https://www.zdnet.com/article/hackers-steal-25-million-worth-of-cryptocurrency-from-uniswap-and-lendf-me/
	Solution utilisée : 

7) Suspected trusted insider 
	Exemple: CoinSecure
	Source: https://securityaffairs.com/71322/hacking/coinsecure-hacked.html

8) Phishing data on fake site
	Exemple: LocalBitcoins
	Source: https://www.reddit.com/r/localbitcoins/comments/ak1u8m/localbitcoins_report_on_the_security/
	Attaque : Redirection des utilisateurs vers un site de phising

9) Server DNS compromised 
	Exemple: EtherDelta
	Source: https://www.ccn.com/cryptocurrency-exchange-etherdelta-hacked-in-dns-hijacking-scheme/

Source vulnérabilités : https://resources.infosecinstitute.com/topic/security-vulnerabilities-of-cryptocurrency-exchanges/

=============================================================================================

https://learn.bybit.com/glossary/definition-centralized-exchange-cex/

Méthode utilisée pour les échanges : Order Book Method
"Order book records the entire open orders to enable buying and selling of assets to traders. If a person wants to buy a particular asset, he must disclose the asset’s estimated price to the middle man involved in the exchange process.
Once that middleman finds someone whose request matches the buyer, it swaps the assets and completes the exchange between them. Order books have disadvantages, such as long waiting time for the exchange to be done and traders’ inability to cross-verify the transaction."

Procédure échange centralisé
- Enregistrement des info avec KYC (know your customer)
- L'utilisateur A dépose la somme qu'il veut échanger dans un portemonnaie
- Un nombre équivalent de IOU est créé
(IOU remplace les smart contracts ici)
- Le CEX échange les IOU contre la monnaie souhaitée lors d'un retrait ou un échange

https://freewallet.org/blog/what-is-an-iou/
Qu'est-ce qu'un IOU ?
IOU = I own you
Ce sont des toxens, qui permettent de quantifier la dette de l'utilisateur envers le CEX (genre de contrat non légal) donc même si la monnaie crypto prend de la valeur, la montant en monnaie fiduciaire ne change pas .

Points négatifs:
- Le CEX peut geler ou bloquer les fonds/porte-monnaies à tout instant
- Cible d'attaques car les CEX gèrent/conservent l'argent
- Pas assez transparents sur le fonctionnement interne, possibilités d'arranger les prix des monnaies ou de manipuler le marché
- Frais de transaction

https://www.leewayhertz.com/exchange-vs-dex-vs-swap
![](https://codimd.romaintestud.fr/uploads/upload_d4a2d97d6d840791336f826336f575ad.png)

10 modèles simplifiés des échanges centralisés:
https://medium.com/intotheblock/10-patterns-of-centralized-crypto-exchanges-explained-using-machine-learning-and-data-b386d913832

=============================================================================================

Notes sur le bridge entre Solana et Ethereum



https://github.com/wormhole-foundation/wormhole
Github implémentation du protocol Wormhole

Qu'est-ce qu'un porte-monnaie Ledger ?
- Porte-monnaie matériel vendu par la companie Ledger
- Conserve les clefs privées de l'utilisateur en hors-ligne
    - Elles sont nécessaires pour les transactions de cryptomonnaie
    - Evite les vols
- Sur une clef USB

https://docs.wormhole.com/wormhole/portal-examples/evm-to-solana
https://docs.wormhole.com/wormhole/vaas
Explication du "portal token bridge" Ethereum vers Solana
- xDapps = Applications décentralisées cross chain
    - Permet de déplacer des tokens d'une chaine vers une autre
    
- Procédure : 
    - Récupérer l'adresse Solana à l'aide d'imports
    - Envoi d'un message de transfert à Ethereum
        - Valeurs retournées : numéro de séquence et l'adresse en bytes du token bridge de Ethereum (ces dernières seront utilisées pour la création du VAA)
    - Création d'un Verifiable Action Approval (VAA) (preuve de consensus) signé par les "gardiens" (19 nodes avec Wormhole)
    Entête du VAA:
        > byte        version                  (VAA Version)
        > u32         guardian_set_index       (Indicates which guardian set is signing)
        > u8          len_signatures           (Number of signatures stored)
        > [][66]byte  signatures               (Collection of ecdsa signatures)
        
        Corps du VAA:
        > u32         timestamp
        > u32         nonce
        > u16         emitter_chain
        > [32]byte    emitter_address
        > u64         sequence
        > u8          consistency_level
        > []byte      payload 
    - Vérifier les signatures et création d'un compte de récupération
    - Envoi du VAA à Solana 
    - Solana vérifie les signatures et envoie l'adresse du bridge
    - Récupération des tokens
    >  // Finally, redeem on Solana
    > const transaction = await redeemOnSolana(
    >   connection,
    >   SOL_BRIDGE_ADDRESS,
    >   SOL_TOKEN_BRIDGE_ADDRESS,
    >   payerAddress,
    >   signedVAA,
    >   isSolanaNative,
    >   mintAddress
    > );
    > const signed = await wallet.signTransaction(transaction);
    > const txid = await connection.sendRawTransaction(signed.serialize());
    > await connection.confirmTransaction(txid);

Explications du contenu/fonction du VAA et du transfert Ethereum->Solana plus détaillées (code):
https://medium.com/coinmonks/how-do-cross-chain-bridges-work-a-case-on-wormhole-part-1-37c8de2cf2e4
Bridge relayers envoient les messages et récupèrent les frais

Partie sur comment vérifier les VAAs:
/!\Partie sur Solana obsolète voir attaque wormhole/!\ 
https://medium.com/coinmonks/how-do-cross-chain-bridges-work-a-case-on-wormhole-part-2-79acc87e3c88


https://medium.com/coinmonks/how-do-cross-chain-bridges-work-a-case-on-wormhole-part-3-9783f0c880e8
Ether = 18 decimals alors que max Wormhole = 8 decimals donc si >8 alors montant *= 10 ** (decimal-8) lors de la normalisation/dénormalisation et les différences (poussières) sont remboursées à l'envoyeur.

https://medium.com/coinmonks/how-do-cross-chain-bridges-work-a-case-on-wormhole-part-4-d68fedc03a4f
VAA Replay (la notion d'utiliser un VAA plusieurs fois)
Comment éviter ?
Solara:
Compte non initialisé "claim" relié à une PDA (Program Derived Addresses) prevenant du VAA et déterminée par l'adresse et la chaîne de l'éméteur ainsi que la séquence de la transaction
Le compte a un drapeau comme un booléen qui pour savoir s'il est initialisé ou non
Chaque VAA a une unique combinaison et par conséquent un compte "claim" unique à usage unique
Ethereum:
Map qui contient tous les hashs (hash des bytes du VAA sans les signatures et de type keccak256) des VAAs des transactions complétées
Comment savoir ? Hash est "marqué" (true)


Ressources sur les rollups bridges:
https://open.harmony.one/harmony-project-highlights/trustless-ethereum-bridge-ganesha-upadhyaya
https://github.com/yoavw/cross-rollup-bridge/blob/master/README.md


https://docs.wormhole.com/wormhole/core-concepts/portal-payloads
Portal Payloads
- Les tokens du pont Wormhole sont agnostiques (cad non spécifiques à une certaine blockchain et donc compatibles avec plusieurs blockchains)
- Constituer des deux payloads
     1) La chaîne A verrouille les tokens et envoyer un message (message de transfert) à B pour confirmer le verrouillage
     2) La chaîne A envoie également des métadonnées d'une addresse permettant à la chaîne B de connaitre l'adresse où se situent les tokens

https://github.com/mineables/EVMBridge
EVM Bridge utilise des contracts intelligents et un middleware centralisé pour transferer des tokens
Explication/Démonstration du middleware ici : https://github.com/mineables/EVMBridge/blob/master/test/testBridgeable.js

A développer:
Le trilemme de l'interopérabilité (lié aux bridges Ethereum)
https://blog.connext.network/the-interoperability-trilemma-657c2cf69f17
Résolu grâce aux bridges optimistes:
https://blog.connext.network/optimistic-bridges-fb800dc7b0e0

