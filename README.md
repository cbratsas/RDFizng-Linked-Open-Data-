# Δημοσιοποίηση Δεδομένων με Τεχνικές Linked Open Data (LOD)

Η εργασία αυτή περιλαμβάνει τρεις ασκήσεις σχετικές με τη δημοσιοποίηση και σημασιολογική αναπαράσταση δεδομένων μέσω τεχνικών και εργαλείων του Σημασιολογικού Ιστού. Οι ασκήσεις εστιάζουν στη χρήση των εργαλείων **D2RQ Server**, **OpenRefine RDFizing** και **LinkedPipes ETL**.

Η εργασία έχει ως στόχο την εξοικείωση με:

- RDF και Linked Data
- URI σχεδιασμό
- Mapping σχεσιακών και tabular δεδομένων σε RDF
- Επαναχρησιμοποίηση υπαρχουσών οντολογιών
- SPARQL querying
- RDF pipelines και semantic transformations

---

# Άσκηση 1 — Μετατροπή Σχεσιακής Βάσης σε RDF με D2RQ Server

Να πραγματοποιήσετε μετατροπή μιας σχεσιακής βάσης δεδομένων MySQL/MariaDB σε RDF χρησιμοποιώντας τον D2RQ Server.

Η βάση πρέπει να περιέχει:

- Τουλάχιστον 4 πίνακες
- Σχέσεις μεταξύ των πινάκων (foreign keys)
- Πάνω από μία εγγραφή σε κάθε πίνακα

---

## Βάσεις δεδομένων που μπορείτε να χρησιμοποιήσετε

1. Employees Sample Database  
   https://dev.mysql.com/doc/employee/en/

2. Airport DB Structure  
   https://dev.mysql.com/doc/airportdb/en/airportdb-structure.html

3. Football DB  
   https://drive.google.com/drive/u/0/folders/1aQXN2Iyfh9U6TQsks0XJh8qpxJ2dOxiW

4. Formula 1 Database  
   https://drive.google.com/drive/u/0/folders/1aQXN2Iyfh9U6TQsks0XJh8qpxJ2dOxiW

Οποιοδήποτε μέρος των παραπάνω βάσεων που ικανοποιεί τις παραπάνω προϋποθέσεις.

---

# Α. Δημιουργία Βάσης

Να δημιουργήσετε ή να εισάγετε τη βάση στη MySQL/MariaDB.

## Προτεινόμενα εργαλεία

- XAMPP
- phpMyAdmin
- MySQL Workbench

Η βάση πρέπει να περιέχει:

- πρωτεύοντα κλειδιά
- ξένα κλειδιά
- κανονικοποιημένες σχέσεις

---

# Β. Δημιουργία Auto-generated Mapping

Να δημιουργήσετε αυτόματο mapping με την εντολή:

```bash
generate-mapping.bat -u root -p PASSWORD jdbc:mysql://localhost/DATABASE > mapping.n3
```

## Παράδειγμα

```bash
generate-mapping.bat -u root -p "" jdbc:mysql://localhost/university > mapping.n3
```

Το mapping που παράγεται είναι auto-generated και βασίζεται αποκλειστικά στο relational schema.

---

# Γ. Τροποποίηση του Auto-generated Mapping

Το auto-generated mapping που παράγεται από το D2RQ πρέπει να τροποποιηθεί ώστε να επαναχρησιμοποιεί υπάρχουσες οντολογίες και vocabularies του Linked Open Data.

Αντικαταστήστε τα auto-generated classes και properties με σημασιολογικά κατάλληλες κλάσεις και ιδιότητες από υπάρχουσες οντολογίες.

---

# Προτεινόμενες Οντολογίες

| Οντολογία | Namespace | Χρήση |
|---|---|---|
| FOAF | `http://xmlns.com/foaf/0.1/` | Περιγραφή προσώπων, ονομάτων, emails |
| Schema.org | `https://schema.org/` | Μαθήματα, οργανισμοί, metadata |
| Dublin Core Terms | `http://purl.org/dc/terms/` | Τίτλοι, δημιουργοί, ημερομηνίες |
| SKOS | `http://www.w3.org/2004/02/skos/core#` | Θεματικές κατηγορίες |
| ORG | `http://www.w3.org/ns/org#` | Οργανισμοί και memberships |
| BIBO | `http://purl.org/ontology/bibo/` | Βιβλιογραφικά δεδομένα |
| CIDOC CRM | `http://www.cidoc-crm.org/cidoc-crm/` | Πολιτιστική κληρονομιά |
| SIOC | `http://rdfs.org/sioc/ns#` | Online communities |
| GeoSPARQL | `http://www.opengis.net/ont/geosparql#` | Γεωχωρικά δεδομένα |
| OWL-Time | `http://www.w3.org/2006/time#` | Χρονικές περίοδοι |
| vCard | `http://www.w3.org/2006/vcard/ns#` | Contact information |
| PROV-O | `http://www.w3.org/ns/prov#` | Provenance |
| Άλλες οντολογίες | https://lov.linkeddata.es/dataset/lov/vocabs | Αναζήτηση LOD vocabularies |

---

# Ελάχιστες Απαιτήσεις Semantic Mapping

Η τελική RDF αναπαράσταση πρέπει να περιλαμβάνει τουλάχιστον:

- 3 επαναχρησιμοποιημένες RDF κλάσεις
- 6 επαναχρησιμοποιημένες RDF ιδιότητες (properties)

από υπάρχουσες οντολογίες.

---

# Παράδειγμα Mapping

## Auto-generated Mapping

```ttl
map:Persons a d2rq:ClassMap;
    d2rq:dataStorage map:database;
    d2rq:uriPattern "Persons/@@Persons.perID@@";
    d2rq:class vocab:Persons;
```

## Ontology-oriented Mapping

```ttl
map:Persons a d2rq:ClassMap;
    d2rq:dataStorage map:database;
    d2rq:uriPattern "person/@@Persons.perID@@";
    d2rq:class foaf:Person;
```

---

# Παράδειγμα Property Mapping

```ttl
map:PersonName a d2rq:PropertyBridge;
    d2rq:belongsToClassMap map:Persons;
    d2rq:property foaf:name;
    d2rq:pattern "@@Persons.firstName@@ @@Persons.lastName@@";
```

---

# Παραδείγματα Αλλαγών στο Auto-generated Mapping

| Auto-generated | Ontology-oriented |
|---|---|
| vocab:Person | foaf:Person |
| vocab:title | dcterms:title |
| vocab:Course | schema:Course |
| custom labels | rdfs:label |

---

# Δ. SPARQL Queries

Να εκτελέσετε τουλάχιστον 3 SPARQL queries στο endpoint:

```text
http://localhost:2020/snorql/
```

---

# Άσκηση 2 — RDFizing CSV Δεδομένων με OpenRefine

Να χρησιμοποιήσετε το OpenRefine και το RDF Extension για μετατροπή CSV σε RDF.

## Dataset

```text
simple_students_courses_foaf_schema.csv
```

---

# Ενδεικτικός Πίνακας Mapping

| Στήλη CSV | RDF Property | Οντολογία |
|---|---|---|
| StudentID | @id | URI |
| FullName | foaf:name | FOAF |
| Email | foaf:mbox | FOAF |
| CourseTitle | schema:name | Schema.org |
| CourseDescription | schema:description | Schema.org |
| ECTS | ex:ects | Custom |
| CourseID | ex:enrolledIn | Custom |

---

# URI Design

## Students — `foaf:Person`

```text
http://example.org/university/student/{StudentID}
```

## Courses — `schema:Course`

```text
http://example.org/university/course/{CourseID}
```

---

# RDF Export

Το τελικό RDF να εξαχθεί σε:

```text
Turtle (.ttl)
```

---

# Άσκηση 3 — Μετατροπή CSV σε RDF με LinkedPipes ETL

Να δημιουργήσετε pipeline στο LinkedPipes ETL για μετατροπή CSV σε RDF του ίδιου CSV με τις ίδιες αντιστοιχίσεις mapping που υλοποιήθηκαν στην Άσκηση 2.

---

# Στόχος Pipeline

Το pipeline πρέπει:

- να διαβάζει CSV
- να δημιουργεί RDF resources
- να παράγει Linked Data URIs
- να εξάγει RDF/Turtle

---

# Ενδεικτική Pipeline Architecture

```text
CSV Input
   ↓
CSV Parser / Tabular
   ↓
SPARQL CONSTRUCT
   ↓
RDF Writer
```

---

# RDF Output

Το RDF να εξαχθεί σε:

```text
Turtle (.ttl)
```

---

# Παραδοτέα

1. Τα τελικά αρχεία:
   - `.n3`
   - `.ttl`
   - SPARQL queries

2. Το pipeline JSON του LinkedPipes.

3. Ένα κείμενο τεκμηρίωσης που να περιλαμβάνει:

## α. Σύντομη περιγραφή των οντολογιών που χρησιμοποιήθηκαν

## β. Πίνακες αντιστοίχισης mappings

## γ. Τη διαδικασία mapping και στα 3 εργαλεία με screenshots

## δ. Τα SPARQL queries και τα αποτελέσματά τους

## ε. Περιγραφή του URI design

## στ. Περιγραφή των semantic transformations που πραγματοποιήθηκαν
