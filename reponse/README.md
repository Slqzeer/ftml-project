# Reponses FTML 2026

Ce dossier contient une proposition de reponse question par question au sujet `FTML/project/FTML 2026 Project description.pdf`.

## Structure

- `exercice_01_bayes_estimator_risk/`: cadre supervise, predicteur de Bayes, risque de Bayes et simulation.
- `exercice_02_absolute_loss/`: perte absolue, mediane conditionnelle et simulation.
- `exercice_03_ols_empirical_risk/`: preuve OLS et estimation numerique de `sigma^2`.
- `exercice_04_regression_dataset/`: regression sur les `.npy` fournis.
- `exercice_05_classification_dataset/`: classification sur les `.npy` fournis.
- `exercice_06_supervised_learning_application/`: pipeline supervise pour le dataset Kaggle jeux video.
- `exercice_07_unsupervised_learning_application/`: pipeline clustering pour le dataset Kaggle Steam.

## Points cles

- Exercices 1 a 5: les notebooks sont executes et les sorties sont incluses dans les cellules.
- Exercice 1: risque test de `f*` = 3.985 pour un risque de Bayes theorique de 4; l'estimateur non optimal atteint 16.98 pour une valeur theorique de 17.
- Exercice 2: la mediane conditionnelle (log 2 pour Exp(1)) bat la moyenne (1.0) en risque absolu test: 0.6928 contre 0.7356. La preuve du cas general inclut l'argument de convexite de `g` (necessaire a cause du piege de la Question 0).
- Exercice 3: preuve complete question par question; la simulation donne `sigma^2` estime a 2.887 pour une valeur theorique de 2.89.
- Exercice 4: ElasticNet et Lasso depassent l'objectif avec environ `R2 = 0.92` sur test (objectif `> 0.88`).
- Exercice 5: malgre une recherche large (LogisticRegression, SVC, KNN, MLP, AdaBoost, QDA/LDA, boosting, selection de variables, stacking), les meilleurs essais restent autour de `0.80-0.81` d'accuracy test (stacking: 0.808). L'objectif indicatif `0.85` n'est pas atteint avec les grilles testees; les scores CV et test sont coherents, la limite ne vient donc pas d'un overfitting de la selection de modele.
- Exercice 6 (jeux video, classification du succes commercial, 16 719 jeux): GradientBoosting meilleur modele avec accuracy test 0.756 et ROC-AUC test 0.839 (baseline 0.5, classes equilibrees par construction). CSV dans `reponse/data/global_video_game_sales_and_ratings.csv`.
- Exercice 7 (Steam, clustering de 82 956 jeux ayant au moins un avis): KMeans avec k = 6 retenu par silhouette (0.186) et Davies-Bouldin (1.60). Clusters interpretables: longue traine indie, hits payants etablis, Early Access, Free To Play, MMO (segment le moins bien note), Racing. Attention: le CSV `dataset/steam_game_dataset.csv` a un en-tete defectueux (`Discount` et `DLC count` fusionnes) repare au chargement dans le notebook.
- Les choix de datasets et objectifs pour 6 et 7 sont detailles dans `dataset/01_...md` et `dataset/02_...md`.

## Execution

Depuis le dossier `FTML`, utiliser l'environnement du projet:

```bash
uv run jupyter lab
```

ou executer les cellules dans l'IDE avec l'environnement `.venv` cree par `uv`.

Toutes les bibliotheques utilisees (numpy, scikit-learn, pandas, matplotlib) sont deja dans le `requirements.txt` du cours; aucun requirements.txt additionnel n'est necessaire.
