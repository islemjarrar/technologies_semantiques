## Projet en Technologies Sémantiques - Domaine de la Santé

### 🎓 Membres de l'équipe
- Islem Jarrar
- Sonia Ghnimi

---

### 🏥 Domaine choisi : La Santé
Le domaine de la santé est crucial pour la structuration, l'organisation et la gestion efficace des informations médicales. Dans ce projet, nous modélisons les relations entre les patients, les médecins, les maladies, les traitements et les hôpitaux. L'utilisation des technologies sémantiques permet une meilleure interopérabilité, une possibilité d'inférence automatisée, et un accès intelligent à l'information.

---

### 📃 Structure du projet
- `/ontologie` : Contient les fichiers RDF/OWL
- `/documentation` : Documentation détaillée du projet

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

---

### 🔍 Phase 3 : Interrogation avec SPARQL
Nous avons conçu 4 requêtes SPARQL pertinentes pour interroger l'ontologie :

#### 1. Liste des patients ayant consulté un médecin :
```sparql
SELECT ?patient ?consultation WHERE {
  ?patient :aConsulté ?consultation .
}
```

#### 2. Maladies diagnostiquées lors des consultations :
```sparql
SELECT ?consultation ?maladie WHERE {
  ?consultation :aDiagnostiqué ?maladie .
}
```

#### 3. Traitements prescrits par un médecin :
```sparql
SELECT ?medecin ?traitement WHERE {
  ?medecin :prescrit ?traitement .
}
```

#### 4. Hôpitaux où les médecins travaillent :
```sparql
SELECT ?medecin ?hopital WHERE {
  ?medecin :travailleDans ?hopital .
}
```

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

### 📅 Période de réalisation
- **Durée** : 5 semaines (19/03 - 30/04)
- **Pré-évaluation** : 16/04
- **Évaluation finale** : 07/05 - 14/05

---

### 📦 Livrables
- Fichiers RDF/XML et OWL dans le répertoire `/ontologie`
- Documentation technique dans `/documentation`
- Projet partagé sur GitHub et Moodle
- README.md explicatif

---

### 📊 Conclusion
Ce projet nous a permis de mettre en pratique les concepts fondamentaux des technologies sémantiques : modélisation RDF/OWL, interrogation SPARQL, inférence par règles SWRL. L'ontologie développée dans le domaine de la santé illustre l'avantage de la représentation sémantique pour structurer, exploiter et raisonner sur les données complexes dans un environnement réel.

---

### 👁️ Auteurs
- Islem Jarrar (islem.jarrar@etudiant-example.tn)
- Sonia Ghnimi (sonia.ghnimi@etudiant-example.tn)


