# Ontologie Médicale - Technologies Sémantiques

## 👥 Membres de l'équipe
- Islem_Jarrar
- Sonia_Ghnimi

## 🏥 Domaine : Système d'Information Médical

### 📌 Objectifs
Modéliser les relations patients-médecins-maladies-traitements pour :
- Améliorer l'interopérabilité des données médicales
- Faciliter les inférences automatiques
- Optimiser la recommandation de traitements

## 🧠 Concepts Clés
| Concept        | Propriétés                          | Relations                     |
|----------------|-------------------------------------|-------------------------------|
| **Patient**    | identifiant, nom, âge, sexe        | souffreDe, hospitaliséDans   |
| **Médecin**    | spécialité, hôpital                | prescrit, travailleDans       |
| **Consultation**| date, durée                        | effectuéPar, aDiagnostiqué   |
| **Maladie**    | type, symptômes                    | traitéPar                    |
| **Traitement** | durée, médicament                  | prescritPar                  |
| **Hôpital**    | services, adresse                  | emploie                      |

## 📂 Structure du Projet
- /ontologie: Contiendra les fichiers RDF/OWL
- /documentation: Documentation détaillée du projet
- /sparql :les requêtes sparql et leurs résultats
- /swrl :les requetes swrl et leurs resultats

