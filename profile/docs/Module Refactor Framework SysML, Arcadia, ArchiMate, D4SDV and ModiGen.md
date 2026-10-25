# Module Refactor Framework: SysML, Arcadia, ArchiMate, DE4SDV and ModiGen

Oct 25, 2026 · @Kevin

## Purpose, scope and assumptions

This document merges the module refactor framework (stages 0 to 8, from earlier in this conversation) with five modeling approaches, so that every refactor stage produces a model artifact that can be reviewed and traced. The framework's core rule stays: never change behavior and structure in the same step, and never take a step you cannot verify or undo cheaply.

Three readings of the five names need your confirmation:

- **SysML, Arcadia, ArchiMate** are used in their standard senses: a systems modeling language, the systems-architecture method behind the Capella tool, and the enterprise architecture language.
- **ModiGen** is read as the LLM-based workflow for Modelica code generation, which combines supervised fine-tuning, graph retrieval and a compile-and-validate feedback loop ([arXiv 2503.18460](https://arxiv.org/pdf/2503.18460)). Other tools named Modgen exist, such as a microsimulation language from Statistics Canada, but they do not fit this context.
- **DE4SDV** (Digital Engineering for Software-Defined Vehicle) is read as the open-source project that applies model-based systems engineering to software-defined vehicle product lines, using SysML v2, product-line engineering, digital continuity and continuous compliance ([DE4SDV on GitHub](https://github.com/de4sdv/DE4SDV)). This document uses it as the source of variant, traceability and compliance constraints, and the SysML view can be expressed in SysML v2. The 4SDV approach from TTTech Auto is a separate effort. If this is not the project you meant, tell me and I'll rework section 8.

The models are described in text and tables, not exported from a modeling tool. Treat every element name as a template to adapt.

## The refactor framework in brief

The framework runs nine stages, from defining the boundary to cleanup, and each stage must be behavior-preserving, green before and after, and revertable on its own.

| Stage | Goal | Exit evidence |
| --- | --- | --- |
| 0. Goal and boundary | Why refactor; public surface; consumers; non-goals | Written goal and consumer list |
| 1. Safety net | Characterization, contract and golden-master tests; performance baseline | Green, fast, deterministic suite |
| 2. Seams | Facade, injection points, side effects isolated | Suite green after every commit |
| 3. Strategy | In-place, strangler fig, shadow mode, or expand and contract | Chosen strategy with rationale |
| 4. Ordered execution | Mechanical, extract, parallel structure, move consumers, flag, remove old | One logical change per commit |
| 5. Data and API changes | Expand, migrate, contract | Migration tested forward and back |
| 6. Verification | Unit, contract, integration, end-to-end, non-functional, differential | Levels checked per step |
| 7. Rollout | Ramp, rollback triggers, bake period | Triggers defined before deploy |
| 8. Cleanup | Delete old paths, promote tests, update docs, compare to goal | Goal met or next iteration planned |

The methods below add one thing to this: each stage's decisions are recorded as model elements that can be traced to requirements and to tests.

## How each method contributes

The five methods work at different levels, so they complement more than they overlap: SysML and Arcadia describe the module as a system, ArchiMate places it in the organization, DE4SDV keeps its variants and compliance constraints explicit, and ModiGen generates executable models of it.

| Method | What it is | What it models in a refactor | Main stages |
| --- | --- | --- | --- |
| SysML | Graphical systems modeling language with nine diagram types across structure, behavior, requirements and parametrics ([IEEE](https://technav.ieee.org/topic/systems-modeling-language/)) | Requirements, block structure of the module, state behavior, and links from requirement to block to test | 0, 1, 6 |
| Arcadia | Method with four perspectives: operational analysis, system analysis, logical architecture, physical architecture ([Capella webinar](https://www.bigmarker.com/conferences/fcf432e26eb0/reviews/new)) | Who needs the module, its black-box contribution, its logical decomposition, and its deployment | 0, 2, 3 |
| ArchiMate | Enterprise architecture language with business, application and technology layers, plus strategy, physical, motivation, and implementation and migration elements ([Sparx Systems](https://www.sparxsystems.eu/languages/archimate/)) | Business services that depend on the module, its application components, hosting, and the migration plan | 0, 3, 5, 7 |
| DE4SDV | Open-source digital engineering project for software-defined vehicle product lines, using SysML v2, product-line engineering, digital continuity and continuous compliance ([DE4SDV on GitHub](https://github.com/de4sdv/DE4SDV)) | Variant, traceability and compliance constraints, including safety and security, that the refactor must preserve | 6, 7 |
| ModiGen | LLM workflow that generates Modelica components and test cases, validated by compilation with error feedback ([arXiv](https://arxiv.org/pdf/2503.18460)) | Executable behavior models and tests for simulation-based checks | 1, 6 |

Use only what the module needs. A pure backend library rarely needs DE4SDV or ModiGen; a vehicle or cyber-physical module may need all five.

## Merged workflow: stage, model artifact, test

Each refactor stage produces named model artifacts, and each artifact has a check that decides whether the stage may close. The three architecture methods supply the artifacts; DE4SDV and ModiGen add constraints and generated checks.

| Stage | Model artifacts | DE4SDV and ModiGen | Test or gate |
| --- | --- | --- | --- |
| 0. Goal and boundary | SysML requirement diagram (goals, non-goals) and context diagram; Arcadia operational and system analysis (consumers, module as black box); ArchiMate motivation and dependent services | DE4SDV: list the safety and security requirements allocated to the module | Every consumer found by search and runtime logs appears in the model; every requirement has an id |
| 1. Safety net | SysML test cases linked to requirements by verify; Arcadia functional chains as the basis for characterization tests | ModiGen: generate component models and test cases for behavior that can be simulated; DE4SDV: flag safety-relevant behavior for stronger tests | Suite is green, fast and deterministic; each functional chain has a test |
| 2. Seams | Arcadia logical architecture with a facade component; SysML internal block diagram showing ports at the seam; ArchiMate application interface | DE4SDV: record timing and safety contracts the facade must keep | Suite green after every commit; contract tests on the facade |
| 3. Strategy | ArchiMate baseline, transition and target plateaus; Arcadia physical architecture, old and new; SysML allocation of blocks to deployment | DE4SDV: state the effect on the safety and security case | Strategy matches the risk level; reviewers sign off on the plateau plan |
| 4. Ordered execution | SysML activity diagram of the step order; SysML state machine of the flag; Arcadia chains unchanged | ModiGen: re-run generated simulations after risky steps | One logical change per commit; green before and after; revertable |
| 5. Data and API changes | ArchiMate work packages for expand, migrate, contract; SysML allocation of data elements | DE4SDV: check that data changes keep safety-relevant fields intact | Migration tested forward and back on production-shaped data |
| 6. Verification | SysML verify links complete; parametric diagram for performance limits | DE4SDV: rerun safety and security analyses; ModiGen: simulation-based differential checks | All verification levels pass; old and new outputs match on recorded inputs |
| 7. Rollout | ArchiMate migration plateaus per ramp step; SysML state machine of the ramp (canary, partial, full) | DE4SDV: confirm update-management constraints are met | Rollback triggers defined first; bake period passes without breach |
| 8. Cleanup | Final models updated; transition elements deleted; traceability matrix refreshed | Retire generated checks that no longer apply | Every requirement traces to a passing test; stage 0 goal compared with result |

## SysML view: requirements, blocks, states

SysML captures what the refactor must guarantee, how the module is structured at the seam, and how the rollout moves between states. In an MBSE workflow, requirements are allocated to blocks and traced to verification, giving a chain from need to test result ([IEEE](https://technav.ieee.org/topic/systems-modeling-language/)). The elements below are templates; rename them for your module.

**Requirement diagram.** Each requirement has an id, a satisfying block and a verifying test.

| ID | Requirement | Satisfied by | Verified by |
| --- | --- | --- | --- |
| R1 | Public surface returns the same outputs for the same inputs | Facade | Contract and characterization tests |
| R2 | Every commit can be reverted alone | Process rule | Per-commit CI run on the reverted tree |
| R3 | Latency and resource use stay within the budget set at stage 1 | New implementation | Baseline comparison |
| R4 | Existing consumers need no change until they are migrated | Facade, router | Consumer integration tests |
| R5 | Safety and security constraints are preserved | Facade, new implementation | Re-run of the DE4SDV analyses |

**Block definition diagram** (as indented text; draw it in your SysML tool):

```
block Module
  part facade: Facade          port: public API (the seam)
  part router: Router          holds the flag; routes to old or new
  part legacy: LegacyImpl      unchanged until stage 4 ends
  part next:   NewImpl         built alongside the old one
block Consumer                 external; depends only on Facade
```

**State machine for the rollout flag.** Rollback is a transition from any serving state back to the first state.

| From | Trigger or guard | To |
| --- | --- | --- |
| Legacy only | New implementation passes the verification stage | Shadow |
| Shadow | Mismatch rate stays at or below the agreed limit | Canary |
| Canary | Error and latency triggers not breached for the bake period | Full new |
| Full new | Bake period passes | Legacy removed |
| Shadow, Canary, Full new | A rollback trigger fires | Legacy only |

## Arcadia view: from consumer need to deployment

Arcadia moves top-down through four perspectives, and Capella links elements across them so an impact analysis can follow a change from need to component ([Capella overview](https://us.fitgap.com/products/capella)). For a refactor, the value is a stable record of what the module must do, kept apart from how it is built.

| Level | Question it answers | Content for the module refactor |
| --- | --- | --- |
| Operational analysis | What do the module's users need to accomplish? (no system yet at this level) | Consumers as operational actors; their activities and scenarios that touch the module |
| System analysis | What does the module contribute, seen as a black box? | The module's public surface as system functions and external interfaces; functional chains per use |
| Logical architecture | How is it broken into logical blocks? | Facade, router, old and new logic as logical components; functions allocated to them |
| Physical architecture | Where does it run and in what form? | Deployment of old and new implementations to services, processes or hosts; links and paths between them |

The system analysis level does most of the refactor's work. The functional chains defined there are the behaviors that must not change, so they become the list of characterization tests in stage 1. The logical level holds the target design, and the move from old to new is a re-allocation of the same functions to different components.

After the physical level, Arcadia can describe an end-product breakdown structure, which is a natural home for the list of artifacts a rollout must deliver (builds, migrations, flags).

## ArchiMate view: the module in its surroundings

ArchiMate places the module among the business services that depend on it and the infrastructure that runs it. Its core has business, application and technology layers, extended with strategy, motivation, physical, and implementation and migration elements ([Sparx Systems](https://www.sparxsystems.eu/languages/archimate/)). The implementation and migration elements model a state of transition and mark what is temporary ([Wikipedia](https://wikipedia.com/wiki/ArchiMate)), which is exactly what a refactor is.

| Layer | Elements for the refactor | Question it answers |
| --- | --- | --- |
| Motivation | Driver, goal, requirement for the refactor | Why are we doing this, and what must stay true? |
| Business | Business services and processes that use the module | Who is affected if the module misbehaves? |
| Application | Module as an application component; facade as an application interface; old and new implementations; data objects | Which components and data change, and which stay? |
| Technology | Nodes, system software and artifacts that host old and new implementations | What must be deployed, and where? |
| Implementation and migration | Work packages, deliverables, and plateaus for baseline, transition and target | In what order does the change arrive, and what is temporary? |

Three uses carry most of the value:

1. **Impact analysis at stage 0.** Walk from the module up to the business services that depend on it, so the consumer list includes non-obvious ones.
2. **Plateaus at stage 3.** Model the baseline (old only), transition (both, behind a flag) and target (new only) states, and the work packages between them.
3. **Rollout planning at stage 7.** Map each ramp step to the technology nodes it touches, so rollback means a known, small set of deployments.

## DE4SDV and ModiGen: constraints and generated checks

DE4SDV adds explicit variants and continuous compliance to the refactor, and ModiGen adds generated, executable behavior models. Both apply mainly when the module belongs to a vehicle or another simulatable system.

**DE4SDV.** The project applies model-based systems engineering with SysML v2, product-line engineering, digital continuity, simulation interoperability and continuous compliance, so that variability across domains such as ADAS, telematics, connectivity and core vehicle software is modeled as configurable architectures ([DE4SDV on GitHub](https://github.com/de4sdv/DE4SDV)). For a refactor this means:

| Concern | What to do in the refactor |
| --- | --- |
| Variants | List every product variant that includes the module; the refactor must keep each one valid, not only the default |
| Constraints per variant | Record safety, security and compliance constraints per variant as requirements, and trace them to the blocks that satisfy them |
| Digital continuity | Keep the model, the code and the tests linked, so a changed block shows which requirements and tests it touches |
| Continuous compliance | Run the compliance checks in CI at every stage, not only before release |
| Alternatives | Compare old and new designs as architecture alternatives with a recorded trade-off decision (stage 3) |

**ModiGen.** The workflow targets Modelica, a language for system behavior as equations. It combines supervised fine-tuning, graph retrieval and feedback optimization, and compiles and validates each generated component, returning errors to the model for refinement; reported gains reached 0.3349 in pass@1 for component generation and 0.2457 for test case generation ([arXiv 2503.18460](https://arxiv.org/pdf/2503.18460)). In a refactor it supports two jobs:

1. **Stage 1:** draft Modelica component models and test cases that reproduce the module's current behavior, to be checked against recorded real behavior.
2. **Stage 6:** run old and new behavior models side by side in simulation as one more differential check.

Treat generated models as candidates. A model that compiles can still be wrong, so accept it only when it matches characterization data from the real module. If the module is not modeled in Modelica, borrow only the pattern: generate, compile or validate, feed errors back, repeat.

## Traceability and verification across the models

The models are worth keeping only if one chain links a reason for the refactor to the test that proves it held. A requirement is allocated to blocks and traced to verification, which gives a machine-readable chain from need to test result ([IEEE](https://technav.ieee.org/topic/systems-modeling-language/)). Use the same identifiers in every tool.

| Link | From | To | Recorded in |
| --- | --- | --- | --- |
| Motivates | ArchiMate goal or driver | SysML requirement | Requirement table, with the goal's id |
| Refines | SysML requirement | Arcadia system function or functional chain | Capella traceability link |
| Allocated to | Arcadia function | Logical component (facade, old, new) | Capella allocation |
| Satisfied by | SysML requirement | SysML block | Satisfy relationship |
| Deployed on | Logical component | ArchiMate technology node or Arcadia physical component | Architecture model |
| Verified by | SysML requirement | Test case, such as a characterization or contract test | Verify relationship, test id in CI |
| Constrained by | Requirement or block | DE4SDV variant and compliance constraint | Variant matrix |
| Checked by | Behavior | ModiGen-generated simulation, if used | Test id in CI |

Three coverage rules turn this table into a gate for stage 8:

1. Every requirement has at least one verifying test, and that test passed on the last run.
2. Every functional chain in the system analysis has a characterization test.
3. Every variant that includes the module appears in the variant matrix with a passing result.

When a model and the code disagree, fix the model in the same change that changes the code, so the models never describe a system that no longer exists.

## Pitfalls and adoption checklist

Most problems come from using too many methods on a module that needs few of them.

- **Modeling everything.** Start with the requirements, the system analysis and the seam; add other views only when a decision needs them.
- **Untailored frameworks.** A generic framework left unadapted can produce inconsistent viewpoints; tailor each viewpoint to your stakeholders' concerns ([Capella webinar summary](https://www.bigmarker.com/conferences/fcf432e26eb0/reviews/new)).
- **Models that drift from the code.** Update the model in the same change as the code, and check it in review.
- **Treating generated models as truth.** A generated Modelica model is a candidate until it matches recorded behavior.
- **Mixing behavior and structure.** The framework's core rule still applies: no behavior change in a structural step, whatever the models say.
- **Ignoring variants.** A refactor that passes for the default configuration can break another variant.

Adoption checklist:

- [ ] Decide which of the five methods this module needs, and drop the rest
- [ ] Write the stage 0 requirements and give each one an id
- [ ] Model the module as a black box with its functional chains
- [ ] Turn each functional chain into a characterization test
- [ ] List variants and their constraints, if the module is vehicle-related
- [ ] Model baseline, transition and target states, and the rollout state machine
- [ ] Add the traceability table to CI as a coverage check
- [ ] Define rollback triggers before the first ramp step
- [ ] At cleanup, delete transition elements and compare the result with the stage 0 goal

## Sources

As of 2026-10-25. These pages were read through search results, not downloaded in full. The model templates in this document are illustrative and were not built in a modeling tool. The refactor framework itself comes from earlier in this conversation and is not an external source.

- [DE4SDV: Digital Engineering for Software-Defined Vehicle (GitHub)](https://github.com/de4sdv/DE4SDV)
- [ModiGen: A Large Language Model-Based Workflow for Multi-Task Modelica Code Generation (arXiv 2503.18460)](https://arxiv.org/pdf/2503.18460)
- [TTTech Auto, 4SDV approach (a separate effort)](https://tttech-auto.com/4sdv)
- [Systems Modeling Language, IEEE Technology Navigator](https://technav.ieee.org/topic/systems-modeling-language/)
- [Capella overview, FitGap](https://us.fitgap.com/products/capella)
- [Capella webinar summary on the four Arcadia perspectives](https://www.bigmarker.com/conferences/fcf432e26eb0/reviews/new)
- [Arcadia and Capella training outline, Obeo](https://www.obeosoft.com/en/services/training/capella-training/984-training-capella-details)
- [ArchiMate layers, Sparx Systems](https://www.sparxsystems.eu/languages/archimate/)
- [ArchiMate, Wikipedia](https://wikipedia.com/wiki/ArchiMate)
