# Dataset 1 - Supervised learning

## Choix du dataset

Nom du dataset : Global Video Game Sales and Ratings

Source : Kaggle

URL : https://www.kaggle.com/datasets/thedevastator/global-video-game-sales-and-ratings

Type de probleme : apprentissage supervise

Tache principale proposee : classification binaire

Tache alternative possible : regression

## Description courte du dataset

Ce dataset regroupe des informations sur des jeux video publies sur differentes
plateformes. Il contient des informations descriptives sur les jeux, comme le
nom, la plateforme, le genre, l'editeur, l'annee de sortie et la classification
d'age ESRB. Il contient aussi des informations quantitatives comme les scores
donnes par les critiques, les scores donnes par les utilisateurs, le nombre de
notes et les ventes par region.

Le dataset est adapte au projet car il permet de poser une question supervisee
claire : peut-on predire le succes commercial d'un jeu video a partir de ses
caracteristiques connues ?

## Objectif du traitement

L'objectif propose est de predire si un jeu video peut etre considere comme un
succes commercial ou non.

Pour transformer le probleme en classification binaire, on peut definir une
variable cible :

```text
success = 1 si Global_Sales est superieur a la mediane des ventes globales
success = 0 sinon
```

Cette definition evite de choisir arbitrairement un seuil fixe en millions de
copies. Elle garantit aussi un probleme mieux equilibre entre les deux classes.

Une deuxieme possibilite est de faire une regression en predissant directement
`Global_Sales`. Cependant, la classification binaire semble plus stable pour un
projet de cours, car les ventes de jeux video sont souvent tres desequilibrees :
quelques jeux ont des ventes enormes, tandis que la majorite vendent beaucoup
moins.

## Variable cible

Variable cible recommandee : `success`

Variable construite a partir de : `Global_Sales`

Description :

`Global_Sales` represente les ventes mondiales du jeu. Selon la description du
dataset, les ventes sont exprimees en millions d'unites. La variable `success`
sera construite pour indiquer si un jeu a des ventes globales superieures ou non
a la mediane du dataset.

## Features envisagees

Les features exactes devront etre confirmees apres telechargement du fichier,
mais le dataset est documente comme contenant notamment :

- `Platform` : console ou plateforme de sortie du jeu.
- `Genre` : genre du jeu, par exemple action, sport, role-playing, shooter.
- `Publisher` : editeur du jeu.
- `Year_of_Release` ou equivalent : annee de sortie.
- `Critic_Score` : score moyen donne par les critiques.
- `Critic_Count` : nombre de critiques prises en compte.
- `User_Score` : score moyen donne par les utilisateurs.
- `User_Count` : nombre de notes utilisateurs.
- `Rating` : classification d'age ESRB.

Features a eviter dans un premier modele :

- Les ventes regionales, comme `NA_Sales`, `EU_Sales`, `JP_Sales` ou
  `Other_Sales`, si la cible est construite a partir de `Global_Sales`.

Ces variables sont directement liees a la cible. Les utiliser rendrait la
prediction artificiellement facile et peu interessante.

## Questions de recherche

Questions principales :

- Peut-on predire si un jeu video aura des ventes globales elevees a partir de
  ses caracteristiques ?
- Les scores critiques et utilisateurs ameliorent-ils fortement la prediction ?
- Le genre, la plateforme ou l'editeur sont-ils des variables importantes ?
- Certains types de jeux sont-ils plus souvent associes a un succes commercial ?

Questions secondaires :

- Est-ce que les modeles lineaires sont suffisants ou faut-il des modeles non
  lineaires ?
- Est-ce que les performances changent si on retire les variables de scores
  utilisateurs et critiques ?
- Est-ce que les classes sont equilibrees apres definition de `success` ?

## Methodologie proposee

### 1. Chargement et comprehension du dataset

- Charger le fichier avec `pandas`.
- Afficher les colonnes, types, valeurs manquantes et dimensions du dataset.
- Identifier les variables numeriques et categorielles.
- Verifier la distribution de `Global_Sales`.

### 2. Nettoyage

- Supprimer ou imputer les valeurs manquantes.
- Convertir les variables numeriques mal encodees, par exemple `User_Score` si
  elle est lue comme texte.
- Regrouper les editeurs tres rares dans une modalite `Other`, si necessaire.
- Supprimer les lignes avec une cible absente.

### 3. Creation de la target

- Calculer la mediane de `Global_Sales` sur le train set uniquement.
- Creer `success` avec ce seuil.
- Verifier la proportion de classes 0 et 1.

### 4. Separation train/test

- Creer un train/test split.
- Utiliser une stratification sur `success` pour conserver les proportions des
  classes.
- Ne pas utiliser le test set pendant la selection des modeles.

### 5. Preprocessing

- Standardiser les variables numeriques avec `StandardScaler`.
- Encoder les variables categorielles avec `OneHotEncoder`.
- Construire un pipeline `scikit-learn` pour eviter les fuites de donnees.

### 6. Modeles a comparer

Modeles simples :

- `LogisticRegression`
- `KNeighborsClassifier`

Modeles plus flexibles :

- `RandomForestClassifier`
- `GradientBoostingClassifier`
- eventuellement `SVC`

Au minimum, deux modeles seront compares, comme demande dans l'esprit du projet.

### 7. Evaluation

Metriques recommandees :

- Accuracy
- F1-score
- ROC-AUC
- Matrice de confusion

L'accuracy seule peut etre insuffisante si les classes sont desequilibrees.

### 8. Interpretation

- Comparer les performances train, validation croisee et test.
- Identifier les variables les plus importantes pour les modeles qui le
  permettent.
- Discuter les limites du dataset.

## Resultat attendu

Le resultat attendu n'est pas seulement un bon score. L'objectif est surtout de
produire une analyse claire :

- un dataset correctement nettoye ;
- une cible bien definie ;
- plusieurs modeles compares ;
- une evaluation honnete ;
- une discussion des variables utiles pour predire le succes commercial.

## Limites identifiees

- Le dataset peut contenir des donnees anciennes ou incompletes.
- Les ventes peuvent etre fortement desequilibrees.
- Les scores utilisateurs et critiques peuvent parfois etre disponibles apres la
  sortie du jeu, donc ils ne representent pas forcement une prediction avant
  lancement.
- Les ventes regionales ne doivent pas etre utilisees pour predire les ventes
  globales, sauf si l'objectif est explicitement different.

## Statut (mise a jour du 5 juillet 2026)

Le traitement est implemente et execute dans
`reponse/exercice_06_supervised_learning_application/`.

- Attention: le fichier local `dataset/video_games_sales.csv` est corrompu
  (colonnes decalees, `Global_Sales` entierement vide). Le notebook utilise un
  export sain du meme dataset ("Video Games Sales as at 22 Dec 2016",
  16 719 jeux), place dans
  `reponse/data/global_video_game_sales_and_ratings.csv`.
- Les features envisagees sont confirmees presentes: `Platform`, `Genre`,
  `Publisher`, `Year_of_Release`, `Critic_Score`, `Critic_Count`, `User_Score`,
  `User_Count`, `Rating`. Les scores critiques/utilisateurs manquent pour
  environ 9 000 jeux sur 16 700 (imputation par la mediane).
- Resultats test (cible `success` = ventes > mediane train, 0.17 million
  d'unites): GradientBoosting accuracy 0.756, F1 0.751, ROC-AUC 0.839;
  RandomForest tres proche; LogisticRegression en retrait.

## Texte court a envoyer au professeur

Pour l'exercice supervise, nous proposons d'utiliser le dataset "Global Video
Game Sales and Ratings" disponible sur Kaggle :
https://www.kaggle.com/datasets/thedevastator/global-video-game-sales-and-ratings

Ce dataset contient des informations sur des jeux video : plateforme, genre,
editeur, annee de sortie, scores critiques, scores utilisateurs, classification
d'age et ventes. L'objectif serait de predire si un jeu video est un succes
commercial ou non. Nous construirions une cible binaire a partir de
`Global_Sales`, par exemple en considerant comme succes les jeux dont les ventes
globales sont superieures a la mediane du dataset. Les features utilisees
seraient notamment la plateforme, le genre, l'editeur, l'annee, le rating ESRB,
le score critique et le score utilisateur. Les ventes regionales ne seraient pas
utilisees comme features principales pour eviter une fuite d'information avec la
cible.

