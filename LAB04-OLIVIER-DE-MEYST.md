# LAB04 – Ontologies & Reasoning
**Olivier De Meyst**

---

## Published files

| File | URL |
|------|-----|
| Data (profile.ttl) | <https://olivierdm.github.io/itsme/profile.ttl> |
| Vocabulary (vocabulary.ttl) | <https://olivierdm.github.io/itsme/vocabulary.ttl> |

---

## Part 1 – Data & Ontology extension

`profile.ttl` was extended with four photographs and the persons depicted in them:

| Local name | Subject URI / seeAlso | Depicts |
|---|---|---|
| *(self)* | `https://olivierdm.github.io/itsme/olivier.jpg` | Olivier De Meyst |
| `:arduinoLogo` | `commons.wikimedia.org/…Arduino_Logo_Registered.svg` | *(no person)* |
| `:kanoPortrait` | `en.wikipedia.org/…Portrait_of_late_Mr._Kano.jpg` | Kanō Jigorō |
| `:vanGoghSelfPortrait` | `en.wikipedia.org/...Vincent_van_Gogh_-_Self-Portrait_-_Google_Art_Project.jpg` | Vincent van Gogh |

`vocabulary.ttl` was extended with the following photo-related concepts:

* **`:Photo`** – class for photographs/images
* **`:title`** – name/title of the photo
* **`:photographer`** – creator of the photo
* **`:depicts`** – person(s) shown in the photo
* **`:takenAt`** – location where the photo was taken
* **`:summary`** – human-readable description

---

## Part 2 – FOAF links

The vocabulary was connected to FOAF as follows:

```turtle
:Person   rdfs:subClassOf  foaf:Person.
:Photo    rdfs:subClassOf  foaf:Image.
:Organisation  rdfs:subClassOf  foaf:Organization.

:name        rdfs:subPropertyOf foaf:name.
:firstName   rdfs:subPropertyOf foaf:firstName.
:lastName    rdfs:subPropertyOf foaf:familyName.
:knows       rdfs:subPropertyOf foaf:knows.
:depicts     rdfs:subPropertyOf foaf:depicts.
:photographer rdfs:subPropertyOf foaf:maker.
```

---

## Part 3 – Reasoning over my own data

With the RDFS/OWL rules loaded alongside `profile.ttl` and `vocabulary.ttl`, the
reasoner can infer all FOAF-typed triples from the custom-ontology triples:

| Inference chain | Result |
|---|---|
| `:Photo rdfs:subClassOf foaf:Image` + `:olivierPhoto a :Photo` | `:olivierPhoto a foaf:Image` |
| `:Person rdfs:subClassOf foaf:Person` + `:me a :Person` | `:me a foaf:Person` |
| `:depicts rdfs:subPropertyOf foaf:depicts` + `:olivierPhoto :depicts :me` | `:olivierPhoto foaf:depicts :me` |
| `:name rdfs:subPropertyOf foaf:name` + `:me :name "Olivier De Meyst"` | `:me foaf:name "Olivier De Meyst"` |

The target FOAF query therefore returns all the images of persons. Note that the photo of the arduino in my profile is not returned, since it's not a foaf:person.

```
@prefix ns1: <https://olivierdm.github.io/itsme/profile.ttl#>.
@prefix ex: <http://example.com/>.

(ns1:photo-me "Olivier De Meyst") a ex:Result.
(ns1:photo-kano "Kanō Jigorō") a ex:Result.
(ns1:photo-vincent-van-gogh "Vincent van Gogh") a ex:Result.
```
---

I also mapped :url to foaf:page in case someone wanted to be able to grab foaf:page information about my pictures.

The query needs only minor adjustment:
```
@prefix ex: <http://example.com/>.
@prefix foaf: <http://xmlns.com/foaf/0.1/>.

{
  ?picture a foaf:Image;
    foaf:page ?url;
    foaf:depicts ?person.
  ?person a foaf:Person;
      foaf:name ?name.
} => {
  (
    ?picture
    ?name
    ?url
  ) a ex:Result.
}.
```

Which returns:
```
@prefix ns1: <https://olivierdm.github.io/itsme/profile.ttl#>.
@prefix ex: <http://example.com/>.

(ns1:photo-me "Olivier De Meyst" <https://olivierdm.github.io/itsme/olivier.jpg>) a ex:Result.
(ns1:photo-kano "Kanō Jigorō" <https://en.wikipedia.org/wiki/Kan%C5%8D_Jigor%C5%8D#/media/File:Portrait_of_late_Mr._Kano.jpg>) a ex:Result.
(ns1:photo-vincent-van-gogh "Vincent van Gogh" <https://en.wikipedia.org/wiki/Self-portrait_%28van_Gogh,_Paris%29#/media/File:Vincent_van_Gogh_-_Self-Portrait_-_Google_Art_Project.jpg>) a ex:Result.

```

## Part 4 – Reasoning over other people's data

### What happens when you try the FOAF query on other people's data?

I tested the query on the data of the following people:
- https://driesver.github.io/ugent-knowledge-graphs/data_lab2.ttl
- https://seppesantens.github.io/rdf-lab2/data.ttl 

For both students, the same query also returned the desired results. Dries chose to use the image urls as the subject of his photo's. Seppe took the same approach as me.

Dries' query results:
```
@prefix ns1: <http://example.com/>.

(<https://vtk.ugent.be/media/thumbs/external/praesides_photos/praesidiumfoto2425.jpg.420x594_q85_crop-smart_upscale.jpg> "Dries Verhoeve") a ns1:Result.
(<https://pietercolpaert.be/img/pc.jpg> "Pieter Colpaert") a ns1:Result.
(<https://driesver.github.io/ugent-knowledge-graphs/paraglide.jpg> "Dries Verhoeve") a ns1:Result.
(<https://driesver.github.io/ugent-knowledge-graphs/neuschwanstein.jpg> "Unknown paraglider") a ns1:Result.
```

Seppe's query results:
```
@prefix : <https://seppesantens.github.io/rdf-lab2/data.ttl#>.
@prefix ex: <http://example.com/>.

(:photo-me "Seppe Santens") a ex:Result.
(:photo-TimBernersLee "Tim Berners-Lee") a ex:Result.
```

### How does this show semantics in action?

This is a concrete demonstration of **semantic interoperability**.  Two datasets can
describe the same real-world things (photos, people) using entirely different
vocabularies, and yet a single query can retrieve consistent results across both —
provided the vocabularies declare their *meaning* by relating their concepts to a
shared ontology via `rdfs:subClassOf` and `rdfs:subPropertyOf`.

Without those semantic declarations the data is just syntax: `:Photo` and
`:Photograph` are unrelated identifiers.  With them, the reasoner can bridge the
terminological gap automatically.  The meaning (semantics) is encoded in the
ontology, not in the data itself.

### What can we use this for?

1. **Data integration** – organisations that independently publish RDF data in their
   own vocabularies become queryable through a common ontology (FOAF, Schema.org,
   Dublin Core, …) without modifying the original data.

2. **Federated search** – a single SPARQL/N3 query can span multiple Linked Open
   Data sources once their ontologies share a common alignment, enabling e.g.
   cross-institutional research profiles or bibliographic searches.

3. **Graceful evolution** – if a data publisher refactors their vocabulary, they can
   keep the FOAF alignment and existing consumers continue to work unmodified.

4. **Semantic Web / LOD cloud** – this is the foundational mechanism behind the
   Linked Open Data cloud: heterogeneous datasets become interoperable purely
   through ontology alignment, without requiring a centralised schema or ETL
   pipeline.
