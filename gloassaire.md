# Glossaires
Ce document est là pour définir les termes courants relatif au sujet du PFE.
Sources:  
- https://medium.com/coinmonks/decentralised-applications-heterogeneous-blockchains-4bbebb0b6eeb
- https://www.ibm.com/fr-fr/topics/hyperledger (consortium blockchain)
- https://www.geeksforgeeks.org/offchain-vs-sidechain-vs-statechannel-in-blockchain/
## Vocabulaire de la blockchain
- Blockcchain Publique (en: Permissionless): blockcchain ouverte qui octroie a quiconque un droit de lecture et permet de recevoir une transaction de n'importe quel utilisitauer valide. Elle est completement décentralisé et peut impliquer un algorirthme de consensus. (ex.: Bitcoin, Ethereum)
- Blockchain de Consortium (en: Permissioned): blockchain "semi-decentralisé" dont tout le monde à un pouvoir de lecture mais seul des noeuds définits possède un pouvoir de décision sur le réseau. (ex.: Hyperledger Fabric)
- Blockchain Privé: blockchain centralisé et fermé visant à gerer une base de données internes à une organisation.
- Blockchains hétérogènes: Pair de blockchain publique et à consortium (ex.: Hyperledger Fabric - BTC)
- Blockchains hybrides: Pair de blockchain publique. (ex.: ETH - BTC) 
- Blockchains homogènes: Pair de blockchain publique basé sur la même blockchain. (ex.: ETH - USDT (ce ssont des EVM-based blockchains)). 
- Off-chain transaction: Transaction concerant la blockchain mais réalisé sur une couche supérieur permettant un niveau d'abstraction visant à aggréger les transactions. Ceci à pour effet de réduire les coûts rendant le réseau plus scalable en dépit de sa résilience. (ex: Lightning Network au dessus de Bitcoin)