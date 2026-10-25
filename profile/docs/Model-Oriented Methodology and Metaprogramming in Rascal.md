# Model-Oriented Methodology and Metaprogramming in Rascal

Oct 25, 2026 · @Kevin

## Purpose and scope

This guide shows how to build software around explicit, typed models using Rascal, a language made for metaprogramming. The method: parse text into a model, then analyze, transform and generate from that model, never from raw text.

**Assumption:** the request said "Rescal". No language of that name turned up, so this guide treats it as **Rascal**, the metaprogramming language from CWI Amsterdam. If you meant a different tool, say so and I'll rework it.

Rascal is a domain-specific language for metaprogramming and language-oriented programming: static code analysis, program transformation, code generation and implementing DSLs ([Wikipedia](https://en.wikipedia.org/wiki/RascalMPL)). Its primitives include immutable data, context-free grammars, algebraic data types, relations, pattern matching, generic traversal and string templates ([SWAT Engineering](https://swat.engineering/rascal)). Its authors name model-driven engineering as one of its prime application areas ([van den Bos et al., 2011](https://arxiv.org/pdf/1107.0064)).

"Model-oriented" here means one rule: every artifact you care about (source code, a specification, a configuration) is first turned into a typed model, and every later step works on that model.

**Caveat:** the code samples are illustrative sketches. They have not been run against a Rascal installation, so check them against the current [Rascal documentation](https://www.rascal-mpl.org) before relying on them.

## The model-oriented methodology

The method is a pipeline of small, separately testable stages, each with a typed input and output. Seven principles keep it healthy:

1. **Model first.** Define the abstract syntax as an algebraic data type before writing any analysis. It is the contract between every stage.
2. **Separate concrete from abstract syntax.** A grammar serves people who type text; the data type serves tools. Convert once, at the edge.
3. **State facts, then query them.** Extract facts from the model into relations, then derive answers with comprehensions and transitive closure.
4. **Transform with pure functions.** A transformation takes a model and returns a model. Data is immutable, so the old model stays available for comparison.
5. **Generate, never hand-edit.** Output text comes from templates fed by the model. Fix the model or the template, not the generated file.
6. **Report problems as data.** Validation returns a set of messages with source locations instead of throwing on the first error.
7. **Test each stage alone.** A stage that cannot run on a ten-line model is too coupled.

A concrete pipeline for a DSL looks like this:

| Stage | Input | Output | Rascal mechanism |
| --- | --- | --- | --- |
| Parse | Text | Parse tree | `syntax` and `lexical` definitions, `parse` |
| Build model | Parse tree | Abstract model | `data` types, `implode` or a hand-written mapping |
| Validate | Model | Set of messages | Relations, comprehensions, pattern matching |
| Transform | Model | Model | `visit`, rewrite rules |
| Generate | Model | Text files | String templates, `writeFile` |

## Rascal building blocks for modeling

A handful of language constructs cover nearly every model-oriented task; each maps to one step of the method.

| Construct | Role in the method | Where you write it |
| --- | --- | --- |
| Concrete grammar | Describes the text people write and yields parse trees | `syntax`, `lexical`, `layout`, `keyword` |
| Algebraic data type | The abstract model, the contract between stages | `data` |
| Keyword parameter | Carries extras such as a source location on every node | `src` on each `data` type |
| Relations, sets, maps | Hold extracted facts, such as which entity references which | `rel`, `set`, `map` |
| Comprehensions | Query the model declaratively | set and list comprehensions |
| Pattern matching | Takes structure apart and tests it | `:=`, typed patterns, `case` |
| `visit` | Walks a nested model and rewrites matches | `visit`, `case ... =>` |
| Transitive closure | Reachability, dependencies, cycles | `+` and `*` on relations |
| String templates | Generate text, with automatic indentation | quoted strings with `<...>` and a `'` margin |
| Messages | Report problems as data with a location | `Message`, `error`, `warning` |
| Tests | Unit and generated-input tests inside the module | `test bool` |

Three lines show the flavor: an abstract syntax, a set of facts, and a query over their closure.

```rascal
data SHAPE = circle(int r) | square(int side);       // abstract syntax
rel[str, str] calls = {<"a", "b">, <"b", "c">};     // facts
set[str] reachable = { y | <"a", y> <- calls+ };    // everything "a" reaches
```

Rascal's own documentation recommends keeping the concrete grammar and the abstract data types in separate modules, with one `Load` module that imports both ([Rascal recipes](https://www.rascal-mpl.org/docs/Recipes/Languages/Exp/Combined/Automatic/)). The worked example follows that layout.

## Worked example: an entity-model DSL

This example takes a tiny modeling language from grammar to generated Java classes in six modules. The input it handles:

```
entity Person { name: string  age: int  employer: Company }
entity Company { name: string }
```

**1. Concrete syntax.** Production labels (`entity:`, `field:`) become constructor names when the tree is imploded, so label every production.

```rascal
module lang::entities::Syntax

layout Layout = [\ \t\n\r]* !>> [\ \t\n\r];
keyword Reserved = "entity" | "string" | "int";
lexical Id = ([a-zA-Z][a-zA-Z0-9]* !>> [a-zA-Z0-9]) \ Reserved;

start syntax Spec = spec: Entity* entities;
syntax Entity = entity: "entity" Id name "{" Field* fields "}";
syntax Field = field: Id name ":" Type typ;
syntax Type = string: "string" | integer: "int" | ref: Id target;
```

**2. Abstract syntax.** Upper-case type names keep the data types distinct from the grammar's non-terminals, the convention used in Rascal's own Pico example ([Pico abstract syntax](https://www.rascal-mpl.org/docs/Recipes/Languages/Pico/Abstract/)). The `src` keyword parameter holds each node's source location.

```rascal
module lang::entities::AST

data SPEC(loc src = |unknown:///|) = spec(list[ENTITY] entities);
data ENTITY(loc src = |unknown:///|) = entity(str name, list[FIELD] fields);
data FIELD(loc src = |unknown:///|) = field(str name, TYPE typ);
data TYPE(loc src = |unknown:///|) = string() | integer() | ref(str target);
```

**3. Load.** The only module that sees both syntaxes, so the only place name clashes can occur.

```rascal
module lang::entities::Load

import ParseTree;
import lang::entities::Syntax;
import lang::entities::AST;

SPEC load(str txt) = implode(#SPEC, parse(#start[Spec], txt).top);
```

**4. Validate and analyze.** Unknown references become messages with locations. Reference facts go into a relation, and transitive closure finds cycles.

```rascal
module lang::entities::Validate

import Message;
import lang::entities::AST;

set[Message] validate(SPEC s) {
  known = { e.name | e <- s.entities };
  return { error("Unknown entity <t>", f.src)
         | e <- s.entities, f <- e.fields, ref(t) := f.typ, t notin known };
}

rel[str from, str to] refs(SPEC s) =
  { <e.name, t> | e <- s.entities, f <- e.fields, ref(t) := f.typ };

set[str] inCycle(SPEC s) = { n | <n, n> <- refs(s)+ };
```

**5. Transform.** A model-to-model rewrite: give every entity an `id` field unless it already has one. The guard makes it idempotent.

```rascal
module lang::entities::Transform

import lang::entities::AST;

SPEC addIds(SPEC s) = visit (s) {
  case ENTITY e => e[fields = [field("id", integer())] + e.fields]
    when !any(f <- e.fields, f.name == "id")
};
```

**6. Generate.** Templates turn each entity into a Java class. Rascal's `'` margin marks where each template line starts, so indentation stays clean.

```rascal
module lang::entities::Generate

import IO;
import lang::entities::AST;

str javaType(string()) = "String";
str javaType(integer()) = "int";
str javaType(ref(t)) = t;

str genClass(ENTITY e) =
  "public class <e.name> {
  '<for (f <- e.fields) {>  private <javaType(f.typ)> <f.name>;
  '<}>}
  ";

void generateAll(SPEC s, loc outDir) {
  for (e <- s.entities)
    writeFile(outDir + "<e.name>.java", genClass(e));
}
```

The driver composes the stages: load, validate, stop if there are errors, otherwise transform and generate. Each module above can be run and tested alone. Rascal's [string template recipe](https://www.rascal-mpl.org/docs/Recipes/Common/StringTemplate/) notes that for complex output it is better to build an abstract model of the target code and render that.

## Metaprogramming patterns

Choose the pattern by what the output must be: facts, a changed model, or text. Rascal's three main application areas are software analysis, software transformation, and DSL design and implementation ([van den Bos et al., 2011](https://arxiv.org/pdf/1107.0064)); the patterns below split those into daily-use moves.

| Pattern | Rascal mechanism | Result | Use it when |
| --- | --- | --- | --- |
| Extract facts | Traverse a tree or model, collect tuples into relations; library front-ends can supply models of existing languages | Relations | Many questions will be asked of the same code or model |
| Query | Comprehensions, pattern matching, transitive closure | Sets, messages | A rule reads as "find every X such that Y" |
| Rewrite in place | `visit` with `case` rules | Same model, changed | One change applies at many nested spots |
| Restructure | Functions over constructors and list patterns | A model of a new shape | Input and output shapes differ |
| Generate text | String templates | Source files | The output is text and needs no grammar of its own |
| Lift to a DSL | Grammar, load function, the patterns above, editor support | A language with tooling | Domain experts should write the model directly |

Two rules keep these patterns reliable. First, keep analysis and transformation separate: a function either computes facts or returns a changed model, not both. Second, give every rewrite a guard that makes it idempotent, so running it twice equals running it once; that property is also easy to test.

## Model transformation and cross-references

Models in Rascal are trees, so a reference from one element to another needs an explicit encoding. Mainstream model-transformation tools work on graph-structured models; a CWI paper explores the functional alternative in Rascal and proposes REFS, a simple encoding and API for cross references between model elements held as data types, evaluated on state machines, meta-models and activity diagrams ([Klint's publications at CWI](https://cwi.academia.edu/PaulKlint)).

Four transformation kinds cover most work:

| Kind | Shape | Rascal approach | Watch out for |
| --- | --- | --- | --- |
| Model to model | New model of a different type | Pure functions over constructors | Losing source locations; copy `src` across |
| Normalization | Same type, simplified | `visit` with guarded cases | Rules that never reach a fixed point |
| Model to text | Model in, files out | String templates | Hand-editing the output |
| Text to model | Text in, model out | Grammar, then `implode` or a hand-written mapping | Concrete and abstract syntax drifting apart |

For cross-references, three encodings fit most models, from simplest to richest:

1. **Reference by name.** Store a key such as `ref("Company")` and resolve it with a relation or map when needed. This is what the worked example does. Validate that keys are unique and that every reference resolves.
2. **Resolve once, pass the table.** Build a `map[str, ENTITY]` at the start of a stage and pass it explicitly, so lookups are cheap and visible.
3. **A reference library.** For models with many reference kinds, adopt a dedicated encoding such as REFS rather than inventing one.

Whatever you choose, treat a rename as a two-sided change: the declaration and every reference must move together.

```rascal
SPEC rename(SPEC s, str oldName, str newName) = visit (s) {
  case ENTITY e => e[name = newName] when e.name == oldName
  case ref(t) => ref(newName) when t == oldName
};
```

After any transformation, run the validator again. A transformation that produces an invalid model is a bug in the transformation.

## Testing metaprograms

Test each stage on its own, with the smallest model that shows the behavior. Rascal tests are `test bool` functions in the module itself, run from the shell with `:test` (see the [course exercise on testing parsers](https://bitbucket.org/sle-uib/inf225/wiki/2017/ex/Exercise%203)).

| Stage | Test | Technique |
| --- | --- | --- |
| Grammar | Valid samples parse; invalid ones are rejected; no ambiguity | `parse` in a `test bool`, expecting an exception for bad input |
| Load | Sample text yields the expected model shape | Compare against a hand-written model value |
| Validate | Each rule fires on its minimal bad case and stays silent on good input | One test per rule |
| Analyze | Relations match a hand-computed answer on a three-entity model | Equality on sets |
| Transform | Result is valid and the rewrite is idempotent | Property tests; re-run the validator |
| Generate | Output matches a stored expected file, and compiles when feasible | Golden-file comparison |
| Round trip | Text to model to text keeps meaning | Compare models after a second parse |

```rascal
test bool unknownRefIsReported() =
  validate(load("entity A { b: B }")) != {};

test bool validModelHasNoMessages() =
  validate(load("entity A { n: string }")) == {};

test bool addIdsIsIdempotent() {
  s = load("entity A { n: string }");
  return addIds(addIds(s)) == addIds(s);
}
```

Keep a small corpus of real inputs next to the tests and run the full pipeline over it on every change. The corpus catches what small tests miss, such as odd whitespace, long names and deep nesting.

## Project structure, workflow and IDE support

One module per pipeline stage keeps each stage replaceable and testable. The folder where modules live depends on your project setup, so only the package layout is shown:

```
lang/entities/
  Syntax.rsc      concrete grammar only
  AST.rsc         abstract data types only
  Load.rsc        parse + implode; the only module importing both
  Validate.rsc    messages and relations
  Transform.rsc   model-to-model rewrites
  Generate.rsc    templates and file output
  Main.rsc        composes the stages
  Tests.rsc       test bool functions
```

A reliable working order for a new language:

1. Write three or four sample inputs, including one that should be rejected.
2. Write the grammar until all samples parse as expected.
3. Define the abstract data types and the load function; print a loaded sample and read it.
4. Add validation rules one at a time, each with its test.
5. Add transformations, then generation, each against stored expected output.
6. Wire the stages in `Main` and add the sample corpus run.

For editor support, Rascal can derive Eclipse plugins and VS Code extensions based on the Language Server Protocol for a language implemented in Rascal ([Wikipedia](https://en.wikipedia.org/wiki/RascalMPL)). That gives your DSL users highlighting, error markers and navigation without a separate tooling project. Check the library documentation for the current registration steps, since they may differ between releases.

## Pitfalls and adoption checklist

Most trouble comes from five places:

- **Ambiguous grammars.** Overlapping lexical rules, such as keywords that also match identifiers, produce ambiguity. Use `keyword` declarations, reject reserved words from identifiers, and add follow restrictions (`!>>`).
- **Concrete and abstract syntax drifting apart.** `implode` depends on type, constructor and field names matching the grammar. The mapping is brittle; Rascal's own maintainers have discussed this problem ([discussion on GitHub](https://github.com/cwi-swat/rascal/issues/470)). Write a hand-made mapping when the shapes differ.
- **Lost source locations.** Rebuilding a node from scratch drops its `src`. Update fields on the existing node instead.
- **Exceptions as error handling.** A parse error throws, but validation errors should be returned as messages so users see all problems at once.
- **Hand-edited generated files.** The next run overwrites them. Change the model or the template.

Adoption checklist for a new model-oriented project:

- [ ] Name the model's purpose and its three most important queries
- [ ] Collect sample inputs, including invalid ones
- [ ] Separate grammar, data types and load modules
- [ ] Return validation problems as messages with locations
- [ ] Give every rewrite a guard and an idempotence test
- [ ] Generate from templates; mark output as generated
- [ ] Run the sample corpus through the full pipeline in CI

## Sources

As of 2026-10-25. These pages were read through search results, not downloaded in full. The code samples follow the patterns in the Rascal documentation but were not executed.

- [Rascal: From Algebraic Specification to Meta-Programming (van den Bos, Hills, Klint, van der Storm, Vinju, 2011)](https://arxiv.org/pdf/1107.0064)
- [RascalMPL overview, Wikipedia](https://en.wikipedia.org/wiki/RascalMPL)
- [Rascal language summary, SWAT Engineering](https://swat.engineering/rascal)
- [Rascal recipe: Exp, automatic combination with implode](https://www.rascal-mpl.org/docs/Recipes/Languages/Exp/Combined/Automatic/)
- [Rascal recipe: Pico Load](https://rascal-mpl.org/docs/Recipes/Languages/Pico/Load)
- [Rascal recipe: Pico Abstract syntax](https://www.rascal-mpl.org/docs/Recipes/Languages/Pico/Abstract/)
- [Rascal recipe: String Template](https://www.rascal-mpl.org/docs/Recipes/Common/StringTemplate/)
- [Paul Klint's publications at CWI, including the REFS model-transformation paper](https://cwi.academia.edu/PaulKlint)
- [Exercise 3, Rascal testing and parsing (INF225)](https://bitbucket.org/sle-uib/inf225/wiki/2017/ex/Exercise%203)
- [GitHub issue 470, on implode and separate ADTs](https://github.com/cwi-swat/rascal/issues/470)
