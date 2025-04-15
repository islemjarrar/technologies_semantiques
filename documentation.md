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

### REQUÊTE 1 - Patients et leurs maladies
```sparql
SELECT ?patient ?maladie WHERE {
  ?patient a :Patient ;
          :souffreDe ?maladie .
}
```

### REQUÊTE 2 - Médecins par spécialité
```sparql
SELECT ?medecin ?specialite WHERE {
  ?medecin a :medecin ;
          :aPourSpecialité ?specialite .
}
```

### REQUÊTE 3 - Prescriptions complètes
```sparql
SELECT ?medecin ?patient ?traitement ?duree WHERE {
  ?medecin :prescrit ?traitement .
  ?patient :recoit ?traitement .
  ?traitement :aPourDurée ?duree .
}
```

### REQUÊTE 4 - Patients hospitalisés
```sparql
SELECT DISTINCT ?patient ?hopital ?medecin WHERE {
  ?patient :hospitaliséeDans ?hopital .
  ?hopital :emploie ?medecin .
  ?medecin a :medecin .
}
```

### REQUÊTE 5 - Traitements spécifiques
```sparql
SELECT ?maladie ?traitement ?duree WHERE {
  ?traitement :traite ?maladie ;
              :aPourDurée ?duree .
}
ORDER BY DESC(?duree)
```

### REQUÊTE 6 - Agenda médical (mai 2025)
```sparql
SELECT ?consultation ?date ?medecin ?maladie WHERE {
  ?consultation a :consultation ;
                :aPourDate ?date ;
                :effectueéPar ?medecin ;
                :aDiagnostiqué ?maladie .
  FILTER(STRSTARTS(?date, "12/05"))
}
```

### REQUÊTE 7 - Exploration d’un médecin
```sparql
DESCRIBE :Islem
```

### REQUÊTE 8 - Sous-graphe Patients-Maladies-Traitements
```sparql
CONSTRUCT {
  ?patient :aMaladie ?maladie .
  ?maladie :aTraitement ?traitement .
}
WHERE {
  ?patient :souffreDe ?maladie .
  ?traitement :traite ?maladie .
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

### REQUÊTE 1 - Identification des maladies chroniques
```swrl
untitled-ontology-3:TraitementChronique(?m) ^
untitled-ontology-3:traite(?m, ?t) ^
untitled-ontology-3:traitement(?t)
-> untitled-ontology-3:MaladieChronique(?t)
```
*But* : Identifier les maladies associées à un traitement chronique.  
*Usage* : Suivi des pathologies de longue durée.  
*Exemple* : `:Paracetamol → :MaladieChronique`

### REQUÊTE 2 - Consultation urgente selon maladie aiguë
```swrl
untitled-ontology-3:consultation(?c) ^
untitled-ontology-3:aconsulté(?c, ?p) ^
untitled-ontology-3:souffreDe(?p, ?m) ^
untitled-ontology-3:MaladieAigue(?m)
-> untitled-ontology-3:ConsultationUrgente(?c)
```
*But* : Repérer les consultations urgentes.  
*Usage* : Triage prioritaire dans les services d'urgence.  
*Exemple* : `:consult1 → :ConsultationUrgente`

### REQUÊTE 3 - Patient traité par médecin
```swrl
untitled-ontology-3:aconsulté(?p, ?m) ^
untitled-ontology-3:aDiagnostiqué(?m, ?d)
-> untitled-ontology-3:traite(?m, ?p)
```
*But* : Lier un médecin à un patient qu'il traite.  
*Usage* : Suivi des responsabilités médicales.  
*Exemple* : `:Dr_Mayssa traite :Montasar`

### REQUÊTE 5 - Patient hospitalisé
```swrl
untitled-ontology-3:souffreDe(?p, ?m) ^
untitled-ontology-3:MaladieChronique(?m) ^
untitled-ontology-3:hospitaliséeDans(?m, ?h)
-> untitled-ontology-3:hospitaliséeDans(?p, ?h)
```
*But* : Déduire l'hospitalisation du patient à partir de celle de la maladie.  
*Usage* : Attribution automatique d’hôpital.  
*Exemple* : `:asma → hospitaliséeDans :Hôpital_Hedi_Chaker`, donc `:Oumaima → hospitaliséeDans :Hôpital_Hedi_Chaker`

---

### 📦 Livrables
- Fichiers RDF/XML et OWL dans le répertoire `/ontologie`
- Documentation technique dans `/documentation`
- Projet partagé sur GitHub et Moodle
- README.md explicatif

---

### 📊 Conclusion
Ce projet nous a permis de mettre en pratique les concepts fondamentaux des technologies sémantiques : modélisation RDF/OWL, interrogation SPARQL, inférence par règles SWRL. L'ontologie développée dans le domaine de la santé illustre l'avantage de la représentation sémantique pour structurer, exploiter et raisonner sur les données complexes dans un environnement réel.



