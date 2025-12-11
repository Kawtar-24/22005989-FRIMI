
# thematique
Détection de fraude sur transactions bancaires — Dataset déséquilibré (Credit Card Fraud Detection)

# small desc
Analyse et modélisation d’un jeu de données fortement déséquilibré afin d’identifier les transactions frauduleuses grâce à des méthodes d’apprentissage supervisé.

# 1. Contexte métier et la mission
Le secteur bancaire est confronté à une augmentation constante des tentatives de fraude lors des transactions par carte. La capacité à identifier rapidement les transactions suspectes permet de réduire considérablement les pertes financières ainsi que d’améliorer la sécurité pour les clients.

Mission :
Construire un modèle de classification binaire capable de détecter les transactions frauduleuses malgré un déséquilibre très important entre les classes.

# PROBLEM
Le dataset présente un déséquilibre massif (~0,17% de fraudes). Les modèles classiques sont biaisés vers la classe majoritaire.

# OBJ
1. Construire un modèle performant.
2. Gérer le déséquilibre.
3. Réaliser une EDA.
4. Comparer plusieurs modèles.

# ENJEUX CRITIQUES
- Déséquilibre extrême
- Réduction des faux négatifs
- Contraintes opérationnelles
- Sécurité des données

# Le jeu de données — contextualisation
- 284 807 transactions
- Cible binaire : Class
- 28 features PCA + Amount + Time

# Analyse approfondie — Nettoyage
- Dataset propre
- Standardisation Amount
- Vérification duplications

# Analyse approfondie — EDA
- Distribution cible
- Corrélations faibles (PCA)
- Importance de V14, V12, V10

# Analyse méthodologique — Split
- Train/Test stratifié
- SMOTE ou undersampling pour train

# Focus théorique — Random Forest & Arbre de décision
## Arbre de décision
Interprétable mais risque d'overfitting.

## Random Forest
- Réduit variance
- Robuste aux données bruitées
- `class_weight='balanced'`

# Corrélation & importance
- Mesurée via feature_importances_
- V14, V12, V10 les plus discriminantes

# Conclusion
Approche complète et robuste pour la détection de fraude bancaire.
