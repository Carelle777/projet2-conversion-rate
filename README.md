# 📈 Projet : Optimisation du Taux de Conversion (Data Science Weekly)

## Contexte Métier
La newsletter *Data Science Weekly* souhaite comprendre le comportement de ses visiteurs pour augmenter son nombre d'abonnés. L'objectif de ce projet est de construire un modèle prédictif capable d'estimer si un utilisateur va s'abonner (conversion) en fonction de ses caractéristiques (âge, pays, source de trafic, nombre de pages visitées).

## Démarche Analytique (Méthodologie RNCP)
Ce projet valide les compétences en Machine Learning Supervisé (Classification) et s'articule autour des étapes suivantes :
1.  **Analyse Exploratoire (EDA) :** Détection et suppression des valeurs aberrantes (outliers sur l'âge > 100 ans) et analyse du taux de conversion par pays.
2.  **Préparation des Données :** Création d'un pipeline strict anti-fuite de données (`StandardScaler` pour le numérique, `OneHotEncoder` pour le catégoriel).
3.  **Gestion du Déséquilibre :** Le jeu de données présente un fort déséquilibre (97% de non-convertis). J'ai appliqué une stratification lors du split (`stratify=y`) et utilisé l'argument `class_weight='balanced'` lors de l'entraînement.
4.  **Baseline :** Évaluation initiale avec un algorithme *Random Forest*.
5.  **Optimisation (GridSearch) :** Entraînement d'une **Régression Logistique** avec optimisation de l'hyperparamètre `C` (régularisation) par validation croisée (K-Fold CV=5) en ciblant la métrique **F1-Score**.
6.  **Soumission :** Génération du fichier de prédictions sur le jeu de test aveugle pour évaluation indépendante.

## Recommandations Business (Leviers d'Action)
L'analyse des coefficients du modèle (Feature Importance) révèle clairement les leviers d'action :
*   **Facteur de succès principal :** Le nombre de pages visitées (`total_pages_visited`) est le critère le plus déterminant. *Recommandation : Créer des parcours de navigation incitatifs (articles liés) pour garder les utilisateurs plus longtemps sur le site.*
*   **Publics cibles :** Les utilisateurs d'Allemagne et du Royaume-Uni convertissent mieux, tandis que la Chine montre un taux très faible. *Recommandation : Cibler le budget publicitaire sur l'Europe ou traduire le site en mandarin.*