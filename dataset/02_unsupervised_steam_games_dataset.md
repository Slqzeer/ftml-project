# Dataset 2 - Unsupervised learning

## Choix du dataset

Nom du dataset : Steam Games Dataset

Source : Kaggle

URL : https://www.kaggle.com/datasets/fronkongames/steam-games-dataset

Type de probleme : apprentissage non supervise

Tache principale proposee : clustering

Tache complementaire possible : reduction de dimension pour visualisation

## Description courte du dataset

Ce dataset contient des informations sur des jeux disponibles sur Steam. Il
regroupe des donnees issues de la plateforme Steam et de Steam Spy. Les
informations disponibles incluent notamment le prix, les votes positifs, les
votes negatifs, les genres et d'autres metadonnees sur les jeux.

Le dataset est adapte au projet car il permet de travailler sur une question non
supervisee naturelle : peut-on identifier des groupes de jeux Steam ayant des
profils similaires, sans utiliser de variable cible ?

## Objectif du traitement

L'objectif propose est de regrouper les jeux Steam en profils de marche.

Exemples de groupes que l'on pourrait chercher a identifier :

- jeux gratuits tres populaires ;
- jeux payants avec beaucoup d'avis positifs ;
- jeux peu connus mais tres bien notes ;
- jeux chers avec peu d'avis ;
- jeux ayant beaucoup d'avis negatifs ;
- jeux de certains genres proches en comportement utilisateur.

L'objectif n'est pas de predire une variable, mais de comprendre la structure du
catalogue Steam a partir des caracteristiques disponibles.

## Variables utilisees

Les variables exactes devront etre confirmees apres telechargement du fichier,
mais le dataset est documente comme contenant notamment :

- `price` : prix du jeu en dollars, avec `0.0` pour les jeux gratuits.
- `positive` : nombre de votes positifs.
- `negative` : nombre de votes negatifs.
- `genres` : genres du jeu.

Variables construites possibles :

- `total_reviews = positive + negative`
- `positive_ratio = positive / (positive + negative)`
- `is_free = 1 si price = 0, sinon 0`
- variables binaires de genres apres encodage multi-label
- logarithme du nombre d'avis, par exemple `log1p(total_reviews)`, pour reduire
  l'effet des jeux extremement populaires

## Questions de recherche

Questions principales :

- Peut-on identifier des groupes de jeux Steam ayant des profils similaires ?
- Les jeux gratuits forment-ils un groupe separe des jeux payants ?
- Les jeux tres populaires sont-ils proches entre eux, independamment du genre ?
- Les genres influencent-ils fortement la structure des clusters ?
- Les jeux avec beaucoup d'avis negatifs forment-ils un groupe identifiable ?

Questions secondaires :

- Le prix est-il une variable structurante ?
- Le ratio d'avis positifs separe-t-il clairement les jeux bien recus des autres
  jeux ?
- Les methodes de clustering donnent-elles des groupes interpretables ?

## Methodologie proposee

### 1. Chargement et comprehension du dataset

- Charger le dataset avec `pandas`.
- Afficher les colonnes, types, dimensions et valeurs manquantes.
- Identifier les variables quantitatives et les variables textuelles ou
  categorielles.
- Verifier la distribution du prix, des avis positifs et des avis negatifs.

### 2. Nettoyage

- Supprimer les jeux avec trop peu d'informations exploitables.
- Traiter les valeurs manquantes.
- Supprimer les doublons si necessaire.
- Filtrer eventuellement les jeux avec zero avis, car ils apportent peu
  d'information pour analyser la reception utilisateur.

### 3. Feature engineering

Variables proposees :

- `price`
- `is_free`
- `positive`
- `negative`
- `total_reviews`
- `positive_ratio`
- `log_total_reviews`
- genres encodes en variables binaires

Transformations recommandees :

- utiliser `log1p` sur les variables de volume d'avis ;
- standardiser les variables numeriques ;
- encoder les genres en multi-hot encoding ;
- eviter que le nombre d'avis domine completement le clustering.

### 4. Reduction de dimension

Methodes possibles :

- PCA pour obtenir une projection interpretable en 2D.
- t-SNE ou UMAP si l'on veut une visualisation plus lisible, mais PCA est plus
  simple a justifier.

La reduction de dimension peut etre utilisee pour visualiser les clusters et
analyser quelles variables structurent les donnees.

### 5. Clustering

Modeles a comparer :

- `KMeans`
- `AgglomerativeClustering`
- `DBSCAN`, si les donnees semblent contenir des groupes de densite differente

KMeans est un bon premier choix, mais il demande de choisir un nombre de
clusters. Ce nombre pourra etre discute avec :

- methode du coude ;
- silhouette score ;
- interpretation metier des clusters.

### 6. Evaluation

Comme il n'y a pas de cible, l'evaluation doit etre adaptee au non supervise.

Metriques possibles :

- silhouette score ;
- Davies-Bouldin score ;
- Calinski-Harabasz score.

Mais l'evaluation ne doit pas etre seulement numerique. Il faudra aussi regarder
si les clusters ont du sens :

- prix moyen par cluster ;
- nombre moyen d'avis par cluster ;
- ratio moyen d'avis positifs ;
- genres les plus frequents ;
- exemples de jeux dans chaque cluster.

### 7. Interpretation

Pour chaque cluster, produire une courte description :

- taille du cluster ;
- profil typique des jeux ;
- prix moyen ;
- niveau de popularite ;
- reception utilisateur ;
- genres dominants ;
- interpretation possible.

Exemple de conclusion attendue :

```text
Le cluster 0 regroupe principalement des jeux gratuits ou tres peu chers, avec
un nombre d'avis eleve et un ratio positif moyen. Il peut representer des jeux
accessibles et populaires.
```

## Resultat attendu

Le resultat attendu est une analyse non supervisee coherente :

- des features construites proprement ;
- une justification du choix des variables ;
- une comparaison de plusieurs methodes ou plusieurs nombres de clusters ;
- une evaluation adaptee ;
- une interpretation des groupes obtenus.

## Limites identifiees

- Les donnees Steam evoluent vite : prix, avis et popularite peuvent changer.
- Le nombre d'avis peut dominer les autres variables si les transformations ne
  sont pas adaptees.
- Les genres peuvent etre multiples et parfois peu normalises.
- Le clustering depend fortement du preprocessing.
- Un bon score de silhouette ne garantit pas toujours une interpretation
  interessante.

## Statut (mise a jour du 5 juillet 2026)

Le traitement est implemente et execute dans
`reponse/exercice_07_unsupervised_learning_application/`.

- Attention: l'en-tete du fichier `dataset/steam_game_dataset.csv` est
  defectueux: les colonnes `Discount` et `DLC count` sont fusionnees en
  `DiscountDLC count` (39 noms pour 40 champs), ce qui decale toutes les
  colonnes d'un cran si on le lit tel quel. Le notebook repare l'en-tete au
  chargement.
- Les variables prevues sont confirmees presentes: `Price`, `Positive`,
  `Negative`, `Genres` (genres separes par des virgules). 125 855 jeux au
  total, 82 956 conserves apres filtrage des jeux sans aucun avis.
- Resultats: KMeans avec k = 6, retenu par le silhouette score (0.186, evalue
  sur un sous-echantillon de 10 000 jeux) et le Davies-Bouldin (1.60).
  Clusters interpretables: longue traine indie payante (48 015 jeux), hits
  payants etablis, Early Access, Free To Play, Massively Multiplayer (segment
  le moins bien note, ratio positif 0.64), Racing.

## Texte court a envoyer au professeur

Pour l'exercice non supervise, nous proposons d'utiliser le dataset "Steam Games
Dataset" disponible sur Kaggle :
https://www.kaggle.com/datasets/fronkongames/steam-games-dataset

Ce dataset contient des informations sur des jeux Steam, notamment le prix, les
votes positifs et negatifs, les genres et d'autres metadonnees. L'objectif serait
d'effectuer un clustering afin d'identifier differents profils de jeux Steam :
jeux gratuits populaires, jeux payants bien notes, jeux de niche, jeux avec une
reception plus negative, etc. Les features utilisees seraient le prix, un
indicateur free-to-play, le nombre total d'avis, le ratio d'avis positifs et les
genres encodes. Nous utiliserions des methodes comme PCA pour visualiser les
donnees, puis KMeans ou clustering hierarchique pour construire et interpreter
les groupes.

