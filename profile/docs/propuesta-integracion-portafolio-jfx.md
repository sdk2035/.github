# Component Integration Proposal for the JFX Portfolio

**Review:** October 17, 2026, Lima time (original inventory: October 4).  
**Scope:** restructuring of the file “propuesta categorizada estratégicamente.txt” and an integration proposal.  
**Result:** 89 references classified: the 69 original references plus llm-d, Panel, Wings, seven unique references supplied on October 16, GLM-5, seven references from the October 17 attachment and two additional catalog references supplied on October 17. CLI Agent Orchestrator was already categorized and is not a new entry. The catalog comprises 87 external components, tools or resources and two references to host projects. Not every item is an installable package.

> Status update: completed work in the [JFXAI4ARCH technical RAG pilot](https://github.com/robotics-intelligent-systems/jfxai4arch/blob/main/docs/rag/PILOT.md) has been removed from the pending actions below. Catalog entries remain as component references. Other integrations remain proposals unless implementation evidence is cited.
>
> Catalog update (October 16): seven unique references from the supplied lists were added to §§5.6 and 5.8. CLI Agent Orchestrator was already listed in §5.8, so no duplicate was added. The new entries remain optional candidates, not adopted dependencies.
>
> Catalog update (October 17): GLM-5 was added to §5.6 as a P2 evaluation candidate; this does not imply model deployment or adoption.
>
> Catalog update (October 17, attached list): six GitHub repositories and the NIST DELS catalog were added under §5.14; SysMD, SysML Repo Modeler and SYSMOD are P2 experiments, while the other entries are references.
>
> Catalog update (October 17, additional references): FastGPT remains a P2 evaluation candidate in §5.6 and sdk2035/requests an R interoperability reference in §5.9. They remain candidates or references, not adopted dependencies.

## 1. Main Recommendation

Organize adoption around **shared services and domain-specific adapters**. Each JFX project should retain its models, data and decisions; AI infrastructure can be reused without turning the entire portfolio into a single application or requiring every component to run within GraalVM.

The next step is to validate the existing technical RAG baseline in **JFXAI4ARCH** with an approved corpus, a real local model and a live Langfuse backend, then demonstrate a domain integration in **JFXAI4DIA**, **JFXLMS4AIR** or **JFXFMIS**, depending on available data and teams.

The destinations and priorities in this document are **architecture recommendations**, inferred from the projects' published scope. They do not imply implemented integrations, except for the explicitly documented JFXAI4ARCH baseline. The review checked repository metadata and READMEs for the main destinations and ambiguous components; it did not compile or audit all the code.

## 2. Corrections to the Original List

| Reference | Corrected classification | Implication for the portfolio |
| --- | --- | --- |
| `jfx-meta`, `jfx-embodied-ai`, `jfx-ai-infra`, `jfx-eval-tools`, `jfx-julia`, `jfx-edge-hardware`, `jfx-enterprise-copilots` | Thematic labels in the attachment, not confirmed repositories | Use them as categories; assign ownership to existing projects |
| `jfxai4agri` | Did not appear in the robotics inventory consulted | Use JFXFMIS as the agricultural host; propose a new repository only if justified later |
| `robotics-intelligent-systems/jfxlcdp` | Unconfirmed path; the verified project is [sdk2035/jfxlcdp](https://github.com/sdk2035/jfxlcdp) | Keep the boundary between robotics and the sdk2035 foundations explicit |
| JFXAI4BSS | Its [current README](https://github.com/robotics-intelligent-systems/jfxai4bss) covers buildings, infrastructure and smart communities; the short description retains an older greenhouse reference | Assign timber construction and the built environment to BSS; agronomy to FMIS |
| [TiMBA](https://github.com/sdk2035/TiMBA) | Economic model of forest-product markets | Supply, demand and trade scenarios, separate from robotic manufacturing |
| [PixSim](https://github.com/sdk2035/PixSim) and [TreeSim](https://github.com/sdk2035/TreeSim) | Pixel-based forest growth and individual-tree simulation | Forestry models; their inclusion does not demonstrate synthetic visual dataset generation |
| [hyperwood-bench](https://github.com/sdk2035/hyperwood-bench) | Design of a wooden bench | CAD/CAM demonstrator, not a structural test bench |
| [Juliana.jl](https://github.com/sdk2035/Juliana.jl) | Translation from CUDA.jl to KernelAbstractions.jl | GPU portability in Julia; the Java–Julia bridge is a different component |
| [modular-backpack](https://github.com/sdk2035/modular-backpack) and [Backpack](https://github.com/sdk2035/Backpack) | Voron toolchanger hardware and ExpressLRS firmware, respectively | Two different uses; neither should be presented as a generic sensor backpack |
| [SpeedSeed3](https://github.com/sdk2035/SpeedSeed3) | Accelerated-growth chamber | Experimental environmental automation; do not classify it as a seeder or a demonstrated phenotypic-selection system |
| [chamelion](https://github.com/sdk2035/chamelion) | LiDAR change detection | Direct fit for JFXLMS4AIR, not metaprogramming |
| [Relevance AI](https://github.com/sdk2035/relevanceai) | SDK for accessing an external platform | Optional connector requiring an account and service; it does not replace a self-hosted platform |
| [open-jev](https://github.com/sdk2035/open-jev) and [AnyJev](https://github.com/sdk2035/AnyJev) | Scoring/decision implementations | Candidate evaluation components, not universal benchmark suites |
| [ABAP basic trial](https://github.com/sdk2035/abap-platform-basic-trial) | Workshop material and SAP environment setup | Training reference; a public repository does not make it a redistributable SAP runtime |

## 3. Recommended Host Projects

| Verified project | Proposed responsibility | First useful deliverable |
| --- | --- | --- |
| [JFXAI4ARCH](https://github.com/robotics-intelligent-systems/jfxai4arch) | Shared RAG services, agent tools, inference and traceability | Validate the existing local RAG baseline on approved documents and a real model; verify live telemetry |
| [JFXAI4NLP](https://github.com/robotics-intelligent-systems/jfxai4nlp) | Language extraction, code intelligence and evaluation | Labeled dataset and comparison of extractors/evaluators |
| [JFXAI4DIA](https://github.com/robotics-intelligent-systems/jfxai4dia) | CAD artifact generation and validation | Text to candidate STEP, geometry validation and visualization |
| [JFXLMS4AIR](https://github.com/robotics-intelligent-systems/jfxlms4air) | Localization, mapping and perception for inspection | Change detection across two versioned LiDAR captures |
| [JFXFMIS](https://github.com/robotics-intelligent-systems/jfxfmis) | Farm operations, crop models and telemetry | Soil–crop–water scenarios with AquaCrop-OSPy |
| [JFXAI4BSS](https://github.com/robotics-intelligent-systems/jfxai4bss) | Built environment, infrastructure and building twins | Exchange of a timber-frame model and its evidence |
| [JFXOSMS](https://github.com/robotics-intelligent-systems/jfxosms) | Microfactory, manufacturing and quality inspection | Visual inspection result linked to a part/batch |
| [JFXSCADA](https://github.com/robotics-intelligent-systems/jfxscada) | Assets, points, alarms and telemetry histories | Sensor ingestion with units, timestamps and data quality |
| [JFXRTESS](https://github.com/robotics-intelligent-systems/jfxrtess) | Firmware, embedded test benches and integration testing | Reproducible board build and bench results |
| [JFXAI4CV](https://github.com/robotics-intelligent-systems/jfxai4cv) and [JFXAI4RSS](https://github.com/robotics-intelligent-systems/jfxai4rss) | Medical imaging and surgical simulation for research | Reviewed volumetric masks for a simulation scenario |
| [JFXAI4BPM](https://github.com/robotics-intelligent-systems/jfxai4bpm), [JFXAI4CRM](https://github.com/robotics-intelligent-systems/jfxai4crm) and [JFXAI4OHS](https://github.com/robotics-intelligent-systems/jfxai4ohs) | Processes, business relationships and purchasing | Enterprise query adapter with contracts and auditing |
| [sdk2035/jfxlcdp](https://github.com/sdk2035/jfxlcdp) | Complementary foundation for authoring and declarative UI | Forms for running and comparing domain jobs |
| [sdk2035/jfxengine](https://github.com/sdk2035/jfxengine) and [sdk2035/jfxmodelica](https://github.com/sdk2035/jfxmodelica) | Complementary visualization and simulation foundations | Consume artifacts and results through open contracts |
| [sdk2035/jfxlegacy2modern](https://github.com/sdk2035/jfxlegacy2modern) | Complementary foundation for code modernization | A traceable Java or ABAP case, with before/after tests |

**Other potential consumers:** JFXAI4RS for forest maps; JFXAI4CBS for evaluating agent execution policies; JFXLMS for educational material. These assignments are proposed extensions based on public descriptions, not verified capabilities.

## 4. Proposed Integration Architecture

```mermaid
flowchart TB
    ui["JFX experience: forms and visualization"] --> domains["Domain project APIs"]
    domains --> jobs["Versioned jobs and artifacts"]
    jobs --> cad["CAD: STEP-LLM, pyplasm, COMPAS Timber"]
    jobs --> robot["Mapping and inspection: Chamelion, Habitat, IVIS"]
    jobs --> agro["Agriculture and forests: AquaCrop, PixSim, TreeSim"]
    jobs --> enterprise["Enterprise adapters: ABAP and Etendo"]
    domains <--> knowledge["JFXAI4ARCH: RAG, models and tools"]
    knowledge --> execution["Bounded tool execution"]
    cad --> evidence["Results, provenance and domain validation"]
    robot --> evidence
    agro --> evidence
    enterprise --> evidence
    evidence --> review["Host project review and decision"]
    review --> ui
    knowledge -. Traces .-> obs["Shared observability and evaluation"]
    jobs -. Metrics .-> obs
```

The architecture proposes three connection approaches, depending on the component:

1. **In-process library:** for compatible, bounded JVM components, such as a Java extractor, after checking dependencies and licensing.
2. **Separate worker or service:** the initial option for scientific Python, R, Julia, GPU workloads and ERP systems. Returns job identifiers, artifacts and diagnostics; avoids making the entire UI depend on a native runtime.
3. **Development tool or documentation resource:** C# specifications, IDEs, examples and hardware designs. These integrate into the workflow or catalog, not the production runtime.

**Proposed initial contracts** — design names, not existing APIs:

| Contract | Minimum fields | Use |
| --- | --- | --- |
| Job | `job_id`, project, operation, inputs, versions, status, error | CAD generation, simulation and analysis |
| Artifact | `artifact_id`, URI, hash, format, units, coordinate system, provenance | STEP, point clouds, maps, masks and results |
| Evidence | test case, expectation, result, tolerance, version, reviewer | Technical acceptance independent of the LLM |
| Telemetry | asset, signal, unit, timestamp, quality, schema version | FMIS, SCADA and RTESS test benches |
| Agent tool | name, input/output schema, permissions, timeout, idempotency | MCP or operation adapters |

Models propose or explain; geometry validators, code tests and simulations check the result. Hardware commands remain within the corresponding project's control interfaces.

## 5. Complete Restructured Catalog

**Priorities:** P0 = candidate foundation for the first pilot; P1 = next domain integration; P2 = experiment conditional on a specific need; R = reference, demonstrator or development tool; H = host project. P0 does not mean an already approved dependency.

In each row, the function comes from the repository consulted; the destination and connection approach are this analysis's proposal. Upstream links identify the fork's declared provenance, without claiming that the two are synchronized.

### 5.1. Metaprogramming and Models

| Component and provenance | Function | Proposed destination | Connection and adoption condition | Priority |
| --- | --- | --- | --- | --- |
| [usethesource/rascal](https://github.com/usethesource/rascal) | Language analysis, grammars and transformations | [jfxlcdp](https://github.com/sdk2035/jfxlcdp), [jfxlegacy2modern](https://github.com/sdk2035/jfxlegacy2modern), [jfxai4nlp](https://github.com/robotics-intelligent-systems/jfxai4nlp) | Extract facts and ASTs with code references; define a versioned IR. This alone does not constitute Truffle integration. | P0 |
| [abertschi/graalphp](https://github.com/abertschi/graalphp) | Experimental PHP implementation on GraalVM | [jfxlegacy2modern](https://github.com/sdk2035/jfxlegacy2modern) | Evaluate a PHP subset through differential testing; do not assume compatibility with an entire PHP ERP. | P2 |
| [sdk2035/spoon](https://github.com/sdk2035/spoon) · [upstream](https://github.com/INRIA/spoon) | Java source analysis and transformation | [jfxlcdp](https://github.com/sdk2035/jfxlcdp), [jfxlegacy2modern](https://github.com/sdk2035/jfxlegacy2modern) | Use as a specialized Java extractor; exchange facts with Rascal through a common schema. | P0 |
| [sdk2035/MPS](https://github.com/sdk2035/MPS) · [upstream](https://github.com/JetBrains/MPS) | Environment for building DSLs with dedicated editors | [jfxlcdp](https://github.com/sdk2035/jfxlcdp), [jfxengine](https://github.com/sdk2035/jfxengine) | Prototype a requirements or configuration DSL; export models without requiring end users to install the editor. | P2 |
| [sdk2035/Metalama](https://github.com/sdk2035/Metalama) · [upstream](https://github.com/metalama/Metalama) | C# metaprogramming and architecture patterns on Roslyn | [jfxlegacy2modern](https://github.com/sdk2035/jfxlegacy2modern) | Build-time tool in a separate .NET toolchain; export diagnostics and artifacts. | P2 |
| [sdk2035/csharplang](https://github.com/sdk2035/csharplang) · [upstream](https://github.com/dotnet/csharplang) | C# specifications and design | [jfxlegacy2modern](https://github.com/sdk2035/jfxlegacy2modern), [jfxai4nlp](https://github.com/robotics-intelligent-systems/jfxai4nlp) | Versioned reference corpus for rules and RAG; not a compiler or execution engine. | R |
| [sdk2035/procyon](https://github.com/sdk2035/procyon) · [upstream](https://github.com/mstrobel/procyon) | JVM metaprogramming and decompilation tools | [jfxlegacy2modern](https://github.com/sdk2035/jfxlegacy2modern) | Analyze authorized binaries when source is missing; retain uncertainties and check results. | P2 |
| [sdk2035/openflexo-core](https://github.com/sdk2035/openflexo-core) · [upstream](https://github.com/openflexo-team/openflexo-core) | Model federation infrastructure | [jfxengine](https://github.com/sdk2035/jfxengine), [jfxlcdp](https://github.com/sdk2035/jfxlcdp) | Model adapter with stable IDs; compare against the project's own IR before introducing another core. | P2 |
| [sdk2035/pamela](https://github.com/sdk2035/pamela) · [upstream](https://github.com/openflexo-team/pamela) | Modeling framework in the Openflexo ecosystem | [jfxengine](https://github.com/sdk2035/jfxengine), [jfxlcdp](https://github.com/sdk2035/jfxlcdp) | Evaluate alongside Openflexo for models and persistence; do not present it as a universal verifier. | P2 |
| [sdk2035/diana](https://github.com/sdk2035/diana) · [upstream](https://github.com/openflexo-team/diana) | Diagram-oriented graphics engine | [jfxlcdp](https://github.com/sdk2035/jfxlcdp), [jfxengine](https://github.com/sdk2035/jfxengine) | Editor and export prototype; check integration with the selected UI and avoid duplicating the existing editor. | P2 |
| [sdk2035/connie](https://github.com/sdk2035/connie) · [upstream](https://github.com/openflexo-team/connie) | Expressions and bindings over Java APIs | [jfxlcdp](https://github.com/sdk2035/jfxlcdp) | Evaluate property expressions with permitted functions and error handling; do not assume security isolation. | P2 |

### 5.2. CAD and Timber Manufacturing

| Component and provenance | Function | Proposed destination | Connection and adoption condition | Priority |
| --- | --- | --- | --- | --- |
| [sdk2035/STEP-LLM](https://github.com/sdk2035/STEP-LLM) · [upstream](https://github.com/JasonShiii/STEP-LLM) | Experimental STEP generation from natural language | [jfxai4dia](https://github.com/robotics-intelligent-systems/jfxai4dia), [jfxengine](https://github.com/sdk2035/jfxengine) | Inference worker returning STEP, parameters and evidence; validate geometry before presenting it as an accepted design. | P1 |
| [sdk2035/pyplasm](https://github.com/sdk2035/pyplasm) · [upstream](https://github.com/plasm-language/pyplasm) | Parametric geometric and solid design language | [jfxai4dia](https://github.com/robotics-intelligent-systems/jfxai4dia), [jfxengine](https://github.com/sdk2035/jfxengine) | Python worker for reproducible geometry and verified exports; test compatibility before using GraalPy. | P1 |
| [sdk2035/compas_timber](https://github.com/sdk2035/compas_timber) · [upstream](https://github.com/gramaziokohler/compas_timber) | Timber-frame structure design | [jfxai4bss](https://github.com/robotics-intelligent-systems/jfxai4bss), [jfxai4dia](https://github.com/robotics-intelligent-systems/jfxai4dia), [jfxosms](https://github.com/robotics-intelligent-systems/jfxosms) | Connect geometry, joints and manufacturing data through adapters; do not confuse design with structural certification. | P1 |
| [sdk2035/hyperwood-bench](https://github.com/sdk2035/hyperwood-bench) · [upstream](https://github.com/jo/hyperwood-bench) | Parametric wooden-bench design | [jfxai4dia](https://github.com/robotics-intelligent-systems/jfxai4dia), [jfxosms](https://github.com/robotics-intelligent-systems/jfxosms) | Design-to-manufacturing and bill-of-materials demonstrator; not a biomechanical benchmark. | R |

### 5.3. Perception and Inspection Robotics

| Component and provenance | Function | Proposed destination | Connection and adoption condition | Priority |
| --- | --- | --- | --- | --- |
| [sdk2035/chamelion](https://github.com/sdk2035/chamelion) · [upstream](https://github.com/url-kaist/chamelion) | Change detection between LiDAR scans and prior maps | [jfxlms4air](https://github.com/robotics-intelligent-systems/jfxlms4air) | Batch service for added/removed changes with confidence; evaluate errors in inspection maps. | P1 |
| [sdk2035/habitat-sim](https://github.com/sdk2035/habitat-sim) · [upstream](https://github.com/facebookresearch/habitat-sim) | 3D simulator for embodied-agent research | [jfxlms4air](https://github.com/robotics-intelligent-systems/jfxlms4air), [jfxengine](https://github.com/sdk2035/jfxengine) | Generate reproducible navigation and perception episodes; deliver observations and metrics to the console. | P1 |
| [sdk2035/habitat-lab](https://github.com/sdk2035/habitat-lab) · [upstream](https://github.com/facebookresearch/habitat-lab) | Tasks, training and evaluation for embodied agents | [jfxlms4air](https://github.com/robotics-intelligent-systems/jfxlms4air) | Experimentation layer over the simulator, with pinned scene, agent and task versions. | P1 |
| [sdk2035/physical-ai-studio](https://github.com/sdk2035/physical-ai-studio) · [upstream](https://github.com/open-edge-platform/physical-ai-studio) | Robot training through imitation of demonstrations | [jfxosms](https://github.com/robotics-intelligent-systems/jfxosms), [jfxrtess](https://github.com/robotics-intelligent-systems/jfxrtess) | Manipulation pilot in simulation and on a controlled test bench; retain versioned datasets and policies. | P2 |
| [nvidia-isaac/video_to_data](https://github.com/nvidia-isaac/video_to_data) | Video, reconstruction and robotics training-data pipeline | [jfxosms](https://github.com/robotics-intelligent-systems/jfxosms), [jfxengine](https://github.com/sdk2035/jfxengine) | Offline processing of demonstrations into scenes/datasets; validate embodiment and simulator compatibility rather than converting video directly into plant commands. | P2 |
| [sdk2035/Integrated-Vision-Inspection-System-IVIS](https://github.com/sdk2035/Integrated-Vision-Inspection-System-IVIS) · [upstream](https://github.com/msf4-0/Integrated-Vision-Inspection-System-IVIS) | Industrial visual inspection system | [jfxosms](https://github.com/robotics-intelligent-systems/jfxosms), [jfxscada](https://github.com/robotics-intelligent-systems/jfxscada) | Adapter sending inspection results to quality events and batch traceability. | P1 |
| [sdk2035/vision_transformer](https://github.com/sdk2035/vision_transformer) · [upstream](https://github.com/google-research/vision_transformer) | Vision Transformer and MLP-Mixer implementations | [jfxai4cv](https://github.com/robotics-intelligent-systems/jfxai4cv), [jfxosms](https://github.com/robotics-intelligent-systems/jfxosms) | Vision baseline to compare on the project's own data; do not present it as a validated medical model. | P2 |

### 5.4. Medical Imaging and Simulation

| Component and provenance | Function | Proposed destination | Connection and adoption condition | Priority |
| --- | --- | --- | --- | --- |
| [sdk2035/SAM-Med3D](https://github.com/sdk2035/SAM-Med3D) · [upstream](https://github.com/uni-medical/SAM-Med3D) | Volumetric medical-image segmentation | [jfxai4cv](https://github.com/robotics-intelligent-systems/jfxai4cv), [jfxai4rss](https://github.com/robotics-intelligent-systems/jfxai4rss) | Segmentation worker for research and simulation; return masks with geometry, version and expert review. | P2 |

### 5.5. Agriculture, Forests and Forest Economics

| Component and provenance | Function | Proposed destination | Connection and adoption condition | Priority |
| --- | --- | --- | --- | --- |
| [sdk2035/aquacrop](https://github.com/sdk2035/aquacrop) · [upstream](https://github.com/aquacropos/aquacrop) | AquaCrop-OSPy: soil–crop–water model | [jfxfmis](https://github.com/robotics-intelligent-systems/jfxfmis) | Irrigation scenario service using weather, soil and crop inputs; record units, parameters and calibration results. | P1 |
| [sdk2035/Drip-3Dponics](https://github.com/sdk2035/Drip-3Dponics) · [upstream](https://github.com/3dponics/Drip-3Dponics) | Printable hydroponics designs | [jfxfmis](https://github.com/robotics-intelligent-systems/jfxfmis), [jfxai4dia](https://github.com/robotics-intelligent-systems/jfxai4dia) | CAD and materials catalog for the system; instrumentation and control require additional components. | P2 |
| [sdk2035/SpeedSeed3](https://github.com/sdk2035/SpeedSeed3) · [upstream](https://github.com/GrowCab/SpeedSeed3) | Accelerated-growth system in a benchtop chamber | [jfxfmis](https://github.com/robotics-intelligent-systems/jfxfmis), [jfxscada](https://github.com/robotics-intelligent-systems/jfxscada) | Integrate environmental records and experimental recipes; review old dependencies before reusing the software. | P2 |
| [sdk2035/TiMBA](https://github.com/sdk2035/TiMBA) · [upstream](https://github.com/TI-Forest-Sector-Modelling/TiMBA) | Economic model of forest-product markets | [jfxfmis](https://github.com/robotics-intelligent-systems/jfxfmis), [jfxai4bpm](https://github.com/robotics-intelligent-systems/jfxai4bpm) | Optional module for supply, demand and trade scenarios; keep aggregate results separate from crop control. | P2 |
| [sdk2035/PixSim](https://github.com/sdk2035/PixSim) · [upstream](https://github.com/nicoscattaneo/PixSim) | Pixel-based forest-growth simulation in R | [jfxfmis](https://github.com/robotics-intelligent-systems/jfxfmis), [jfxai4rs](https://github.com/robotics-intelligent-systems/jfxai4rs) | R worker with growth maps and scenarios; do not classify it as a synthetic-foliage generator for vision. | P2 |
| [sdk2035/TreeSim](https://github.com/sdk2035/TreeSim) · [upstream](https://github.com/AbbasNabhani/TreeSim) | Individual-tree simulation and 3D visualization | [jfxfmis](https://github.com/robotics-intelligent-systems/jfxfmis), [jfxengine](https://github.com/sdk2035/jfxengine) | Forestry model with regional parameters; export forest state and geometry through a dedicated contract. | P2 |
| [sdk2035/isoblue](https://github.com/sdk2035/isoblue) · [upstream](https://github.com/oats-center/isoblue) | ISOBlue/Avena agricultural data hardware and software | [jfxfmis](https://github.com/robotics-intelligent-systems/jfxfmis), [jfxscada](https://github.com/robotics-intelligent-systems/jfxscada) | Machinery telemetry gateway; validate messages, timing and compatibility with real equipment. | P1 |

### 5.6. Shared AI Services

**llm-d integration status:** [sdk2035/llm-d](https://github.com/sdk2035/llm-d) is an upstream fork with local integration proposed for JFXAI4ARCH; a local milestone and progress estimate are not yet defined. The next proposed AI-assisted task is to draft a bounded local integration plan and benchmark criteria against upstream. This reference is maintained in the integration proposal rather than as a personal project entry in the profile.

| Component and provenance | Function | Proposed destination | Connection and adoption condition | Priority |
| --- | --- | --- | --- | --- |
| [sdk2035/OpenShell](https://github.com/sdk2035/OpenShell) · [upstream](https://github.com/NVIDIA/OpenShell) | Isolated execution environment for agents | [jfxai4arch](https://github.com/robotics-intelligent-systems/jfxai4arch), [jfxai4cbs](https://github.com/robotics-intelligent-systems/jfxai4cbs) | Evaluate as a job runtime with bounded policies and credentials; verify its guarantees in the selected deployment. | P1 |
| [sdk2035/mcp-memory-service](https://github.com/sdk2035/mcp-memory-service) · [upstream](https://github.com/doobidoo/mcp-memory-service) | Persistent agent memory with APIs and MCP | [jfxai4arch](https://github.com/robotics-intelligent-systems/jfxai4arch), [jfxai4nlp](https://github.com/robotics-intelligent-systems/jfxai4nlp) | Optional memory service separate from the evidence repository; partition by user, project and retention policy. | P1 |
| [sdk2035/ray](https://github.com/sdk2035/ray) · [upstream](https://github.com/ray-project/ray) | Distributed computing and AI libraries | [jfxai4arch](https://github.com/robotics-intelligent-systems/jfxai4arch), [jfxengine](https://github.com/sdk2035/jfxengine) | Job executor when measured workload justifies distribution; begin with simple workers. | P2 |
| [sdk2035/kserve](https://github.com/sdk2035/kserve) · [upstream](https://github.com/kserve/kserve) | Inference serving on Kubernetes | [jfxai4arch](https://github.com/robotics-intelligent-systems/jfxai4arch) | Deployment profile for teams already operating Kubernetes; not a prerequisite for the first pilot. | P2 |
| [sdk2035/otari](https://github.com/sdk2035/otari) · [upstream](https://github.com/mozilla-ai/otari) | Self-hosted, OpenAI-compatible LLM gateway for routing multiple providers, virtual keys, budgets and usage tracking | [jfxai4arch](https://github.com/robotics-intelligent-systems/jfxai4arch) | Consider only if the project needs multi-provider routing, scoped keys, budgets or usage accounting; compare with its existing model-provider approach and verify credential handling, data flows, tenant controls and operational requirements. | P2 |
| [zai-org/GLM-5](https://github.com/zai-org/GLM-5) | Open-weight GLM-5 series for complex systems engineering, coding and long-horizon agentic tasks; listed checkpoints reach 744B parameters | [jfxai4arch](https://github.com/robotics-intelligent-systems/jfxai4arch), [jfxai4nlp](https://github.com/robotics-intelligent-systems/jfxai4nlp) | Evaluate a specific checkpoint through a supported inference engine; compare task quality, latency and serving cost against current models, and verify accelerator capacity, engine compatibility and checkpoint-specific terms. The largest variants require substantial compute and are not a default local-runtime assumption. | P2 |
| [sdk2035/llm-d](https://github.com/sdk2035/llm-d) · [upstream](https://github.com/llm-d/llm-d) | Distributed LLM inference serving on Kubernetes, with cache-aware routing and prefill/decode disaggregation | [jfxai4arch](https://github.com/robotics-intelligent-systems/jfxai4arch) | Optional external inference service over model servers such as vLLM; evaluate only when workload and accelerator capacity justify distributed serving. Compare time to first token, inter-token latency, throughput and resource cost against a simple serving baseline; verify Kubernetes, network and model compatibility. | P2 |
| [sdk2035/langflow](https://github.com/sdk2035/langflow) · [upstream](https://github.com/langflow-ai/langflow) | AI flow and agent builder | [jfxai4arch](https://github.com/robotics-intelligent-systems/jfxai4arch), [jfxai4nlp](https://github.com/robotics-intelligent-systems/jfxai4nlp) | Optional authoring tool; export/version flows and keep execution contracts independent. | P1 |
| [sdk2035/AutoGPT](https://github.com/sdk2035/AutoGPT) · [upstream](https://github.com/Significant-Gravitas/AutoGPT) | Agent platform and tools | [jfxai4arch](https://github.com/robotics-intelligent-systems/jfxai4arch) | Experimental orchestration alternative; evaluate components and licenses before selecting it alongside another orchestrator. | P2 |
| [sdk2035/FastGPT](https://github.com/sdk2035/FastGPT) · [upstream](https://github.com/labring/FastGPT) | Self-hosted AI Agent and RAG platform with knowledge bases, visual flows, APIs and MCP | [jfxai4arch](https://github.com/robotics-intelligent-systems/jfxai4arch), [jfxai4nlp](https://github.com/robotics-intelligent-systems/jfxai4nlp) | Evaluate only as an alternative for one bounded workflow; compare its API, data isolation and evaluation with the existing JFXAI4ARCH baseline. Review its repository-specific LICENSE: the Apache-2.0 base has additional SaaS and branding conditions, and GitHub metadata reports NOASSERTION. | P2 |
| [sdk2035/harness2](https://github.com/sdk2035/harness2) · [upstream](https://github.com/google-research/harness2) | Framework for recursively optimizing agent harnesses through propose–probe–compose and persistent improvement lessons | [jfxai4arch](https://github.com/robotics-intelligent-systems/jfxai4arch), [jfxai4nlp](https://github.com/robotics-intelligent-systems/jfxai4nlp) | Bounded experiment on fixed benchmarks and model configurations; evaluate reported gains against JFX baselines and verify model access, execution isolation, tool permissions and evaluation cost before adoption. | P2 |
| [sdk2035/antigravity-sdk-python](https://github.com/sdk2035/antigravity-sdk-python) · [upstream](https://github.com/google-antigravity/antigravity-sdk-python) | Python SDK for stateful agents powered by Antigravity and Gemini; published wheels include a compiled runtime | [jfxai4arch](https://github.com/robotics-intelligent-systems/jfxai4arch) | Optional isolated Python worker for a bounded agent task. Verify platform-wheel support, provider authentication, data handling and service terms; keep JFX job/tool contracts provider-neutral. | P2 |
| [sdk2035/agents-cli](https://github.com/sdk2035/agents-cli) · [upstream](https://github.com/google/agents-cli) | CLI and skills for coding assistants to build, govern and deploy agents on Google Cloud's Gemini platform | [jfxai4arch](https://github.com/robotics-intelligent-systems/jfxai4arch) | Optional agent-development workflow; validate cloud IAM, generated resources, credentials, cost and provider constraints before use. | P2 |
| [sdk2035/relevanceai](https://github.com/sdk2035/relevanceai) · [upstream](https://github.com/RelevanceAI/relevanceai) | SDK for the Relevance AI platform | [jfxai4crm](https://github.com/robotics-intelligent-systems/jfxai4crm), [jfxai4arch](https://github.com/robotics-intelligent-systems/jfxai4arch) | Optional external connector; requires an account and service terms, and is not equivalent to a fully self-hosted platform. | P2 |
| [langfuse/langfuse](https://github.com/langfuse/langfuse) | Observability and evaluation for LLM applications | [jfxai4arch](https://github.com/robotics-intelligent-systems/jfxai4arch), [jfxai4nlp](https://github.com/robotics-intelligent-systems/jfxai4nlp) | The local metadata adapter exists in JFXAI4ARCH. Remaining: provision and validate a live local backend, then assess evaluation and trace requirements beyond status, evidence count and duration. | P0 |
| [sdk2035/semantic-caching-with-redis-langcache](https://github.com/sdk2035/semantic-caching-with-redis-langcache) · [upstream](https://github.com/redis-developer/semantic-caching-with-redis-langcache) | Semantic caching demonstration using Redis LangCache | [jfxai4arch](https://github.com/robotics-intelligent-systems/jfxai4arch) | Reference for an experiment with invalidation, tenant separation and stale-answer measurement. | R |
| [sdk2035/mlx](https://github.com/sdk2035/mlx) · [upstream](https://github.com/ml-explore/mlx) | Array and ML framework targeting Apple silicon | [jfxai4arch](https://github.com/robotics-intelligent-systems/jfxai4arch), [jfxai4nlp](https://github.com/robotics-intelligent-systems/jfxai4nlp) | Optional backend for compatible workstations; separate this profile from Linux/CUDA deployment and measure specific models. | P2 |
| [sdk2035/llama_index](https://github.com/sdk2035/llama_index) · [upstream](https://github.com/run-llama/llama_index) | Document processing and retrieval for AI | [jfxai4arch](https://github.com/robotics-intelligent-systems/jfxai4arch), [jfxai4nlp](https://github.com/robotics-intelligent-systems/jfxai4nlp) | Local Markdown/TXT retrieval, evidence provenance and retrieval tests already exist in JFXAI4ARCH. Remaining: approved corpus, held-out evaluation and comparison of dense retrieval against BM25. | P0 |
| [sdk2035/cohere-toolkit](https://github.com/sdk2035/cohere-toolkit) · [upstream](https://github.com/cohere-ai/cohere-toolkit) | Components and examples for RAG applications | [jfxai4arch](https://github.com/robotics-intelligent-systems/jfxai4arch), [jfxai4crm](https://github.com/robotics-intelligent-systems/jfxai4crm) | Reference or alternative accelerator; isolate provider dependencies through the model gateway. | P2 |
| [Sumanth077/Hands-On-AI-Engineering](https://github.com/Sumanth077/Hands-On-AI-Engineering) | Educational collection of OCR, RAG and agent projects | [jfxai4nlp](https://github.com/robotics-intelligent-systems/jfxai4nlp), [jfxlms](https://github.com/robotics-intelligent-systems/jfxlms) | Patterns and learning material; review the grounded_document_agent example before reusing code. | R |
| [sdk2035/llm-engineer-toolkit](https://github.com/sdk2035/llm-engineer-toolkit) · [upstream](https://github.com/KalyanKS-NLP/llm-engineer-toolkit) | Curated, category-wise index of 120+ LLM libraries | [jfxai4nlp](https://github.com/robotics-intelligent-systems/jfxai4nlp), [jfxai4arch](https://github.com/robotics-intelligent-systems/jfxai4arch) | Use as a discovery reference for LLM application development, RAG, agents and evaluation; review each linked project independently for fit, maintenance, license and compatibility. The catalog itself is not an installable dependency. | R |
| [sdk2035/seekdb](https://github.com/sdk2035/seekdb) · [upstream](https://github.com/oceanbase/seekdb) | Search engine with vector and structured data | [jfxai4arch](https://github.com/robotics-intelligent-systems/jfxai4arch), [jfxai4nlp](https://github.com/robotics-intelligent-systems/jfxai4nlp) | Compare as a retrieval store; do not replace the transactional database without testing requirements. | P2 |

### 5.7. Model Evaluation and Decisions

| Component and provenance | Function | Proposed destination | Connection and adoption condition | Priority |
| --- | --- | --- | --- | --- |
| [sdk2035/jevals](https://github.com/sdk2035/jevals) · [upstream](https://github.com/openlayer-ai/jevals) | Trace evaluation using decision models | [jfxai4nlp](https://github.com/robotics-intelligent-systems/jfxai4nlp), [jfxai4arch](https://github.com/robotics-intelligent-systems/jfxai4arch) | Compare groundedness, tool selection and scope against human-labeled examples. | P1 |
| [sdk2035/open-jev](https://github.com/sdk2035/open-jev) · [upstream](https://github.com/daseinlabs/open-jev) | Local option scoring with Gemma and MLX | [jfxai4nlp](https://github.com/robotics-intelligent-systems/jfxai4nlp) | Experimental evaluation backend; check platform, data distribution and calibration. | P2 |
| [sdk2035/AnyJev](https://github.com/sdk2035/AnyJev) · [upstream](https://github.com/nokia-applied-research/AnyJev) | Typed decisions based on model probabilities | [jfxai4nlp](https://github.com/robotics-intelligent-systems/jfxai4nlp), [jfxai4arch](https://github.com/robotics-intelligent-systems/jfxai4arch) | Compare with classifiers and rules; do not interpret probabilities as demonstrated reliability without local calibration. | P2 |

### 5.8. Specification-Driven Development and Development Tools

The added-value weight is a qualitative portfolio score from 1 (limited or narrow reuse) to 5 (strong, reusable contribution to specification-led delivery). It considers applicability across JFX projects, support for traceable requirements and implementation, and adoption cost or overlap. It is a prioritization aid, not a measured result. P0/P1/P2/R retain the meanings defined above; the score does not imply approval or implementation.

| Component and provenance | Function | Proposed destination | Connection and adoption condition | Added-value weight (1–5) | Priority |
| --- | --- | --- | --- | ---: | --- |
| [sdk2035/spec-kit](https://github.com/sdk2035/spec-kit) · [upstream](https://github.com/github/spec-kit) | Toolkit for scenario-oriented specification-driven development | JFX contribution workflow; pilot in [jfxlcdp](https://github.com/sdk2035/jfxlcdp) and one domain repository | Test whether its specification, plan and task artifacts can be committed and reviewed using existing JFX conventions; adopt only the useful workflow steps. | 5 | P0 |
| [sdk2035/OpenSpec](https://github.com/sdk2035/OpenSpec) · [upstream](https://github.com/Fission-AI/OpenSpec) | Specification-driven change workflow for AI coding assistants | JFX contribution workflow; pilot alongside spec-kit in one repository | Compare change proposal, implementation and archive artifacts with spec-kit; select one primary workflow after a small trial to avoid parallel sources of truth. | 5 | P0 |
| [sdk2035/Kiro](https://github.com/sdk2035/Kiro) · [upstream](https://github.com/kirodotdev/Kiro) | Agentic IDE and CLI with specs, hooks, steering and MCP | Optional engineering tool for a bounded JFX pilot | Evaluate portability of specifications and project rules, privacy and team workflow; do not make the IDE or its service a runtime dependency. | 3 | P2 |
| [sdk2035/py-gpt](https://github.com/sdk2035/py-gpt) · [upstream](https://github.com/szczyglis-dev/py-gpt) | Cross-platform desktop AI assistant with multi-provider models, agents, MCP, plugins and RAG | Optional local experimentation for JFX contributors, especially [jfxai4arch](https://github.com/robotics-intelligent-systems/jfxai4arch) | Evaluate only for a distinct contributor workflow; review agent/tool permissions, provider credentials and data handling, fork alignment and licensing. Keep it outside the JFX product runtime. | 2 | P2 |
| [sdk2035/antigravity-cli](https://github.com/sdk2035/antigravity-cli) · [upstream](https://github.com/google-antigravity/antigravity-cli) | Terminal coding-agent interface with multi-step reasoning, tool calling, multi-file edits and persistent history | Optional development tool for a bounded JFX code-assistance pilot | Evaluate product terms, workspace/data access, credentials and command-execution permissions; do not make it a JFX product runtime dependency. | 3 | P2 |
| [sdk2035/kirograph](https://github.com/sdk2035/kirograph) · [upstream](https://github.com/davide-desio-eleva/kirograph) | Local semantic code graph and symbol lookup, with primary support for Kiro | [jfxlegacy2modern](https://github.com/sdk2035/jfxlegacy2modern), [jfxai4nlp](https://github.com/robotics-intelligent-systems/jfxai4nlp) | Test code navigation and impact analysis on a representative repository; broader MCP integrations are described as experimental, so confirm compatibility before adoption. | 2 | R |
| [sdk2035/powers](https://github.com/sdk2035/powers) · [upstream](https://github.com/kirodotdev/powers) | Collection of on-demand specialized context and tools for Kiro agents | Optional engineering workflows for repositories with a matching task | Select and review individual powers, their instructions, tools and dependencies; the collection is Kiro-oriented and should not be treated as a uniform JFX dependency. | 3 | P2 |
| [sdk2035/cli-agent-orchestrator](https://github.com/sdk2035/cli-agent-orchestrator) · [upstream](https://github.com/awslabs/cli-agent-orchestrator) | Coordinates multiple AI coding CLIs in isolated terminal sessions | Optional engineering automation for [jfxai4arch](https://github.com/robotics-intelligent-systems/jfxai4arch) or repository maintenance | Prototype only for a workflow needing cross-CLI orchestration; inspect process isolation, credentials, audit trail and operational cost before use. | 3 | P2 |
| [sdk2035/kiro-team](https://github.com/sdk2035/kiro-team) · [upstream](https://github.com/requix/kiro-team) | Team-lead and specialized agents for planning, parallel work and validation in Kiro CLI | Optional development workflow for a bounded repository task | Compare with CLI Agent Orchestrator and existing review/CI practices; it is Kiro CLI-specific and overlaps with other orchestration options. | 2 | R |
| [sdk2035/kirodex](https://github.com/sdk2035/kirodex) · [upstream](https://github.com/thabti/kirodex) | Desktop application for coding agents | Optional developer tool for JFX contributors | Evaluate as a separate tool; do not integrate it as a product runtime dependency. | 2 | R |
| [sdk2035/spec-for-codex](https://github.com/sdk2035/spec-for-codex) · [upstream](https://github.com/atman-33/spec-for-codex) | Extension for specification-driven development | JFX contribution workflow | Compare its specification format and export with spec-kit and OpenSpec; retain only if it improves interoperability or fits the chosen developer environment. | 3 | P2 |
| [sdk2035/spec-for-codex-ide](https://github.com/sdk2035/spec-for-codex-ide) · [upstream](https://github.com/atman-33/spec-for-codex-ide) | IDE extension for specifications and assistants | Optional developer tool for JFX contributors | Compare with spec-for-codex and the selected specification workflow; avoid maintaining duplicate authoring experiences. | 2 | R |
| [sdk2035/tasktrooper](https://github.com/sdk2035/tasktrooper) · [upstream](https://github.com/makifbaysal/tasktrooper) | Task management and development-agent execution | Optional project-team workflow | Evaluate task and artifact tracking separately from product orchestration; use only if it fills a demonstrated collaboration gap. | 2 | R |
| [sdk2035/bruno](https://github.com/sdk2035/bruno) · [upstream](https://github.com/usebruno/bruno) | API client and testing tool | [jfxai4arch](https://github.com/robotics-intelligent-systems/jfxai4arch), [jfxai4bpm](https://github.com/robotics-intelligent-systems/jfxai4bpm), [jfxscada](https://github.com/robotics-intelligent-systems/jfxscada) | Health, Search and Answers checks already exist for JFXAI4ARCH. Extend collections for new domain contracts, error cases, retries and idempotency. | 5 | P0 |

Spec-kit and OpenSpec are the strongest candidates to compare first because their central contribution—structured, reviewable changes—can apply across multiple portfolio repositories. They are alternatives to evaluate, not dependencies to install together. Kiro, its powers and Kiro-specific orchestration tools remain optional developer tooling; none is proposed as part of the JFX production stack.

### 5.9. Scientific Computing and Interoperability

| Component and provenance | Function | Proposed destination | Connection and adoption condition | Priority |
| --- | --- | --- | --- | --- |
| [sdk2035/requests](https://github.com/sdk2035/requests) · [upstream](https://github.com/psf/requests) | Python HTTP/1.1 client for sessions, TLS verification, streaming and timeouts | Python workers for [jfxai4arch](https://github.com/robotics-intelligent-systems/jfxai4arch) and versioned service APIs | Optional client for bounded worker-to-service calls; set explicit timeouts and response limits, and track this fork against upstream. GitHub metadata reports Apache-2.0. | R |
| [sdk2035/julia4j](https://github.com/sdk2035/julia4j) · [upstream](https://github.com/rssdev10/julia4j) | Java–Julia binding through JNI and libjulia | [jfxengine](https://github.com/sdk2035/jfxengine), [jfxmodelica](https://github.com/sdk2035/jfxmodelica) | Bounded prototype for types, memory and lifecycle; the README describes basic functions and outstanding work. | P2 |
| [sdk2035/Juliana.jl](https://github.com/sdk2035/Juliana.jl) · [upstream](https://github.com/artecs-group/Juliana.jl) | Translation from CUDA.jl to KernelAbstractions.jl | [jfxengine](https://github.com/sdk2035/jfxengine), [jfxai4dia](https://github.com/robotics-intelligent-systems/jfxai4dia) | GPU portability experiment on selected Julia kernels; not a Java/GraalVM bridge. | P2 |
| [genieframework/Genie.jl](https://github.com/genieframework/Genie.jl) | Julia web framework | [jfxengine](https://github.com/sdk2035/jfxengine), [jfxmodelica](https://github.com/sdk2035/jfxmodelica) | Option for exposing scientific jobs as a service; maintain a stable API toward JavaFX. | P2 |

### 5.10. Firmware and Laboratory Hardware

| Component and provenance | Function | Proposed destination | Connection and adoption condition | Priority |
| --- | --- | --- | --- | --- |
| [sdk2035/platformio-core](https://github.com/sdk2035/platformio-core) · [upstream](https://github.com/platformio/platformio-core) | Embedded build and development tools | [jfxrtess](https://github.com/robotics-intelligent-systems/jfxrtess), [jfxscada](https://github.com/robotics-intelligent-systems/jfxscada) | Reproducible build toolchain per board; retain firmware, configuration and test results. | P1 |
| [sdk2035/Tasmota](https://github.com/sdk2035/Tasmota) · [upstream](https://github.com/arendst/Tasmota) | IoT firmware for compatible ESP devices | [jfxscada](https://github.com/robotics-intelligent-systems/jfxscada), [jfxfmis](https://github.com/robotics-intelligent-systems/jfxfmis) | Sensor and MQTT/HTTP telemetry pilot; do not use as a substitute for industrial safety logic. | P1 |
| [sdk2035/modular-backpack](https://github.com/sdk2035/modular-backpack) · [upstream](https://github.com/onsimon/modular-backpack) | Parts and cable distribution for Voron toolchangers | [jfxosms](https://github.com/robotics-intelligent-systems/jfxosms), [jfxai4dia](https://github.com/robotics-intelligent-systems/jfxai4dia) | Mechanical/electrical reference for laboratory printers; not a portable robotic backpack. | R |
| [sdk2035/Backpack](https://github.com/sdk2035/Backpack) · [upstream](https://github.com/ExpressLRS/Backpack) | ExpressLRS firmware for compatible FPV hardware | [jfxrtess](https://github.com/robotics-intelligent-systems/jfxrtess) | Evaluation bench for configuration and telemetry; not equivalent to modular-backpack. | P2 |
| [sdk2035/OpenLaptop](https://github.com/sdk2035/OpenLaptop) · [upstream](https://github.com/MiklosPathy/OpenLaptop) | Laptop hardware design | [jfxrtess](https://github.com/robotics-intelligent-systems/jfxrtess), [jfxengine](https://github.com/sdk2035/jfxengine) | Laboratory workstation reference; verify component availability and manufacturing cost. | R |
| [sdk2035/Openterface_KVM-GO_Hardware](https://github.com/sdk2035/Openterface_KVM-GO_Hardware) · [upstream](https://github.com/TechxArtisanStudio/Openterface_KVM-GO_Hardware) | KVM hardware design files | [jfxrtess](https://github.com/robotics-intelligent-systems/jfxrtess), [jfxscada](https://github.com/robotics-intelligent-systems/jfxscada) | Maintenance access to bench equipment; review capture, control and permissions without treating it as a robot controller. | R |

### 5.11. Enterprise Modernization and ABAP

| Component and provenance | Function | Proposed destination | Connection and adoption condition | Priority |
| --- | --- | --- | --- | --- |
| [sdk2035/abap-platform-basic-trial](https://github.com/sdk2035/abap-platform-basic-trial) · [upstream](https://github.com/SAP-samples/abap-platform-basic-trial) | Workbook and setup for a SAP ABAP Cloud environment | [jfxlegacy2modern](https://github.com/sdk2035/jfxlegacy2modern) | Laboratory reference and examples; requires a SAP environment and is not an open distribution of the SAP runtime. | R |
| [open-abap/open-abap-core](https://github.com/open-abap/open-abap-core) | Reusable ABAP artifacts for abaplint/transpiler | [jfxlegacy2modern](https://github.com/sdk2035/jfxlegacy2modern) | Test an ABAP subset; it does not represent a complete SAP kernel or a Truffle implementation. | P1 |
| [sdk2035/abap-mcp](https://github.com/sdk2035/abap-mcp) · [upstream](https://github.com/abap-ai/mcp) | MCP server SDK implemented in ABAP | [jfxai4bpm](https://github.com/robotics-intelligent-systems/jfxai4bpm), [jfxlegacy2modern](https://github.com/sdk2035/jfxlegacy2modern) | ABAP query and tool adapter; the README marks this generation as frozen and points to mcp2 for further development. | P1 |
| [sdk2035/ai-abap-assistant-sample](https://github.com/sdk2035/ai-abap-assistant-sample) · [upstream](https://github.com/google/ai-abap-assistant-sample) | Example of code assistance and review in the ABAP editor | [jfxlegacy2modern](https://github.com/sdk2035/jfxlegacy2modern), [jfxai4nlp](https://github.com/robotics-intelligent-systems/jfxai4nlp) | Reference for explanations, reviews and test proposals with human review. | R |
| [sdk2035/SAP_Project](https://github.com/sdk2035/SAP_Project) · [upstream](https://github.com/PrithviVenu/SAP_Project) | Demonstration application for ABAP analysis for S/4HANA | [jfxlegacy2modern](https://github.com/sdk2035/jfxlegacy2modern) | Study UX and the analysis workflow; require semantic tests before accepting suggested refactorings. | R |
| [sdk2035/com.etendoerp.copilot](https://github.com/sdk2035/com.etendoerp.copilot) · [upstream](https://github.com/etendosoftware/com.etendoerp.copilot) | Copilot and tools in the Etendo ecosystem | [jfxai4bpm](https://github.com/robotics-intelligent-systems/jfxai4bpm), [jfxai4crm](https://github.com/robotics-intelligent-systems/jfxai4crm), [jfxai4ohs](https://github.com/robotics-intelligent-systems/jfxai4ohs) | Connector to an Etendo instance or design reference; do not assume direct installation into Axelor, Dolibarr or OFBiz. | P2 |

### 5.12. Host Projects, Not Packages

| Component and provenance | Function | Proposed destination | Connection and adoption condition | Priority |
| --- | --- | --- | --- | --- |
| [robotics-intelligent-systems/jfxai4bpm](https://github.com/robotics-intelligent-systems/jfxai4bpm) | Portfolio process-orchestration project | [jfxai4bpm](https://github.com/robotics-intelligent-systems/jfxai4bpm) | Host for enterprise contracts and adapters; do not count it as an external dependency. | H |
| `robotics-intelligent-systems/jfxlcdp` → [sdk2035/jfxlcdp](https://github.com/sdk2035/jfxlcdp) | Reference in the attachment that should be corrected to sdk2035/jfxlcdp | [jfxlcdp](https://github.com/sdk2035/jfxlcdp) | Use the verified sdk2035/jfxlcdp path as the interface/low-code project; no same-named repository under robotics was confirmed. | H |

### 5.13. Self-Hosted Infrastructure and Game-Server Operations

| Component and provenance | Function | Proposed destination | Connection and adoption condition | Priority |
| --- | --- | --- | --- | --- |
| [sdk2035/panel](https://github.com/sdk2035/panel) · [upstream](https://github.com/pterodactyl/panel) | Web control plane for managing game servers and their nodes | Optional portfolio demo or development lab that needs isolated game-server workloads | Evaluate as a separately deployed operations platform; use its documented API for provisioning/status only where a concrete use case exists. Confirm the applicable license, supported versions, authentication, network exposure and container isolation before adoption. | P2 |
| [sdk2035/wings](https://github.com/sdk2035/wings) · [upstream](https://github.com/pterodactyl/wings) | Node daemon that manages game-server containers for Pterodactyl Panel | Same optional lab environment as Panel; deploy alongside the control plane on managed nodes | Treat as Panel's paired node agent, not a standalone JFX runtime or general-purpose worker. Validate version compatibility, daemon privileges, node firewalling and lifecycle operations before connecting it to a lab. | P2 |

Panel and Wings are proposed as optional infrastructure for a concrete game-server hosting or demonstration need. Their presence in this catalog does not establish integration with JFX services or justify adding them to the shared application stack.

### 5.14. Model-Based Systems Engineering and SysML v2

| Component and provenance | Function | Proposed destination | Connection and adoption condition | Priority |
| --- | --- | --- | --- | --- |
| [sdk2035/SysMD](https://github.com/sdk2035/SysMD) · [upstream](https://github.com/tukcps/SysMD) | SysML v2/KerML notebook with executable model cells and an integrated constraint solver | [jfxlcdp](https://github.com/sdk2035/jfxlcdp), [jfxengine](https://github.com/sdk2035/jfxengine) | Pilot with a bounded requirements/constraints model; validate solver semantics, supported language coverage and model interchange. Its README describes SysML v2/KerML support as incomplete, so do not treat it as the portfolio's standard authoring environment. | P2 |
| [sdk2035/sysml-repo-modeler](https://github.com/sdk2035/sysml-repo-modeler) · [upstream](https://github.com/ClearFracture/sysml-repo-modeler) | Self-hosted AI harness that generates SysML v2 models and interactive architecture graphs from repositories | [jfxlegacy2modern](https://github.com/sdk2035/jfxlegacy2modern), [jfxai4nlp](https://github.com/robotics-intelligent-systems/jfxai4nlp) | Test on a public or disposable repository; inspect model-to-source traceability and false connections, confirm provider/tool access to source code, and compare results with a reviewed architecture map before adoption. | P2 |
| [sdk2035/sysmod-sysmlv2](https://github.com/sdk2035/sysmod-sysmlv2) · [upstream](https://github.com/MBSE4U/sysmod-sysmlv2) | SysML v2 library for the SYSMOD MBSE method, requirements-to-architecture artifacts and optional AI prompt metadata | [jfxlcdp](https://github.com/sdk2035/jfxlcdp), [jfxengine](https://github.com/sdk2035/jfxengine) | Pilot one requirements-to-architecture workflow; verify import compatibility with the selected SysML v2 tool and review AI prompts, traceability and human approval before reuse. | P2 |
| [sdk2035/DE4SDV](https://github.com/sdk2035/DE4SDV) · [upstream](https://github.com/de4sdv/DE4SDV) | SysML v2 reference model for software-defined vehicle product lines, digital continuity and continuous compliance | [jfxrtess](https://github.com/robotics-intelligent-systems/jfxrtess) | Use as a domain case study for architecture variants and lifecycle evidence; keep vehicle-specific safety and compliance scope within that project. It is a model/reference project, not a reusable JFX runtime component. | R |
| [sdk2035/SysML-v2-Pilot-Implementation](https://github.com/sdk2035/SysML-v2-Pilot-Implementation) · [upstream](https://github.com/Systems-Modeling/SysML-v2-Pilot-Implementation) | Eclipse-based pilot for SysML v2/KerML textual notation, editing and visualization | [jfxlcdp](https://github.com/sdk2035/jfxlcdp), [jfxengine](https://github.com/sdk2035/jfxengine) | Treat as a prototype and standards reference. Its documented toolchain uses Eclipse 2025-12, Java 21, Xtext/QVT and Maven; verify build and compatibility before reuse. | R |
| [sdk2035/sysml-examples](https://github.com/sdk2035/sysml-examples) · [upstream](https://github.com/elan8/sysml-examples) | Compact SysML v2 textual models for learning, teaching and experimentation | [jfxlcdp](https://github.com/sdk2035/jfxlcdp), [jfxengine](https://github.com/sdk2035/jfxengine) | Use pinned models as tutorial, parser or importer fixtures; validate each example against the selected SysML v2 implementation before relying on it. | R |
| [NIST DELS model library](https://catalog.data.gov/dataset/sysml-model-libraries-supporting-discrete-event-logistics-systems-dels-models) | SysML 1.4 libraries for discrete-event logistics systems, distributed as XMI/HTML and MagicDraw materials; dataset last updated in 2020 | [jfxosms](https://github.com/robotics-intelligent-systems/jfxosms), [jfxengine](https://github.com/sdk2035/jfxengine) | Use as a legacy-format logistics reference or migration fixture, not as a SysML v2 library. Validate conversion and provenance, and review the [NIST open-license terms](https://www.nist.gov/open/license) before redistribution. | R |

The three P2 entries are optional experiments for a concrete SysML/MBSE need. The four R entries are domain references, a pilot implementation, examples or a legacy dataset; none implies adoption of a common SysML toolchain. The NIST DELS models use SysML 1.4 and require a separate migration decision before use with SysML v2 tooling.

## 6. Restructuring the Cross-Cutting Stack

The attachment's final list mixes languages, runtimes, libraries, managed services and practices. Each should be recorded with its type and responsibility.

| Layer | Initial proposal | Alternatives and limits |
| --- | --- | --- |
| UI | JavaFX for desktop; React/TypeScript for web when there is a use case | React is a UI library, not a language. `reactj4j` remains an unidentified reference: the attachment provides no repository or version |
| Core and contracts | JVM for Java components; APIs for external workers | GraalVM is an execution/optimization option that must be measured; it does not automatically turn the entire stack into a single application |
| Scientific Python and ML | CPython workers for the first prototype; evaluate GraalPy per package and platform | GraalPy implements Python 3; Jython maintains the Python 2.7 line and is not an equivalent substitute for this ecosystem. See [GraalPy](https://www.graalvm.org/reference-manual/graalpy/) and [Jython](https://www.jython.org/news.html) |
| JavaScript and TypeScript | TypeScript compiled to JavaScript; a Node service when Node APIs are required | Embedded GraalJS does not automatically provide `fs`, `http` or other Node APIs. See the [interoperability documentation](https://www.graalvm.org/reference-manual/js/NashornMigrationGuide/) |
| Go | Native service with an HTTP/gRPC contract if it adds value | `golang-jvm` remains unidentified and is not recommended: a concrete reference and compatibility tests are missing |
| Julia and R | Separate scientific workers or services | Evaluate JNI/libjulia only if measurements justify its complexity; Juliana.jl addresses GPU portability, not JVM integration |
| LLM orchestration | Choose a primary runtime; LlamaIndex for the proposed ingestion, LangChain or LangChain4j when their ecosystem fits the service | Langflow may provide authoring; AutoGPT and Relevance AI are alternatives with their own limits. Avoid deploying all of them without distinct responsibilities |
| Inference serving | Start with one versioned model endpoint behind the application/provider adapter; pin model and server configuration | Compare runtime engines separately from deployment/orchestration layers; no runtime is adopted here. |
| Model providers | Interchangeable gateway for local models, OpenAI API or AWS Bedrock | Record provider, version, transmitted data and per-task cost; select through evaluation without assuming functional equivalence |
| Persistence | PostgreSQL for transactional records; object storage for artifacts | DynamoDB is a managed alternative for a specific access pattern, not another mandatory dependency |
| Retrieval | Select a text/vector index using a test query set | SeekDB is a comparison candidate; avoid introducing multiple databases merely because they appear in the list |
| Cache | Redis only where a measured benefit exists | Semantic caching requires invalidation, user/project scope and stale-answer tests; it does not replace the evidence source |
| Packaging | Docker containers per worker; reproducible configuration | GPU, devices and native runtimes need explicit profiles |
| Optional AWS | ECS for services/containers, S3 for artifacts, RDS for the PostgreSQL option and CloudWatch for operations | Lambda and API Gateway for tasks/adapters suited to their limits; do not assume every simulation or GPU workload fits Lambda |
| Infrastructure as code | Choose Terraform or CloudFormation according to the operating environment | Do not maintain two divergent descriptions of the same infrastructure |
| Scaling | Simple workers initially; Ray for distributed computing, KServe for inference serving on Kubernetes, and llm-d for distributed LLM serving when measured demand justifies it | These are different responsibilities; they can coexist when a measured need justifies it |
| APIs | REST for jobs/artifacts; events or WebSocket for progress and telemetry | GraphQL is optional for aggregate queries; gRPC may serve internal contracts. MCP exposes tools, not a replacement for every protocol |
| Testing | Unit, API contract, integration and domain-specific scientific tests | Bruno helps test APIs; jevals/AnyJev/open-jev do not replace geometry, semantics or hardware tests |
| Continuous delivery | CI/CD with pinned versions, artifacts and test results | Record code, models, datasets, configurations and firmware separately |

Compatibility with native extensions and platforms must be evaluated for the selected versions; the [GraalPy documentation](https://www.graalvm.org/latest/python/docs/) distinguishes support by platform and package. The proposal does not presume speed improvements from switching runtimes.

## 7. Remaining Pilot Work and Acceptance Criteria

The quantities below are planning starting points, not quality estimates or performance commitments. Complete validation of the existing shared pilot, then select a domain.

Completed baseline work is excluded from this backlog: local Markdown/TXT loading, LlamaIndex/BM25 retrieval, source/line/hash evidence, authenticated APIs, Ollama fallback handling, a Langfuse metadata adapter and Bruno requests. The [pilot documentation](https://github.com/robotics-intelligent-systems/jfxai4arch/blob/main/docs/rag/PILOT.md#verification) reports 22 automated tests and three passing Bruno requests/tests. These reported checks were not rerun for this documentation update; they do not establish real-model quality or a working Langfuse deployment.

| Pilot | Project and selection | Deliverable | Criterion for deciding whether to continue |
| --- | --- | --- | --- |
| A. Validate and extend the technical assistant | JFXAI4ARCH; JFXAI4NLP remains a proposed evaluation partner | Approved technical corpus, real local model validation and verified live Langfuse metadata; compare dense retrieval against the existing BM25 baseline | Curate at least 50 independent questions with relevant passages and unanswerable cases; separate development and held-out sets. Measure retrieval, claim support, citation validity, unsupported answers, p50/p95 latency and resource cost; set thresholds before evaluation |
| B. Verifiable text-to-CAD | JFXAI4DIA + JFXENGINE; STEP-LLM and a geometry validator | Candidate STEP generator with a report and viewer | Test approximately 20 simple parts; measure validity, units, dimensions, geometry and failures. A renderable image is not sufficient for manufacturing acceptance |
| C. Change inspection | JFXLMS4AIR; Chamelion, project-owned scenes/datasets and Habitat when it contributes a useful task | Detection of added and removed elements between captures | Measure precision, recall, registration error and time; compare against a baseline over annotated changes |
| D. Agricultural scenarios | JFXFMIS; AquaCrop-OSPy, ISOBlue or existing records | Water/crop scenario and telemetry comparison tool | Validate inputs/units, reproducibility and local calibration; present decisions as scenarios without automating irrigation from the LLM |
| E. ABAP modernization | JFXLEGACY2MODERN + JFXAI4BPM; open-abap-core and a SAP/MCP adapter | Analysis and transformation of a small subset with tests | Define support, compare behavior and errors, and record unsupported constructs; begin with queries before enterprise writes |

**Suggested execution order:**

1. Inventory versions, data availability and ownership; select one provider/model and one storage path per pilot.
2. Reuse the existing JFXAI4ARCH API and BM25 baseline for pilot A; define missing contracts and baselines for the selected domain pilot.
3. Run tests, record failures and review the adoption decision.
4. Introduce distribution, persistent memory or semantic caching only after measuring the bottleneck.

```mermaid
flowchart TB
    catalog["Candidate and host project"] --> scope["Bounded use case and contract"]
    scope --> baseline["Baseline and test data"]
    baseline --> prototype["Reproducible adapter or worker"]
    prototype --> measure["Measure quality, latency, cost and failures"]
    measure --> decision{"Meets agreed criteria?"}
    decision -->|Yes| adopt["Versioned adoption with an owner"]
    decision -->|No| revise["Revise or withdraw candidate"]
    revise --> scope
    adopt --> monitor["Monitor changes and regressions"]
```

## 8. Decisions That Should Remain Separate

- **Rascal, Spoon and MPS:** Rascal for rules and languages, Spoon as a specialized Java extractor, MPS when DSL authoring is needed. They are not three interchangeable engines for the same task.
- **Langflow, AutoGPT and Relevance AI:** evaluate authoring/orchestration/connector alternatives; do not declare all of them mandatory dependencies.
- **Ray, KServe and llm-d:** distinguish distributed computing, Kubernetes inference serving and distributed LLM-serving optimizations. Evaluate compatible combinations when needed; none is a prerequisite for the local RAG pilot.
- **Habitat and Video to Data:** navigation/embodied tasks versus reconstruction and learning from demonstrations. Choose according to task, environment and robot.
- **AquaCrop, PixSim, TreeSim and TiMBA:** crop/water, spatial forest growth, individual trees and forest economics, respectively. Combining them requires compatible models and scales.
- **COMPAS Timber and hyperwood-bench:** design tool versus product example; both fit CAD/manufacturing better than agronomic simulation.
- **Agent memory and evidence:** memory may summarize experiences; technical statements must be traceable to identifiable documents, data and results.
- **Etendo and other ERPs in the catalog:** reusing an API or pattern does not mean the module installs unchanged in other ERPs.

## 9. Minimum Record for Component Adoption

For each selected candidate, retain:

| Field | Purpose |
| --- | --- |
| Requested repository, upstream and commit/tag | Know exactly which code is used and whether the fork is behind |
| Artifact type | Distinguish library, service, tool, dataset, model, documentation and hardware |
| Code, model, data and design licenses | Resolve the conditions applicable to each artifact; do not infer them solely from public visibility |
| Host project and owner | Assign maintenance and acceptance responsibility |
| Integration contract | Version inputs, outputs, units, errors and limits |
| Tested environment | Runtime, OS, CPU/GPU, native dependencies and resources |
| Test evidence | Baseline, dataset, metrics, known failures and decision |
| Operating and exit costs | Measure resources and define how to replace or retire the component |

GitHub metadata shows diverse licenses and some cases without conclusive identification. This document does not determine legal compatibility or presume that all repositories can be reused under the same conditions.

## 10. Verification Scope

The inventory covered **47 JFX repositories** visible under `robotics-intelligent-systems` and eight JFX references under `sdk2035`. The attachment's 69 references were checked against GitHub; the JFXLCDP path under robotics was not confirmed and was corrected to the verified sdk2035 project.

To classify destinations, the review read the READMEs of JFXAI4ARCH, JFXAI4NLP, JFXAI4DIA, JFXLMS4AIR, JFXAI4CV, JFXAI4BSS, JFXFMIS, JFXOSMS, JFXSCADA, JFXAI4RSS, JFXAI4BIO, JFXAI4BPM, JFXAI4CRM, JFXAI4OHS and JFXRTESS. It also reviewed READMEs for components whose names or original descriptions could be misleading, such as TiMBA, PixSim, TreeSim, Juliana.jl, hyperwood-bench, the two Backpack projects, ABAP MCP, Relevance AI and Julia4J. The October 14 update checked the sdk2035 Panel and Wings fork metadata and descriptions against their declared Pterodactyl upstreams; the catalog entry is an optional lab proposal, not an implemented integration.

Each row links to the component source and, where a fork exists, its declared upstream. The recommendations for destinations, priorities, contracts and pilots are the proposed work in this document. No packages were installed, no models or simulations were run, and no GitHub repositories were modified during the original analysis.

The October 6 update reviewed the llm-d fork README and declared upstream, the current sdk2035 profile, and JFXAI4ARCH's pilot implementation report. The catalog now includes llm-d; completed baseline tasks were removed from pending work while real-model, live-telemetry and domain validation remain open. The October 14 update adds the Pterodactyl Panel and Wings forks under self-hosted infrastructure.

The October 16 review of the first supplied list checked README content and metadata for four references, including declared fork origins. CLI Agent Orchestrator was already cataloged in §5.8, so three new rows were added. A second review checked sdk2035/llm-engineer-toolkit and sdk2035/py-gpt; GitHub metadata lists the toolkit as Apache-2.0 and confirms PyGPT's upstream parent, while PyGPT's license metadata is NOASSERTION. A third review checked sdk2035/otari and its declared mozilla-ai/otari upstream; GitHub metadata lists its license as Apache-2.0. A fourth review checked sdk2035/harness2 and its declared google-research/harness2 upstream; the README describes recursive agent-harness optimization and reports benchmark results, and GitHub metadata lists Apache-2.0. Projects indexed by the toolkit retain their individual terms. These updates are proposals from source review, not builds, license audits or integration tests. The October 17 review checked the official zai-org/GLM-5 README and repository metadata. It confirms downloadable GLM-5-series checkpoints, inference-engine references and Apache-2.0 repository metadata; this source check does not establish checkpoint-specific usage terms or local hardware suitability. GLM-5 remains an optional P2 evaluation, not an adopted model. A separate October 17 attachment review checked six sdk2035 repositories, their available README materials and license metadata, and declared upstreams, along with the supplied NIST/Data.gov record. The SysML v2 Pilot Implementation also documents its Eclipse, Java, Xtext/QVT and Maven environment. The NIST DELS dataset record says it was last updated in 2020 and describes SysML 1.4 models in MagicDraw, XMI and HTML, so it is cataloged as a legacy-format reference. This review did not build code or models.

The October 17 follow-up reviewed GitHub README files, repository metadata and license files for sdk2035/requests and sdk2035/FastGPT. GitHub identifies sdk2035/requests as a fork of psf/requests with Apache-2.0 metadata. sdk2035/FastGPT is a fork of labring/FastGPT; its LICENSE adds conditions to an Apache-2.0 base and GitHub reports NOASSERTION, so inspect those terms before reuse. This source review did not build or test either repository.

## References

- [Model Serving and Open-Model Stack](https://github.com/sdk2035/sdk2035/blob/main/docs/model-serving-and-open-model-stack.md)
