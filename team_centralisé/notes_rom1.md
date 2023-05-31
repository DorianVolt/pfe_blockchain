# Notes team centralisé
## Web3sec
https://web3sec.notion.site/web3sec/Web3-Security-ddaa8bf9a985494dbaf70d698345b899  

### Résumé
Groupe de recherche et de partage d'informations sur la sécurité du web3.  
Base de données d'incidents avec sources ( + Github de reproduction des incidents)  
Incidents sur des bridges : 
- Li.Fi
    - api pour creer un lien entre des blockchains
    - vulnérabilité dans les smartcontract
    - 600k volé sur 29 wallet (remboursés)
    - bug fixé
    - exploit sur la fonctionnalité de swap avant bridge (?)
    - https://twitter.com/lifiprotocol/status/1505738407938387971
    - https://blog.li.fi/20th-march-the-exploit-e9e1c5c03eb9
- Meter
    - exploit :
        - methode deposit() utilisée avec wrapped token (?)
        - le Handler a une condition spéciale qui présume que si c'est wrapped, alors les assets sont déja transférés
    - erreur d'implémentation
    - methode depositEth() implémentée par meter, mais la methode deposit() originale est quand meme dispo
- Qubit Finance
    - voir au dessus
- Multichain (Anyswap)
    - https://medium.com/zengo/without-permit-multichains-exploit-explained-8417e8c1639b
    - https://twitter.com/PeckShieldAlert/status/1483363515411099651
    - exploit : get eth avec une signature non valide
- Poly network
    - 
- Chainswap

### DeFiHackLabs
Github de reproduction des incidents (DeFiHackLabs) : 
https://github.com/SunWeb3Sec/DeFiHackLabs
- POCs
- Contrats
- Reprend la bdd

### DeFiVulnLabs
https://github.com/SunWeb3Sec/DeFiVulnLabs
Apprentissage des vulnarabilités communes des smartcontracts

==========================================================================================

### https://aucoindubloc.com/qu-est-ce-qu-un-bridge-pont-crypto-et-comment-ca-fonctionne/
Bridge -> echange entre blockchains 
centralisé car on doit faire confiance au bridge

étapes : 
- envoi des cryptos dans un smartcontract
- le smart contract verouille les crypto dans la blockchain source (pour ne pas les utiliser sur les 2 blockchain)
- copie du smartcontract sur la blockchain destination (oracle blockchain = verification)

frais sur les transferts
/!\ a la fiabilité du bridge

Oracle en crypto : "pont entre la blockchain et le monde réel"
Protocole qui transmet des informations fiables et vérifiées par le biais d'interaciton avec les smart contracts
Execution en fonction des condifitons fixés


### https://ethereum.org/en/bridges/

catégories de bridges : trusted et trustless

Trusted bridges : 
- dépend d'une entité centrale / d'un système dédié aux opérations
- soit disant sur, on se base sur l'opérateur réputation/fiabilité
- les utilisateurs donnent le controle de leurs cryptos

Trustless bridges : 
- utilise des Smartcontracts et des algorithmes
- trustless : la sécu = sécu de la blockchain rattachée (https://blog.connext.network/the-interoperability-trilemma-657c2cf69f17 check ça)
- grace aux smart contracts les utilisateurs gardent le controle de leur cryptos

Risques : 
- Smart contracts : erreurs d'implementations, etc
- risques technologiques : bugs, crash, attaques etc

Pour les trusted bridges : 
- censure : l'autorité peut empecher les users de transferer des cryptos
- vol d'argent 

Top des attaques sur des bridges : 
https://rekt.news/leaderboard/

Stratégies de secu cross-chain : 
https://debridge.finance/blog/10-strategies-for-cross-chain-security/

### https://arxiv.org/pdf/2204.08664.pdf
Papier sur l'utilisation de CEX vs DEX conclusion : 
- CEX meilleure ergonomie (pensé grand public plus commercial etc)
- CEX frais d'utilisation plus bas que DEX ----> Faux ???
- CEX controlés par les gouvs, moins de confidentialité que DEX
- Dans l'exemples, ce sont chercheurs chinois qui acceptent que leur onfidentalité soit compromise par les CEX
