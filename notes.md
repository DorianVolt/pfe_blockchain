
# Résumé articles PFE

Je vais pas faire de résumé ultra précis avec toute les maths des articles ca serait useless (et surtout ca prend du temps à comprendre à 100%)
Plusieurs modes de gouvernance sont possibles :

- Un mode « **ouvert** » (tout le monde peut lire et écrire les registres de la chaîne). Dans ce cas, en règle générale, la loi applicable à la chaîne est la loi (code algorithmique) désignée par les parties.
- Un mode « **semi-fermé** » (seul un organisme central peut écrire, mais l'accès en lecture est plus libre). Cela peut être utilisé pour les fonctions dévolues aux États (cadastres…) ou aux institutions gérant une donnée sécurisée (traçabilité alimentaire…). Dans ce cas, les règles sont plus libres, l'organisme central ayant la main sur les aspects techniques de validation de la Blockchain.
- Un mode **fermé** (seul un organisme central peut écrire, personne ne peut lire sauf cet organisme). Dans ce cas, l'intérêt réside dans la robustesse théorique et la traçabilité du processus, qui n'a pas besoin d'être (ou ne doit pas être) public, mais qui a besoin de cette sécurité. À noter que dans ce cas, il reste vulnérable à une attaque des 51 %, du fait de la non-décentralisation, et de la non-publication.

Chaque bloc de la chaîne est constitué des éléments suivants :

- plusieurs transactions ;
- une somme de contrôle (« hash »), utilisée comme identifiant ;
- la somme de contrôle du bloc précédent (à l’exception du premier bloc de la chaîne, appelé bloc de genèse) ;
- une mesure de la quantité de travail qui a été nécessaire pour produire le bloc. Celle-ci est définie par la méthode de consensus utilisée au sein de la chaîne, telle que la « preuve de travail », ou « preuve de participation »

## Article 1

Exemple de swap atomique:
Alice souhaite acheter une Cadillac à Carol contre des BTC. Carol vend une Cadillac contre des ETH. Bob echange des BTC contre des ETH.
1. Alice réalise un contrat en bloquant les BTC sur la blockchain à destination de Bob. Le contrat est associé au hash un secret $s$ tel $h = H(s)$ et périme à une date $t$. Seule Alice connait $s$
2. Bob attend le contrat de Alice, et bloque ses ETH sur la blockchain en destination de Carol et y associe le $h$ et le $t$ du contrat de Carol.
3. Carol attend que le contrat de Bob soit émis. Elle réalise ensuite un contrat en bloquant le certificat de possession de la Cadillac sur la blockchain des certificats de voiture (imagine que ca existe) à destination d'Alice. Elle associe le contrat au même $h$ et même $t$ que celui d'Alice. (Carol ne connait pas encore $s$, elle copie betement la valeure)
4. Alice attend de voir le contrat de Bob et lui rèvele son secret $s$. Ceci à pour effet de débloquer le contrat. Alice obtient le certificat de la Cadillac
5. Carol revèle le secret à Bob. Cela débloque la transaction de ETH vers Carol
6. Bob revèle le secret à Alice. Cela débloque la transaction de BTC vers Bob.

Aucun parti n'a d'interet a ne pas suivre le protocole.
Si Carol ne partage pas son secret, alors Alice à bien obtenue la Cadillac, Bob est remboursé puisque la transaction échoue. Alice est remboursé puisque la transaction échoue. Seule Carol à perdu.

Le swap peut aussi avoir un interet autre que le partage entre blockchain. C'est le partage de valeur au sein d'une blockchain (permettant par exemple la mise en place d'une fragmentation de la blockchain https://ethereum.org/fr/upgrades/sharding/).

Un swap atomique guaranti que:
- Si toutes les parties prennent place dans le protocole alors tout les échanges ont lieu
- Si une partie dévie alors elle est la seule à en subir les conséquences
- Il l'y a pas que coalition qui a pour intention de dévier du protocole

Une blockchain est un système **distribué** qui permet de aux clients de publier publiquement des transactions dans un registre lisibible publiquement et résistant aux altérations.

Smart contract: Etablissement d'une transaction avec la plupart du temps une valeur d'échange, un destinataire, une source et un timer de révocation au cas où la transaction serait trop longue. Une fois un contract rendu publique il est **irrévocable**.

Certaines parties peuvent etre **rationnelle**: agir en coalition ou pas dans leur profit personnel. Ou bien **irrationnelle**: agir a l'encontre du protocole même si ca leur est défavorable.

Voici les résultats pour une partie v, organisée en classes, Pour
bref, chaque classe a un nom abrégé.
- La partie acquiert des actifs sans payer : au moins un arc l'entrée de v est déclenchée, mais aucun arc sortant de v n'est déclenché (**FreeRide**).
- La partie acquiert des actifs en payant moins que prévu : tous les arcs entrant dans v sont déclenchés, mais au moins un arc sortant v n'est pas déclenché (**Discount**).
**Par exemple**: Alice partage le secret, Carol le partage ensuite a Bob. Bob ne le partage jamais à Alice. Alice possède la Cadillac, Carol obtient les ETH, Bob ne récupère jamais les BTC. 
- La partie échange les actifs comme prévu : tous les arcs entrant et laissant v sont déclenchés (**Deal**).
- Aucun actif ne change de main : aucun arc entrant ou sortant v n'est déclenché (**NoDeal**).
- La partie paie sans acquérir tous les actifs attendus : au moins un arc entrant dans v n'est pas déclenché, et au moins un l'arc sortant de v est déclenché (**Underwater**).

Un protocole d'échange est **uniforme** si toute les parties qui suivent le protocole finissent dans l'état **Deal**. Si des parties dévient du protocole alors aucune partie qui suivi ce dernier ne fini dans l'état **Underwater** 

Un protocole est **atomique** si il est uniforme et a un equilibre fort de Nash 

Le protocole de l'article se décompose en deux phases:
- La création et la propagation des contracts dans la chaine
- La propagation dans le sens inverse des hash des secrets (qui constituent les transactions en elles même si j'ai bien compris)

Le protocole est décentralisé mais soutient tout de même une notion de leader et follower ce que je trouve un peu contre intuitif.

## Article 2
On retrouve la même notion de suiveurs et déviants du protovole.

Le protocole s'éxécute en 5 phases:
- Phase de **découverte** des participants
- Phase de **démonstration** des assests
- Phase de **transfert** des ownerships selon ledeal
- Phase de **validation** du deal, chaque partie check si le deal est est acceptable
- Phase de **commit**, déscision si le deal doit rester permanent ou bien etre revert

Ce protocole suppose un modèle de réseau synchrone où le temps de propagation de la blockchain est connu et borné.
 Les étapes du protocole restent les mêmes malgrès le fait qu'il soit améloiré au cour de l'article (CBC, timelock,etc...)

Vulnérabilité au DDoS si le delta du timelock est trop petit. La principale différence de performances dans le timelock et Les protocoles CBC sont le nombre de signatures qui doivent être vérifié pour compléter le protocole, nous devons donc considérer n parties vs. f + 1 validateurs



## Article 3
Un attaquant a réussi a forger une preuve en cassant l'implémentation de Binance. Ca lui a permis de faire des fausses transaction pour son compte.

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
## Article 4
Gros rassemblement de failles et d'attaque sur des bridges de crypto (l'article 5 est mentionné). En gros les bridge de crypto se font beaucoup attaquer parce qu'il représente une surface d'attaque très grande et plus simple a exploité qu'un blockchain elle même. En plus le fait de violer un protocole de blockchain ne profite pas a la partie attaquante d'ou le fait qu'ils choissisent les bridges plutot que les implémentations directes des protocoles de blockchain.
## Article 5
Worhmhole est un intermédiaire qui permet de transférer une crypto vers une autre.
L'attaque a permis a l'attaquant de récupérer 120.000 ETH en equivalent wETH (un fork de ETH). L'entreprise lui a offert un contract de 10% de la somme en échange des quels ils ne le poursuivraient pas en justice.
L'attaque provient d'un commit défectueux du 13 Janvier. La chose bizarre c'ets que l'exploit a été effectué **après** le commit.
La fonction qui a été changée ne s'occupaient pas de checker l'intégrité des smarts contracts mais plutot la fait que le check d'intégrité soit vérifié.
L'attaquant a pu spoofer une adresse d'un guadrian (une entité qui vérifie les transaction et les valide ou pas) et ansi valider la transaction de 120k ETH.
# Résumé

Les protocoles décentralisés sont pllus complexes et théorique que les centralisés mais sont bien plus fiables.

## Article "A Survey on Blockchain Interoperability: Past, Present, and Future Trends" (https://doi.org/10.1145/3471140)
Meta-analyse de 280 papiers de recgerches et 120 documents "amateurs" sur l'interopérabilité des blockchains. Dispo gratuitement sur la BU de la FAC.  
Parle de l'interoparibilité entre les blockchain qui peut etre considéré comme une généralisation du swap de crypto.  
Beaucoup de bonnes idées pour une intrdouction au sujet.  
Il présente ente autre un graphique présentant l'interet porté par la recherche sur l'intéroperabilité en illustrant un passage de 2 papiers de rechers en 2016 à 208 en 2020.

On classifie les types d'echange en fonction de la relatioon entre la blockchain source et la blockchain cible.
Si nous somme dans deux chainages du meme systeme de blockchain (blockchains homogènes), nous avons un "cross-chain" (ex: ETH -> USDT).  
Si nous somme dans un systeme de blockchains hétérogène ou hybride ont réalise un echange de type "cross-blockchain".  
NOTE: J'ai quelques intérogations sur la nuance entre blockchains homogènes/hétérogènes/hybrides. L'auteur de l'article parle des système homogènes avec comme exemple deux blockchain ERC20. ET il parle ensuite de système heterogènes comme  etant entre blockchain publqiue et a consortium. (Ce qui correspond a ce que je trouve sur d'autres sources). la question est donc où se placent les systèmes "hybrides". Càd entre différentes blockchains publique (BTC - ETH). Est-ce du Cross-chain ou du Cross-blockchain ?  
La méta analyse classe les types d'interopérabilité suivant les questions suivantes: "Qui ? Quoi ? Où ? Quand ? Comment ?"
- Le "Quoi" se refère aux types de données echangés (Fobgibles, non fongibles, valeurs marchandes, données, ...)
- Le "Qui" determine les acteurs de la blockchain (Utilisateur final (BTC/ETH), un consortium, un tiers de confiance).
- Le "Où" vise à determiner le support sur lequel se place la transaction. (blockchain publique, privée, systèmes centralisés (ex: VISA), transaction off-chain, ...). Il vise aussi determiner la présence ou non d'oracle (expert dans le cas d'assurance par exemple).
- Le "Quand" se refère à l'ensembles des processus qui vont être éxécutés définits dans le "runtime" de l'opération.
- Le "Comment" (c'est compliqué bref)
