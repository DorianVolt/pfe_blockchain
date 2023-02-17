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
