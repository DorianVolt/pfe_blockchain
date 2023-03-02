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
