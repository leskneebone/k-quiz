# K-Quiz

## 1. Core idea

**K-Quiz** is a recurring, agent-generated quiz about RDF, knowledge
graphs and the Semantic Web.

The idea began as a "quick quiz" of perhaps ten questions, but evolved
toward a **weekly canonical set of ten questions**. Everyone sees the
same quiz for that week, making it a shared social object:

> "Did you do the K-Quiz this week? I got 7/10."

The agent is used to **generate and check the quiz**, rather than
improvising a different quiz for each respondent.

The quiz should be enjoyable and intellectually interesting rather than
feeling like formal corporate training.

------------------------------------------------------------------------

## 2. What the quiz is for

The quiz should not primarily be thought of as a conventional training
product.

A better model is:

**K-Quiz is an engagement layer**

A participant might:

1.  spend five or ten minutes answering ten questions;
2.  score 7/10;
3.  learn something from the answer explanations;
4.  disagree with or discuss one of the questions;
5.  send the quiz to a colleague;
6.  encounter deeper resources where relevant.

This is marketing by **demonstrating competence rather than asserting
competence**.

Over time, quiz questions can reveal which topics deserve new
explanatory resources. If users repeatedly struggle with `rdfs:domain`,
for example, that is evidence that a concise KurrawongAI explainer on
`rdfs:domain` would be useful.

------------------------------------------------------------------------

## 3. Weekly communal model

The shared weekly quiz is important.

Rather than giving each person a personalised AI-generated quiz, the
agent produces a **canonical weekly quiz**. Once published, those
questions remain fixed for all respondents.

Benefits:

-   scores become comparable;
-   colleagues can discuss particular questions;
-   the quiz becomes a recurring ritual;
-   people have a reason to return each week;
-   old quizzes can form an archive;
-   particular questions may generate useful community discussion.

The desired workplace behaviour is something like:

> "Have you done this week's K-Quiz?"

A possible model is:

-   new quiz published weekly;
-   one scored submission per person/team;
-   answers and explanations shown after submission;
-   the quiz remains associated with that week;
-   no aggressive technological attempt to prevent research or
    collaboration.

Indeed, groups of colleagues doing the quiz together could be a feature
rather than cheating. "Solo" and "Team" scores could conceivably exist
later.

------------------------------------------------------------------------

## 4. Ten questions

Ten questions now seems preferable to six because a score such as
**7/10** is immediately legible and ten questions allow a useful mixture
of subjects and cognitive tasks.

A possible internal recipe --- not labels shown to the user --- is:

1.  RDF fundamentals
2.  Turtle / syntax
3.  vocabulary knowledge
4.  complete the triple
5.  spot the problem
6.  RDFS / OWL reasoning
7.  SHACL / SPARQL / tooling
8.  broader Semantic Web ecosystem
9.  short-answer or puzzle
10. a deliberately difficult, surprising or orthogonal question

The categories should **not be displayed in the interface**. They are
editorial/generation machinery. Regular users may notice patterns. That is fine and
may become part of the ritual.

In particular, users might gradually learn that question 10 is
dangerous:

> "How'd you go?"\
> "Eight."\
> "Same. Bloody question ten."

Internally, question 10 might be called the **challenge question** or
**stretch question**.

It should preferably be difficult because it requires reasoning, not
because it depends on obscure factual recall.

------------------------------------------------------------------------

## 5. Domain versus question type

An important modelling distinction emerged.

### Domain

The **domain** describes *what knowledge the question concerns*.

Examples:

-   RDF
-   RDFS
-   OWL
-   Turtle
-   SPARQL
-   SHACL
-   SKOS
-   Linked Data
-   RDF modelling
-   datatypes
-   named graphs
-   JSON-LD
-   DCAT
-   PROV-O
-   SOSA/SSN
-   ODRL
-   schema.org

### Question type

The **question type** describes *what cognitive operation the respondent
is being asked to perform*.

Examples:

-   identify
-   select
-   complete
-   diagnose
-   infer
-   repair
-   compare
-   explain
-   classify

These dimensions are orthogonal. An OWL question could ask the
respondent to infer something, diagnose a modelling problem, select a
correct statement, or explain a result.

------------------------------------------------------------------------

## 6. Response format is another dimension

"Multiple choice" is not really a question type in the same sense as
"spot the problem".

A **spot-the-problem** question could use either multiple choice or free
text.

Therefore response format should be modelled separately.

Possible response formats:

``` text
ResponseFormat
    MultipleChoice
    MultipleSelect
    Boolean
    ShortText
    StatementCompletion
    CodeOrRDF
```

Example:

``` text
Question
├── hasDomain → OWL
├── hasQuestionType → DiagnoseProblem
├── hasResponseFormat → MultipleChoice
└── hasAssessmentMethod → ExactMatch
```

The same cognitive task could instead be:

``` text
Question
├── hasDomain → RDFModelling
├── hasQuestionType → DiagnoseProblem
├── hasResponseFormat → ShortText
└── hasAssessmentMethod → AgentAssessment
```

------------------------------------------------------------------------

## 7. Assessment method is yet another dimension

Response format and assessment method should not be conflated.

Possible assessment methods:

``` text
AssessmentMethod
    ExactMatch
    SetMatch
    PatternMatch
    GraphEquivalence
    AgentAssessment
```

Examples:

-   A single multiple-choice answer can use `ExactMatch`.
-   A multiple-select question can use `SetMatch`.
-   A short textual response might use `AgentAssessment`.
-   An RDF repair question could potentially use `GraphEquivalence`.

`GraphEquivalence` is particularly interesting because a respondent
could produce Turtle that differs textually from the model answer but
represents the intended RDF graph.

This would allow K-Quiz to assess RDF as RDF rather than merely
comparing strings.

------------------------------------------------------------------------

## 8. Feedback is distinct from assessment

Scoring and feedback should also be considered separately.

An exact-match question might produce:

> Correct. `rdfs:subClassOf` relates one class to another.

An agent-assessed response might produce something more nuanced:

> Essentially correct. You identified the subject/object reversal. The
> original statement is not necessarily syntactically invalid, but it is
> semantically suspicious given the apparent intended meaning.

The explanatory feedback may ultimately be as valuable as the score.

Each answer could include a concise explanation of:

-   why the correct answer is correct;
-   why plausible distractors are wrong or less appropriate;
-   the underlying RDF/Semantic Web principle;
-   optionally, a link to a deeper KurrawongAI resource.

------------------------------------------------------------------------

## 9. Candidate question patterns

### A. Recognition

> Which of these is **NOT** an `owl:Class`?

This could be generated from real ontology content rather than a static
trivia bank.

### B. Statement completion

Complete a statement by selecting the most sensible object:

``` turtle
ex:class ex:predicate ???
```

Or, more practically:

> You need to state that a dataset conforms to a particular standard.
> Which predicate is the best fit?

Possible answers:

-   `dcterms:conformsTo`
-   `dcterms:relation`
-   `schema:citation`
-   `rdfs:seeAlso`

The explanation should address why the distractors are plausible but
inferior.

### C. Graph literacy

Given:

``` turtle
ex:alice a schema:Person ;
    schema:knows ex:bob .

ex:bob a schema:Person .
```

Ask:

> Which statement can we safely make from this graph?

Distractors could expose assumptions about symmetry, uniqueness or
closed-world reasoning.

### D. Spot the modelling problem

Present several snippets, only one of which contains a significant
problem.

For example:

``` turtle
ex:thing dcterms:created "2025-04-01"^^xsd:date .
```

Compare this with alternatives involving:

-   an inappropriate datatype;
-   class/property confusion;
-   subject/object reversal;
-   literal versus IRI confusion;
-   misuse of domain/range;
-   a semantically inappropriate predicate.

### E. Entailment / inference

Given:

``` turtle
ex:Dog rdfs:subClassOf ex:Mammal .
ex:fido a ex:Dog .
```

Ask:

> Which additional triple follows?

This can scale from elementary RDFS to considerably harder OWL
reasoning.

### F. Vocabulary selection

> Which vocabulary would you most naturally use to describe provenance?

Or present a modelling requirement and ask the respondent to choose
among SKOS, PROV-O, DCAT, SOSA, ODRL, etc.

### G. Fix the Turtle

Given:

``` turtle
ex:thing
    dcterms:title "My dataset"
    dcterms:creator ex:les .
```

Ask:

> There is one syntax error. Fix it.

An agent or parser could assess the submitted correction rather than
relying on an exact text match.

### H. What is wrong with this statement?

Given:

``` turtle
ex:Person a ex:les .
```

Ask the respondent to explain the likely problem.

A good response should distinguish between **syntactic validity** and
**semantic/intended modelling plausibility**.

### I. Conceptual challenge

For example:

> Two IRIs identify the same real-world thing. What, if anything, does
> RDF itself allow you to conclude?

This is ideal challenge-question territory: apparently technical, but
leading into identity, semantics and epistemology.

RDF has the delightful property that one can begin with a
computer-science question and end up asking:

> What is a concept, exactly?

And mean it seriously.

------------------------------------------------------------------------

## 10. How obscure may the subject matter be?

Not every question needs to be limited to RDF/RDFS/OWL, but specialist
knowledge should normally not be a prerequisite for answering.

A useful three-ring model:

### Core knowledge

Generally fair game without additional introduction:

-   RDF
-   RDFS
-   OWL
-   Turtle
-   SPARQL
-   SHACL
-   SKOS
-   common XSD datatypes
-   basic Linked Data concepts

### Semantic Web working knowledge

Reasonable territory, but questions may need enough context to support
reasoning:

-   DCAT
-   PROV-O
-   Dublin Core Terms
-   schema.org
-   SOSA/SSN
-   ODRL
-   JSON-LD
-   RDF-star
-   named graphs

### The wider ecosystem

Examples:

-   RiC-O
-   GeoSPARQL
-   QUDT
-   OWL-Time
-   BIBFRAME
-   CIDOC CRM

These can make excellent questions if the question does not merely test
whether the respondent happens to know a specialist vocabulary.

Bad:

> What is the range of `rico:hasOrHadPhysicalLocation`?

This is essentially a lookup test.

Better:

> RiC-O is an ontology developed by the International Council on
> Archives. Which of these would you **least** expect it to model?
>
> A. Records\
> B. Agents\
> C. Places\
> D. Chemical elements

A respondent can reason toward the answer while also learning that RiC-O
exists.

### Editorial principle

> **Specialist knowledge may be the subject of a question, but should
> not normally be a prerequisite for answering it.**

Occasional deliberate exceptions are acceptable, particularly for the
challenge question.

------------------------------------------------------------------------

## 11. Difficulty

Difficulty should not simply mean increasingly obscure terminology.

A possible progression:

### Easy --- RDF literacy

-   triples
-   IRIs
-   literals
-   Turtle
-   `rdf:type`
-   elementary RDFS

### Medium --- RDF modelling

-   domain/range
-   SKOS
-   datatypes
-   blank nodes
-   named graphs
-   DCAT
-   inference
-   selecting appropriate predicates

### Hard --- semantics and judgement

-   OWL semantics
-   open-world assumption
-   identity
-   entailment
-   SHACL versus OWL
-   property characteristics
-   modelling alternatives
-   subtle semantic traps

The ideal weekly difficulty distribution should make a strong RDF
practitioner likely to score somewhere around 6--8, with 10/10 feeling
genuinely impressive.

------------------------------------------------------------------------

## 12. Agent-generated, but editor-checked

Generating RDF questions is easy.

Generating a question with **four plausible answers where exactly one is
defensibly correct** is much harder --- particularly in a community
capable of debating the semantics of a predicate for 45 minutes.

Therefore quiz production should probably involve at least two agent
roles/passes:

1.  **Generator** --- creates candidate questions according to the
    week's constraints.
2.  **Editor/checker** --- challenges each question, checks sources and
    semantics, looks for multiple defensible answers, verifies
    distractors, and rejects ambiguity.

Human editorial approval may also remain desirable, especially early on.

The agent should generate a **deliberate quiz as a whole**, not merely
ten independently generated questions.

------------------------------------------------------------------------

## 13. Towards a K-Quiz ontology

The ontology could describe questions independently of the application
used to render them.

An early sketch (prefixes updated to match the repository structure):

``` turtle
@prefix kquiz: <http://example.org/kquiz#> .
@prefix ex: <http://example.org/kquiz/example/> .
@prefix kquizdom: <http://example.org/kquiz/vocab/domain/> .
@prefix kquizqtype: <http://example.org/kquiz/vocab/qtype/> .
@prefix kquizrformat: <http://example.org/kquiz/vocab/rformat/> .
@prefix kquizassessmethod: <http://example.org/kquiz/vocab/assessmethod/> .
@prefix kquizdifficulty: <http://example.org/kquiz/vocab/difficulty/> .
@prefix skos: <http://www.w3.org/2004/02/skos/core#> .
@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .
@prefix schema: <http://schema.org/> .
@prefix dcterms: <http://purl.org/dc/terms/> .
@prefix owl: <http://www.w3.org/2002/07/owl#> .

ex:q27
    a kquiz:Question ;
    kquiz:domain kquizdom:RDFS ;
    kquiz:questionType kquizqtype:Inference ;
    kquiz:responseFormat kquizrformat:MultipleChoice ;
    kquiz:assessmentMethod kquizassessmethod:ExactMatch ;
    kquiz:difficulty kquizdifficulty:Medium ;
    kquiz:prompt "Given the following statements, which additional statement is entailed?" ;
    kquiz:hasOption ex:q27-a, ex:q27-b, ex:q27-c, ex:q27-d ;
    kquiz:correctOption ex:q27-c .
```

An important modelling instinct is to avoid immediately creating a large
subclass hierarchy such as:

``` text
MultipleChoiceQuestion
ShortAnswerQuestion
InferenceQuestion
OWLInferenceMultipleChoiceQuestion
...
```

The dimensions overlap and would quickly create a combinatorial class
hierarchy.

Instead, a `kquiz:Question` can have independently specified:

-   domain
-   question type
-   response format
-   assessment method
-   difficulty

The controlled values could initially be SKOS concepts where
appropriate.

This makes agent constraints expressive:

> Generate ten questions using at least five different question types
> and four different domains; use no more than seven multiple-choice
> response formats; include exactly one agent-assessed response; avoid
> subjects overrepresented in the previous three quizzes.

------------------------------------------------------------------------

## 14. Potential longer-term capabilities

Without needing these in the first prototype, the model could later
support:

-   empirical question difficulty based on response statistics;
-   avoidance of recently used questions/topics;
-   balancing domains across weeks;
-   solo versus team participation;
-   an archive of weekly quizzes;
-   guest-curated quizzes;
-   links from questions to explanatory resources;
-   quiz generation from a supplied ontology, profile or dataset;
-   organisation-specific quizzes;
-   graph-aware assessment of RDF answers;
-   analytics revealing common areas of misunderstanding.

A particularly interesting future mode is:

> **Generate a six or ten question quiz from this ontology/data
> model.**

The agent could inspect a graph and manufacture questions about its
actual classes, properties and modelling decisions.

That moves the concept from a generic RDF quiz generator toward a
**semantic-data training and assessment agent**.

------------------------------------------------------------------------

## 15. Product character

The interface should avoid the aesthetics and language of corporate
training.

Avoid:

-   "learning objectives" on screen;
-   mandatory module progression;
-   completion certificates as the central incentive;
-   artificial countdown pressure;
-   "Congratulations! You have completed Module 3."

Prefer:

> **K-Quiz #1**\
> Ten questions. All things RDF.\
> How much do you know?

The learning should happen almost incidentally through interesting
questions, good explanations and discussion.

The aim is not merely:

> "KurrawongAI provides RDF training."

It is:

> "These people clearly know what they're talking about."

And then, of course, question ten happens.
