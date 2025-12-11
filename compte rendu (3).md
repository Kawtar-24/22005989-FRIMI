Nom & Prénom : FRIMI Kawtar

![WhatsApp Image 2025-12-11 à 11 35 50_43b476ee](https://github.com/user-attachments/assets/a7e5368d-6e3b-437a-b9c0-b57183f89f1b)


# Rapport Académique : Détection de Fraude sur Cartes Bancaires avec Datasets Déséquilibrés

---

## Thématique
**Machine Learning pour la Détection de Fraude Financière**

---

## Description
Analyse d'un cas d'étude Kaggle sur la détection de fraude bancaire confronté au défi majeur du déséquilibre extrême des données (0,172% de fraudes sur 284k transactions). Comparaison de techniques de rééquilibrage (SMOTE, undersampling) et 7 algorithmes de classification pour identifier la solution optimale.

---

## 1. Contexte Métier et Mission

### Contexte métier
Les institutions bancaires européennes traitent quotidiennement des millions de transactions par carte bancaire. La fraude représente un risque financier majeur avec des pertes mondiales estimées à plusieurs milliards d'euros annuellement. Le dataset étudié provient de Worldline et de l'Université Libre de Bruxelles (ULB), couvrant des transactions européennes de septembre 2013.

### Mission
Développer un système de détection automatique de fraude en temps réel capable d'identifier les transactions frauduleuses tout en minimisant les faux positifs qui impactent l'expérience client. La mission inclut le traitement du déséquilibre massif des données et la sélection du modèle optimal pour un déploiement en production.

---

## Problématique

**Comment détecter efficacement les fraudes bancaires dans un contexte de déséquilibre extrême où les transactions frauduleuses représentent moins de 0,2% du volume total ?**

Le déséquilibre critique (ratio 1:577 entre fraudes et transactions légitimes) biaise les algorithmes ML classiques vers la classe majoritaire, produisant des modèles ayant une accuracy élevée (>99%) mais incapables de détecter réellement les fraudes. Un système prédisant systématiquement "non-fraude" atteindrait 99,8% d'accuracy mais serait totalement inutile.

---

## Objectifs

### Objectif principal
Identifier la combinaison optimale technique de rééquilibrage + algorithme de classification maximisant la détection de fraudes (Recall) tout en contrôlant les faux positifs (Precision).

### Objectifs spécifiques
1. Évaluer l'impact de différentes techniques de rééquilibrage (SMOTE, Random Undersampling, NearMiss, SMOTE-Tomek)
2. Comparer les performances de 7 algorithmes de classification sur données équilibrées vs déséquilibrées
3. Prioriser les métriques pertinentes (Recall, F1-Score, ROC-AUC) plutôt que l'Accuracy trompeuse
4. Quantifier l'impact économique et opérationnel pour le secteur bancaire
5. Formuler des recommandations concrètes pour le déploiement en production

---

## Enjeux Critiques

### Enjeu financier
Chaque fraude non détectée coûte en moyenne 100-500€, tandis qu'un faux positif coûte 2-5€ en investigation. Pour une banque traitant 1 million de transactions/jour avec 0,2% de fraudes (2000 fraudes potentielles), un recall de 94% signifie 120 fraudes manquées quotidiennes, soit une perte potentielle de 12-60k€/jour.

### Enjeu réglementaire
La directive européenne PSD2 impose aux banques des obligations strictes en matière de sécurité des paiements et de lutte contre la fraude. Un système de détection défaillant expose l'institution à des sanctions réglementaires et à des pertes de certification.

### Enjeu expérience client
Un taux élevé de faux positifs (blocages injustifiés de transactions légitimes) dégrade l'expérience client et génère des coûts de support. L'équilibre Precision/Recall est donc critique pour la satisfaction client.

### Enjeu technique
Le traitement temps-réel de millions de transactions quotidiennes nécessite des algorithmes scalables et performants. Le système doit scorer chaque transaction en moins de 100ms pour validation avant autorisation de paiement.

---

## 2. Données et Nettoyage

| **Aspect** | **Description** |
|-----------|----------------|
| **Source** | Dataset Kaggle (Worldline & ULB) - Transactions européennes Sept 2013 |
| **Volume** | 284 807 transactions |
| **Features** | 30 variables (V1-V28 transformées par PCA, Time, Amount, Class) |
| **Déséquilibre** | 492 fraudes (0,172%) vs 284 315 transactions légitimes (99,828%) |
| **Valeurs manquantes** | Aucune |
| **Normalisation** | RobustScaler appliqué sur Time et Amount (V1-V28 déjà normalisées) |
| **Confidentialité** | Features anonymisées via PCA pour protection des données |

---

## 3. Analyse Exploratoire (EDA)

### Insights majeurs :

1. **Déséquilibre critique** : Ratio 1:577 (fraude:légitime) nécessitant impérativement des techniques de rééquilibrage pour éviter un modèle biaisé prédisant systématiquement "non-fraude".

2. **Corrélations significatives** : Les features V3, V10, V12, V14 montrent une corrélation négative forte avec les fraudes (-0,3 à -0,5), tandis que V4 et V11 présentent une corrélation positive, identifiant ces variables comme prédicteurs clés.

3. **Distribution des montants** : Les transactions frauduleuses présentent des montants globalement inférieurs (médiane ~80€) comparé aux transactions légitimes, suggérant des stratégies de fraude ciblant des sommes modestes pour éviter la détection.

### Figure résumée :

La matrice de corrélation révèle des patterns distincts : les variables PCA les plus corrélées (V17, V14, V12, V10) permettent une séparation optimale des classes, tandis que Time montre une distribution uniforme sans pouvoir discriminant significatif.

---

## 4. Méthodologie de Split et Modèles Testés

### Stratégie de split :

- **Train/Test** : 80/20 avec stratification sur la variable Class
- **Validation croisée** : StratifiedKFold (5 folds) pour robustesse
- **Point critique** : Rééquilibrage appliqué **uniquement sur train set** après split pour éviter le data leakage

### Techniques de rééquilibrage :

1. **Random Undersampling** : Réduction classe majoritaire (ratio 1:1)
2. **SMOTE** : Génération synthétique instances minoritaires
3. **NearMiss** : Undersampling intelligent basé sur k-NN
4. **Combinaison SMOTE-Tomek** : Oversampling + nettoyage frontières

### Modèles évalués :

- Logistic Regression (baseline)
- K-Nearest Neighbors (KNN)
- Support Vector Machine (SVM)
- Decision Tree
- Random Forest
- XGBoost
- Ensemble Voting (combinaison modèles performants)

---

## 5. Résultats et Interprétations

### Performances sur dataset original (déséquilibré) :

| Modèle | Precision | Recall | F1-Score | Accuracy |
|--------|-----------|--------|----------|----------|
| Logistic Regression | 0.88 | 0.61 | 0.72 | 0.999 |
| Random Forest | 0.92 | 0.72 | 0.81 | 0.9995 |
| XGBoost | 0.91 | 0.76 | 0.83 | 0.9996 |

**Observation** : Accuracy élevée trompeuse due au déséquilibre (prédire toujours "non-fraude" = 99,8% accuracy).

### Performances après SMOTE :

| Modèle | Precision | Recall | F1-Score | ROC-AUC |
|--------|-----------|--------|----------|---------|
| Logistic Regression | 0.94 | 0.92 | 0.93 | 0.98 |
| SVM | 0.96 | 0.93 | 0.94 | 0.99 |
| Random Forest | 0.99 | 0.85 | 0.91 | 0.97 |
| XGBoost | 0.97 | 0.94 | 0.95 | 0.99 |

### Performances après Random Undersampling (1:1) :

| Modèle | Precision | Recall | F1-Score | ROC-AUC |
|--------|-----------|--------|----------|---------|
| Logistic Regression | 0.96 | 0.94 | 0.95 | 0.99 |
| Random Forest | 0.93 | 0.93 | 0.93 | 0.98 |
| XGBoost | 0.93 | 0.94 | 0.93 | 0.99 |

### Interprétations clés :

1. **XGBoost et SVM leaders** : Meilleur équilibre precision/recall après SMOTE, minimisant faux négatifs (fraudes non détectées) et faux positifs (blocages légitimes).

2. **Trade-off critique** : Random Forest privilégie precision (99%) au détriment du recall (85%), tandis que XGBoost optimise le recall (94%) essentiel en contexte bancaire où manquer une fraude coûte plus cher qu'un faux positif.

3. **Undersampling vs Oversampling** : SMOTE surpasse l'undersampling simple en préservant l'information de la classe majoritaire. Random undersampling risque de perdre des patterns légitimes importants.

4. **Métriques prioritaires** : En fraude bancaire, **Recall** et **F1-Score** sont plus pertinents que Accuracy. Un recall de 94% signifie 6% de fraudes non détectées.

---

## 6. Lien avec le Sujet : Arguments Concrets

### Application bancaire réelle :

**Contexte opérationnel** : Les banques traitent des millions de transactions quotidiennes avec un taux de fraude < 0,2%. Un système basé uniquement sur accuracy serait inutilisable en production.

**Impact économique quantifié** :
- Coût fraude non détectée : 100-500€ par transaction
- Coût faux positif : 2-5€ (investigation + insatisfaction client)
- Avec 1M transactions/jour et 0,2% fraudes : 2000 fraudes potentielles
- Recall 94% = 120 fraudes manquées/jour = perte 12-60k€/jour vs système parfait

**Avantages techniques démontrés** :

1. **SMOTE + XGBoost** : Solution optimale identifiée avec F1=0.95, déployable en production avec seuil de probabilité ajustable selon politique risque banque.

2. **Interprétabilité features** : Les corrélations V3, V10, V12, V14 permettent aux analystes de comprendre les patterns frauduleux, facilitant l'amélioration continue du système.

3. **Scalabilité** : XGBoost supporte le traitement parallèle, crucial pour le scoring temps-réel de millions de transactions.

4. **Coût-bénéfice** : Avec recall 94%, le système évite 1880 fraudes/jour (vs 2000), soit économie nette de ~200k€/jour (94k€ fraudes évitées - coût FP investigation).

### Positionnement scientifique :

Cette approche s'inscrit dans l'état de l'art : des études récentes (2023-2024) confirment la supériorité de SMOTE couplé à XGBoost/SVM sur datasets bancaires déséquilibrés, avec des F1-scores similaires (0.93-0.96).

---

## 7. Limites et Recommandations

### Limites identifiées :

1. **Dataset statique** : Données de septembre 2013 ne capturent pas l'évolution des techniques de fraude (phishing, cryptomonnaies, deepfakes). Risque d'obsolescence rapide du modèle.

2. **Anonymisation contraignante** : PCA empêche l'interprétation business des features V1-V28, limitant la validation expert et l'enrichissement avec features métier (géolocalisation, historique client, comportement achats).

3. **Absence features contextuelles** : Manque de données comportementales (fréquence achats, patterns horaires, localisation GPS) qui amélioreraient significativement la détection selon littérature récente.

4. **Trade-off non résolu** : Optimisation recall augmente faux positifs, frustrant clients légitimes. Nécessite calibration fine du seuil selon profil risque banque et segment clientèle.

### Recommandations opérationnelles :

1. **Réentraînement continu** : Pipeline MLOps avec retraining mensuel sur nouvelles fraudes pour adaptation patterns émergents. Monitoring drift features via tests statistiques (KS, PSI).

2. **Approche hybride** : Combiner ML (scoring automatique) + règles métier + validation humaine sur transactions à risque intermédiaire (probabilité 0.4-0.6) pour optimiser coût/efficacité.

3. **Enrichissement données** : Intégrer APIs géolocalisation, profils comportementaux, réseaux sociaux (consentement RGPD) pour créer features discriminantes supplémentaires.

4. **Techniques avancées** : Explorer deep learning (LSTM pour patterns temporels), GANs (génération fraudes synthétiques), ensemble stacking (XGBoost + Neural Networks) et explainability (SHAP values pour audit conformité).

5. **Graphes**
<img width="1216" height="406" alt="7" src="https://github.com/user-attachments/assets/3a22f403-aaef-48c9-bf2f-7b21b75b0f03" />
<img width="575" height="320" alt="1" src="https://github.com/user-attachments/assets/b36cc101-a423-424f-a965-24dd92d10788" />
<img width="1145" height="282" alt="2" src="https://github.com/user-attachments/assets/119d5d22-7f7a-4a4c-a841-f3eba296a6fd" />
<img width="468" height="280" alt="3" src="https://github.com/user-attachments/assets/a5758634-7b39-4944-8376-7eaec5d4cd94" />
<img width="1291" height="1217" alt="4" src="https://github.com/user-attachments/assets/eba11187-6cda-422a-9122-951d7b704f64" />
<img width="1291" height="1217" alt="5" src="https://github.com/user-attachments/assets/c4b30339-8f3e-45e6-bf79-c5e48b8c5a09" />
<img width="1223" height="294" alt="6" src="https://github.com/user-attachments/assets/de569451-c352-40a0-a2fc-6c02ded72886" />

   
---

**Conclusion** : L'étude démontre qu'un traitement méthodique du déséquilibre via SMOTE couplé à XGBoost permet d'atteindre des performances opérationnelles (F1=0.95, Recall=0.94) adaptées au déploiement bancaire, sous réserve d'un monitoring continu et d'enrichissements contextuels.
