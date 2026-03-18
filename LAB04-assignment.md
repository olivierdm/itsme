Lab 4: Ontologies & Reasoning
Part 1: Update your data and ontology

Extend your data and vocabulary turtle files from the previous lab sessions to describe several photographs found on the Web. Make sure that one of the examples is a description of a photo of yourself (you don't have to use a real photo if you prefer not to). Link these to the data you already have using your own ontology, which will have to be extended accordingly.

You're free to create the concepts that you find important to describe photos, but definitely include:

    title/name of the photo

    author/creator/photographer of the photo

    people depicted in the photograph

    the place/location where the photograph was taken

    a description/summary of the photograph

Hint: work on this task iteratively by alternating between growing your data and the ontology.
Part 2: Link to an existing ontology

In the second part, no changes to the data will be made. It stays as it is: modeled in your own ontology.

Instead, expand your ontology to connect your ontology's concepts to concepts from FOAF. You can use the Turtle version of the FOAF ontology or the FOAF specification to find the relevant concepts. The course notes show examples of how you can connect your predicates and classes to FOAF.

Hint: important terms include rdfs:subClassOf and rdfs:subPropertyOf
Part 3: Reasoning over your data

The goal of this task is that you will access your own data, modeled in your own ontology, with a query that only uses the FOAF ontology. We use EYE, which is a Notation3 reasoner, without built-in knowledge of RDFS and OWL. We have created an online tool you can use for this exercise, where you can upload files for the reasoner and execute queries.

The target query is:

@prefix ex: <http://example.com/>.
@prefix foaf: <http://xmlns.com/foaf/0.1/>.
{
  ?picture a foaf:Image;
    foaf:depicts ?person.
  ?person a foaf:Person;
      foaf:name ?name.
} => {
  (
    ?picture
    ?name
  ) a ex:Result.
}.

Notation3 reasoners express queries as N3 rules, but the above closely resembles a SPARQL CONSTRUCT query (and works in a similar way).

Hint: work on this task iteratively by alternating between Task 2 and 3. Don't start from the full query, but start with more simple queries (with fewer constraints) to see the result of the changes to your ontology.

Hint: EYE has no built-in knowledge of RDFS and OWL. You will need to load it together with your data and ontology. Rules that model common RDFS and OWL constructs are available here.
Part 4: Reasoning over other people’s data

Now load other people's data and ontology, and send them to the reasoner together with your data and ontology.

    What happens when you try the above FOAF query?

    (How) does this show semantics in action?

    What can we use this for?

Submission requirements

Your data and ontology turtle files need to be published online, as described in Lab 2. Your named nodes do not have to be dereferenceable as is required there though (although it is definitely allowed!).

Submit a markdown file which links to both your data and ontology turtle files. It should also contain your answers to the questions of Part 4.
Grading

Criterion
	

Points

The data and ontology cover the required fields described in Part 1
	

20%

The ontology is linked correctly to FOAF
	

30%

The questions of Part 4 are adequately answered
	

50%