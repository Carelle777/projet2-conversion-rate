# 📈 Projet : Optimisation du Taux de Conversion (Data Science Weekly)

## Contexte Métier
La newsletter *Data Science Weekly* souhaite comprendre le comportement de ses visiteurs pour augmenter son nombre d'abonnés. L'objectif de ce projet est de construire un modèle prédictif capable d'estimer si un utilisateur va s'abonner (conversion) en fonction de ses caractéristiques (âge, pays, nouvel utilisateur ou non, source de trafic, nombre de pages visitées).

## Démarche Analytique
Ce projet valide les compétences en Machine Learning Supervisé (Classification) :
1. **Analyse exploratoire (EDA) :** 284 580 lignes, 5 variables. Suppression de 2 valeurs aberrantes (âge supérieur ou égal à 100 ans) et analyse du taux de conversion par pays.
2. **Préparation des données :** pipeline sans fuite de données (`StandardScaler` pour le numérique, `OneHotEncoder` pour le catégoriel). Découpage 80/20 stratifié : 227 662 lignes pour l'entraînement, 56 916 pour le test.
3. **Déséquilibre des classes :** environ 3 % d'abonnés seulement. J'ai utilisé la stratification (`stratify=Y`). J'ai aussi testé `class_weight='balanced'` : avec cette option le F1 test de la régression logistique est de 0,509, sans elle de 0,762. Je l'ai donc **retiré du modèle final**.
4. **Baseline :** Random Forest (`class_weight='balanced'`), F1 test = 0,567.
5. **Optimisation :** Régression Logistique avec `GridSearchCV` sur l'hyperparamètre `C` (0,01 ; 0,1 ; 1 ; 10 ; 100), validation croisée à 5 plis, métrique F1. Meilleur `C` = 1, F1 en validation croisée = 0,763.
6. **Soumission :** fichier de prédictions sur le jeu de test aveugle : `notebooks/conversion_data_test_predictions_CARELLE.csv`.

## Résultats (jeu de test)
| Modèle | F1 test |
|---|---|
| Random Forest (baseline, class_weight balanced) | 0,567 |
| Régression logistique avec class_weight balanced (C = 0,1) | 0,509 |
| **Régression logistique optimisée, sans class_weight (C = 1)** | **0,763** |

Modèle final : précision 0,85 et rappel 0,69 pour les abonnés. Matrice de confusion : 54 857 non-abonnés bien reconnus, 223 fausses alertes, 565 abonnés manqués, 1 271 abonnés trouvés.

En mots simples : quand le modèle annonce qu'un visiteur va s'abonner, il a raison 85 fois sur 100, et il retrouve environ 7 abonnés sur 10.

## Leviers d'action (coefficients du modèle)
* **Pays :** l'Allemagne, le Royaume-Uni et les États-Unis ont les plus gros effets positifs par rapport à la Chine (pays de référence), qui convertit le moins.
* **Pages visitées :** c'est la variable chiffrée la plus influente : plus on visite de pages, plus on s'abonne.
* **Nouvel utilisateur :** effet négatif : les visiteurs qui reviennent s'abonnent plus que les nouveaux.
* **Âge :** effet négatif léger. **Source de trafic :** effet faible.

Recommandations (à tester avant de décider) :
* Proposer des articles liés pour faire visiter plus de pages.
* Encourager les visiteurs à revenir (ils convertissent mieux).
* Concentrer la publicité sur l'Allemagne, le Royaume-Uni et les États-Unis, et vérifier pourquoi la Chine convertit si peu (langue, pertinence) avant d'investir.

## Limites
* Les coefficients montrent des liens statistiques, pas des causes.
* Un seul découpage train/test.
* Le F1 est calculé avec le seuil de décision par défaut (0,5), qu'on pourrait ajuster.
* Les coefficients des pays et des variables chiffrées ne sont pas parfaitement comparables (les variables chiffrées sont standardisées).

## Relancer le projet
1. `pip install -r requirements.txt`
2. Placer `conversion_data_train.csv` et `conversion_data_test.csv` dans le dossier `data/` (non versionné).
3. Ouvrir `notebooks/01-conversion-rate.ipynb` et exécuter toutes les cellules.
