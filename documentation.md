## Projet en Technologies Sémantiques - Domaine de la Santé

### 🎓 Membres de l'équipe
- Islem Jarrar
- Sonia Ghnimi

---

### 🏥 Domaine choisi : La Santé
Le domaine de la santé est crucial pour la structuration, l'organisation et la gestion efficace des informations médicales. Dans ce projet, nous modélisons les relations entre les patients, les médecins, les maladies, les traitements et les hôpitaux. L'utilisation des technologies sémantiques permet une meilleure interopérabilité, une possibilité d'inférence automatisée, et un accès intelligent à l'information.

---

### 📃 Structure du projet
- `/ontologie` : Contient les fichiers RDF/OWL et RDF/XML
- `/documentation` : Documentation détaillée du projet
- `/interet de l'entologie`:Comparaison Ontologie OWL vs Base de Données Relationnelle

---

### 💡 Concepts Clés du Domaine
| Classe        | Description                                                                 |
|---------------|-----------------------------------------------------------------------------|
| Patient       | Identifiant, nom, âge, sexe, données médicales                            |
| Médecin       | Identifiant, nom, spécialité, hôpital d'exercice                          |
| Consultation  | Date, durée, patient concerné, médecin responsable                        |
| Maladie       | Nom, type, symptômes                                                       |
| Traitement    | Nom, médicament, thérapie, durée                                          |
| Hôpital       | Nom, adresse, services disponibles                                           |

---

### ✏️ Phase 1 : Choix du domaine
- Domaine choisi : **Santé**
- Justification : Ce domaine offre une richesse d'entités et de relations permettant d'appliquer les principes RDF, RDFS, OWL et SPARQL de façon concrète.

---

### 📄 Phase 2 : Modélisation RDF et RDFS
Nous avons créé des classes RDF pour chaque entité du domaine et utilisé les namespaces standards suivants :
- `xsd:` http://www.w3.org/2001/XMLSchema#
- `dc:` http://purl.org/dc/elements/1.1/
- `foaf:` http://xmlns.com/foaf/0.1/
- `skos:` http://www.w3.org/2004/02/skos/core#
- `rdfs:` http://www.w3.org/2000/01/rdf-schema#
- `owl:` http://www.w3.org/2002/07/owl#

#### 🔗 Relations RDF créées
| Propriété         | Domaine     | Portée (Range) | Description |
|------------------|-------------|----------------|-------------|
| aConsulté        | Patient     | Consultation   | Un patient a consulté un médecin dans une consultation |
| aDiagnostiqué    | Consultation| Maladie        | Une consultation a mené à un diagnostic de maladie |
| prescrit         | Médecin     | Traitement     | Un médecin prescrit un traitement |
| traite           | Traitement  | Maladie        | Le traitement cible une maladie |
| hospitaliséDans  | Patient     | Hôpital        | Un patient est hospitalisé dans un hôpital |
| travailleDans    | Médecin     | Hôpital        | Un médecin travaille dans un hôpital |
| effectuéePar     | Consultation| Médecin        | Une consultation est effectuée par un médecin |
| souffreDe        | Patient     | maladie        | un patient souffre d'une maladie |
| aPourDate        | Consultation| date           | une consultation a une date specifique |
| aPourSpecialité  | medecin     | specialité     | un medecin a une specialité |
| recoit           | patient     | traitement     | un patient recoit un traitement |


---

### 🔍 Phase 3 : Interrogation avec SPARQL
Nous avons conçu 4 requêtes SPARQL pertinentes pour interroger l'ontologie :

# REQUÊTE 1 - Patients et leurs maladies
PREFIX : <http://www.semanticweb.org/administrator/ontologies/2025/2/untitled-ontology-3#>

SELECT ?patient ?maladie WHERE {
  ?patient a :Patient ;
           :souffreDe ?maladie .
}

# REQUÊTE 2 - Médecins par spécialité
PREFIX : <http://www.semanticweb.org/administrator/ontologies/2025/2/untitled-ontology-3#>
SELECT ?medecin ?specialite WHERE { ?medecin a :medecin ; :aPourSpecialité ?specialite . }

# REQUÊTE 3 - Prescriptions complètes
PREFIX : <http://www.semanticweb.org/administrator/ontologies/2025/2/untitled-ontology-3#>
SELECT ?medecin ?patient ?traitement ?duree WHERE {
  ?medecin :prescrit ?traitement .
  ?patient :recoit ?traitement .
  ?traitement :aPourDurée ?duree .
}

# REQUÊTE 4 - Patients hospitalisés
PREFIX : <http://www.semanticweb.org/administrator/ontologies/2025/2/untitled-ontology-3#>

SELECT DISTINCT ?patient ?hopital ?medecin WHERE {
  ?patient :hospitaliséeDans ?hopital .
  ?hopital :emploie ?medecin .
  ?medecin a :medecin .
}


# REQUÊTE 5 - Traitements spécifiques
PREFIX : <http://www.semanticweb.org/administrator/ontologies/2025/2/untitled-ontology-3#>

SELECT ?maladie ?traitement ?duree WHERE {
  ?traitement :traite ?maladie ;
              :aPourDurée ?duree .
}
ORDER BY DESC(?duree)




# REQUÊTE 6 - Agenda médical
PREFIX : <http://www.semanticweb.org/administrator/ontologies/2025/2/untitled-ontology-3#>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>

SELECT ?consultation ?date ?medecin ?maladie WHERE {
  ?consultation a :consultation ;
                :aPourDate ?date ;
                :effectueéPar ?medecin ;
                :aDiagnostiqué ?maladie .
  FILTER(STRSTARTS(?date, "12/05"))  # Filtre mai 2025
}



# REQUÊTE 7 - Exploration complète d'un médecin
PREFIX : <http://www.semanticweb.org/administrator/ontologies/2025/2/untitled-ontology-3#>

DESCRIBE :Islem


*/# REQUÊTE 8 - Sous-graphe Patients-Maladies-Traitements
PREFIX : <http://www.semanticweb.org/administrator/ontologies/2025/2/untitled-ontology-3#>

CONSTRUCT {
  ?patient :aMaladie ?maladie .
  ?maladie :aTraitement ?traitement .
}
WHERE {
  ?patient :souffreDe ?maladie .
  ?traitement :traite ?maladie .
}

---

### 🔖 Phase 4 : Ontologie OWL
Nous avons enrichi notre ontologie avec des éléments OWL :
- **Restrictions** (e.g., un patient ne peut consulter qu'un médecin à la fois)
- **Classes équivalentes** (e.g., une hospitalisation peut être définie par un patient + hôpital + date)
- **Propriétés inverses** (e.g., `consultéPar` inversée de `aConsulté`)

---

### 🔨 Phase 5 : Règles SWRL
Nous avons ajouté 4 règles SWRL pour enrichir les inférences :

#### Règle 1 : Si un patient a une consultation, alors il est un patient actif
```swrl
Patient(?p) ^ aConsulté(?p, ?c) → PatientActif(?p)
```

#### Règle 2 : Si un médecin prescrit un traitement à une maladie, il est spécialisé
```swrl
Médecin(?m) ^ prescrit(?m, ?t) ^ traite(?t, ?maladie) → Spécialisé(?m)
```

#### Règle 3 : Si une consultation a diagnostiqué une maladie, alors le patient concerné a cette maladie
```swrl
Consultation(?c) ^ aDiagnostiqué(?c, ?m) ^ aConsulté(?p, ?c) → aMaladie(?p, ?m)
```

#### Règle 4 : Si un traitement dure plus de 30 jours, alors il est long terme
```swrl
Traitement(?t) ^ duree(?t, ?d) ^ swrlb:greaterThan(?d, 30) → LongTerme(?t)
```

---

### 📦 Livrables
- Fichiers RDF/XML et OWL dans le répertoire `/ontologie`
- Documentation technique dans `/documentation`
- Projet partagé sur GitHub et Moodle
- README.md explicatif

---

### 📊 Conclusion
Ce projet nous a permis de mettre en pratique les concepts fondamentaux des technologies sémantiques : modélisation RDF/OWL, interrogation SPARQL, inférence par règles SWRL. L'ontologie développée dans le domaine de la santé illustre l'avantage de la représentation sémantique pour structurer, exploiter et raisonner sur les données complexes dans un environnement réel.




