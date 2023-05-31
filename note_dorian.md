# Notes Dorian

## Références

- [x] https://arxiv.org/pdf/2209.03907.pdf 
- [x] https://pantos.io/en
- [x] https://lightning.network/lightning-network-paper.pdf
- [x] https://dspace.mit.edu/bitstream/handle/1721.1/119736/1078689077-MIT.pdf?sequence=1&isAllowed=y --> Très bon
- [x] https://lightning.engineering/posts/2017-11-16-ln-swap/
- [x]  https://stakenet.io/Whitepaper_Stakenet_V3.0_EN.pdf -> Corpo
- [x] https://medium.com/stakenet/lightning-network-and-atomic-swaps-f24eb4996eb9
- [x] https://github.com/decred/dcrdex
- [ ] https://ieeexplore.ieee.org/document/8794500

## Définitions
Les swaps « on-chain » qui ont lieu entre deux cryptomonnaies sur deux blockchains différentes ; et les swaps « off-chain » qui ont lieu dans les canaux de seconde couche d’une blockchain principale, comme ceux du Lightning Network par exemple. 

connecteur publique = bureau de change a hauteur d'individu

P2pool -> Pool de minage ne pair a paire et donc décentralisé qui permet d'éviter d'utiliser un opérateur central et ainsi se prévenir du DoS
Réseau Lightning similaire à Tor

Les HTLC (Hashed Time-Lock Contracts) sont un mécanisme utilisé dans les blockchains décentralisées pour sécuriser les transactions entre deux parties qui ne se font pas confiance. Les HTLC permettent de garantir que le transfert de fonds se fait de manière sécurisée et sans risque de fraude ou d'abus.
## Réseau Lightning
Le  Lightning Network est une couche de protocole de paiement construite au-dessus de la blockchain  Bitcoin qui vise à accélérer les transactions  Bitcoin, à réduire les coûts et à améliorer l'évolutivité.
Il permet aux utilisateurs de créer des canaux de paiement peer-to-peer bidirectionnels  pour effectuer des transactions en dehors de la blockchain principale, permettant des transactions plus rapides, moins chères et plus privées. 
Les transactions sur le Lightning Network sont effectuées avec des contrats intelligents qui permettent aux utilisateurs de transférer des fonds à des tiers sans l'approbation de la blockchain principale. Les canaux de paiement flash sont créés en verrouillant temporairement des fonds sur une adresse multisignal, qui est ensuite utilisée pour envoyer des transactions à d'autres participants du réseau. 
Lightning Network utilise un système de routage pour acheminer les paiements entre les participants, en utilisant les canaux de paiement existants pour créer des itinéraires optimaux entre les participants. Les frais d'utilisation de Lightning Network sont généralement bien inférieurs aux frais de transaction sur la blockchain principale, ce qui en fait une option plus attrayante pour les  petites et moyennes transactions.

## Notes papier MIT

### Contexte
Amélioration du protocole Lightning de Bitcoin qui va permettre de faire de l'échange cross chain.
Le 19 septembre 2017, le projet Decred a mené avec succès la premier échange atomique en chaîne entre Decred et Litecoin.
Peu de temps après, le 7 octobre 2017 Altcoin.io a compléter le premier échange atomique entre ethereum et bitcoin, deux jetons sur différentes blockchains.
Cette réalisation a marqué une autre étape avancée dans la décentralisation des transactions inter-chaînes, mais le processus était encore "on chain", ce qui signifie que tout échange entraînerait le mining et la congestion dans le système, en particulier à grande échelle.
Le MIT va se baser sur une amélioration d'un fork du réseau Lightning appelé "lit".
Ils essayent de contrer le problème de grande échelle grace a leurs améliorations.
Ils veulent mettre en place cette solution pour des blockchains qui n'ont pas de valeurs monétaire (informations, NFT,etc...)
### Leur solution aux problèmes
Leur programme met en place la capacité d'effectuer des transactions sur différentes blockchains, et permettre aux utilisateurs d'échanger du LTC contre BTC et tout autre jeton blockchain qui sont rendus compatibles avec le système.
Les HTLC sont des mécanismes de coffre-fort qui maintiennent les changements d'état globaux unilatéraux sur plusieurs nœuds en utilisant des secrets hachés pour déverrouiller les fonds stockés (ils agissent comme des garanties en cas de non respect du contract comme le veut l'atomic swap)
![](https://md.dorianvolpe.fr/uploads/upload_f9db7fd158aec5eada2c0f0b375b5071.png)

Leur programme rajoute 4 commandes au réseau Lightning mentionné précédemment:
- Price:
    - Commande de canal simple qui permet aux utilisateurs de rechercher la valeur marchande actuelle en USD de toute crypto-monnaie disponible sur le site coinranking.com. L'utilisateur fournit le nom de la devise et le système se connecte au site de classement pour récupérer les prix du marché continuellement mis à jour. Si le nom de la devise existe, le programme imprime les prix sur le terminal de l'utilisateur et se déconnecte du site.
- Compare
    - La commande nécessite que l'utilisateur fournisse les noms de deux devises. Le système utilise alors la fonctionnalité de la commande "Prix" pour récupérer la valeur marchande actuelle de chaque devise souhaitée et effectue une simple opération arithmétique pour retourner la valeur comparée d'une unité de la première devise correspondant à la quantité correspondante d'unités de la deuxième devise (sans les frais inclus).
- Exchange
    - La commande Exchange est la première des deux commandes développées pour effectuer des échanges inter-chaînes. La commande demande à l'utilisateur d'approvisionner le canal avec la première devise souhaitée, un montant pour l'initiateur pour envoyer sur ce canal, le canal avec la deuxième devise souhaitée, et un montant que l'accepteur doit envoyer sur ce canal. Cette commande agit comme une requête. Si Alice veut échanger du Bitcoin pour du Litecoin avec Bob, elle entrerait "échange 1 10 000 000 2 1 000 000 000 ».
    - ![](https://md.dorianvolpe.fr/uploads/upload_e58d831a23e0646fce5c4729e5049998.png)

- Respond
    - À ce stade, Bob a une minute pour décider si pour accepter la demande d'échange d'Alice. Il peut faire l'une des trois choses suivantes : répondre oui et accepter l'échange, répondre non et le refuser, ou laisser la demande temps libre.

## Limitations et applications
Pas de support de channel hopping donc beaucoup de création de channel ce qui epmpeche (pour le moment) son usage grand public.
Pas de check pour les chaines non monétaires 
On peut utiliser cela pour avoir des payment dans n'importe quel cryptos pour des bien réeel grace a la rapidité du réseau Lightning


## Leur repo:  https://github.com/jmathus/lit/tree/HTLCswaps


