Auteur: Amaury  
Derniere mise à jour: 15/02/23

# Article "A Survey on Blockchain Interoperability: Past, Present, and Future Trends" (https://doi.org/10.1145/3471140)

## Rapide résumé

Ce document est une meta-analyse de 280 papiers de recgerches et 120 documents "amateurs" sur l'interopérabilité des blockchains. Dispo gratuitement sur la BU de la FAC.  
Parle de l'interoparibilité entre les blockchains qui peut etre considéré comme une généralisation du swap de crypto. (Si il y a intéropérabilité il y a swap de crypto possible)  
Beaucoup de bonnes idées pour une intrdouction au sujet.  
Il présente entre autre un graphique présentant l'interet porté par la recherche sur l'intéroperabilité en illustrant un passage de 2 papiers de rechers en 2016 à 208 en 2020.

## On rentre dans le fond (en cours)

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
