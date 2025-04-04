# Comparaison Ontologie OWL vs Base de Données Relationnelle

## Scénario OWL
### Fonctionnalité :
Le système détecte automatiquement qu'un traitement est inadapté à une maladie.  
✅ **Avantage clé :** Pas besoin d'écrire une requête manuelle.


##🔍 Exemple de requête SQL :

SELECT p.nom  FROM patients p    JOIN prescriptions pr ON p.id = pr.patient_id  JOIN contre_indications ci ON pr.traitement_id = ci.traitement_id  JOIN diagnostics d ON p.id = d.patient_id  WHERE d.maladie_id = ci.maladie_id;

##📊 Tableau comparatif


| Fonctionnalité               | Ontologie OWL                    | Base Relationnelle            |
|------------------------------|-----------------------------------|-------------------------------|
| Inférence automatique         | ✅ (via Raisonneur)               | ❌ (Requête manuelle)         |
| Hiérarchie dynamique          | ✅ (subClassOf)                   | ❌ (Corrections de tables)    |
| Flexibilité sémantique        | ✅ (OWL DL)                       | ❌ (Schéma rigide)            |
| Gestion des contradictions    | ✅ (Vérification de cohérence)    | ❌ (Problèmes de redondance)  |

##🔎 Analyse détaillée


| Critère                        | Ontologie OWL                       | Base SQL Relationnelle         |
|---------------------------------|--------------------------------------|--------------------------------|
| Détection d'erreurs            | Inférence automatique               | Nécessite des déclencheurs complexes |
| Évolutivité                    | Ajout de règles sans migration      | ALTER TABLE nécessaire         |
| Performance                    | Peut ralentir sur >100k instances    | Optimisé pour gros volumes    |
| Flexibilité                    | Modélisation sémantique riche       | Schéma rigide                 |
| Maintenance                    | Règles centralisées                 | Code dispersé                 |



## ✅Conclusion

L'utilisation d'une ontologie OWL permet une détection automatique des incohérences et facilite l'évolution du système grâce aux règles sémantiques et à l'inférence automatique.  
En revanche, une base relationnelle est plus optimisée pour le traitement de gros volumes de données, mais nécessite une gestion manuelle des règles et une structure de données rigide.

