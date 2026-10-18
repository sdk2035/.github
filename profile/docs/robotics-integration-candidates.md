# Robotics Integration Candidates

**Review date:** 2026-10-16  
**Source:** user-provided file `https github com sdk2035.txt` (17 GitHub URLs).  
**Status:** categorized portfolio references for future evaluation. Listing a repository here does not mean its code was audited, its license was checked, or a package was selected or installed. Some links may point to mirrors or research/documentation projects rather than installable packages.

## Candidate catalog

### Robot platform, hardware and deployment

| Repository | Categorized role | Potential portfolio connection |
| --- | --- | --- |
| [RP1](https://github.com/sdk2035/RP1) | Full-stack open humanoid platform, spanning mechanical/electrical design, firmware, robot models, control and learning. | Reference platform for a bounded robotics integration; select a concrete subsystem and interface before connecting it to a host project. |
| [RP1_hardware](https://github.com/sdk2035/RP1_hardware) | Mechanical CAD, PCB, wiring harness and bill-of-material design artifacts. | JFXOSMS for manufacturing/quality traceability, if a specific artifact exchange is defined. |
| [RP1_firmware](https://github.com/sdk2035/RP1_firmware) | Linux board support, USB-CANFD bridge firmware and Debian package sources. | JFXRTESS for reproducible firmware/build and hardware-integration evidence. |
| [rp1_description](https://github.com/sdk2035/rp1_description) | Candidate robot-description repository referenced by the source list. The repository README was unavailable at review time, so its contents and status remain unverified. | Confirm repository identity and supported description formats before assigning a host or consuming models. |
| [roboparty_deploy](https://github.com/sdk2035/roboparty_deploy) | ROS 2 deployment framework with motor/IMU interfaces, policy inference and depth perception for RPO/RP1. | JFXRTESS for deployment and integration-test scenarios, subject to an agreed ROS 2 boundary. |
| [roboparty_joystick](https://github.com/sdk2035/roboparty_joystick) | Android teleoperation interface with UDP/BLE control, gamepad input, authentication and emergency-stop flow. | Candidate operator interface; define authorization, safety and command contracts before connecting to another system. |

### Actuation and end effectors

| Repository | Categorized role | Potential portfolio connection |
| --- | --- | --- |
| [ROMOMO](https://github.com/sdk2035/ROMOMO) | Joint-module specifications, hardware manuals, CAN-FD protocol and test materials. The README describes documentation and test materials, not a software package. | JFXRTESS for test-evidence references; verify revision and protocol compatibility against the target hardware. |
| [rp_dexhand](https://github.com/sdk2035/rp_dexhand) | RP Hand product and integration documentation for a dexterous hand; README points to a separately maintained driver. | JFXRTESS for device/ROS integration tests after confirming the driver and hardware revision. |

### Learning, motion and policy research

| Repository | Categorized role | Potential portfolio connection |
| --- | --- | --- |
| [roboparty_train](https://github.com/sdk2035/roboparty_train) | Isaac Lab/RSL-RL training workspace for locomotion policies, with evaluation, export, motion-retargeting and MuJoCo Sim2Sim workflows. | Candidate for a reproducible training-to-deployment evidence flow; keep simulation and hardware validation distinct. |
| [MimicLite](https://github.com/sdk2035/MimicLite) | Humanoid motion tracking and policy training/deployment project landing page, linking implementation, framework, motion toolkit and deployment repositories. | JFXLMS4AIR as a possible perception/motion research consumer only if a concrete dataset or evaluation interface is established. |
| [UFO](https://github.com/sdk2035/UFO) | Unsupervised reinforcement-learning framework for humanoid control; the README explicitly says the RP1 TeCH policy has not been tested on the physical robot. | Research/evaluation candidate; do not treat simulation results as physical RP1 validation. |
| [P-BFM](https://github.com/sdk2035/P-BFM) | Perceptive behavioral foundation model for terrain-adaptive humanoid control. | Potential policy/perception evaluation reference; first define model, observation and action interfaces. |
| [I-BFM-Code](https://github.com/sdk2035/I-BFM-Code) | Reward-conditioned humanoid interaction research code with training source, checkpoint and task examples. | Potential offline benchmark or research adapter after environment, checkpoint and license review. |
| [INTACT-JEPA](https://github.com/sdk2035/INTACT-JEPA) | Isomorphic intent-to-action world-model research; its README marks code and models as forthcoming. | Research reference only until releasable code/models and an integration contract are available. |

### Testing and reliability

| Repository | Categorized role | Potential portfolio connection |
| --- | --- | --- |
| [RP1_test](https://github.com/sdk2035/RP1_test) | RP1 module-aging and whole-robot reliability project with test rigs, PLC/CAN HMI, dashboard, data archive and analysis. | JFXRTESS for test orchestration/evidence or JFXOSMS for quality records, with data ownership and schemas agreed first. |

### Perception and visual-inertial research

| Repository | Categorized role | Potential portfolio connection |
| --- | --- | --- |
| [vistream](https://github.com/sdk2035/vistream) | Visual-inertial research code and website, including method, configurations and evaluation tools; datasets, checkpoints and runtime dependencies are separate. | JFXLMS4AIR as a candidate evaluation/reference source; reproducibility requires obtaining and pinning external assets. |

### Manufacturing knowledge and supply chain

| Repository | Categorized role | Potential portfolio connection |
| --- | --- | --- |
| [robot_supplychain](https://github.com/sdk2035/robot_supplychain) | Procurement and component-selection knowledge, acceptance criteria, gates and supplier-quality playbooks for humanoid robots. | JFXOSMS for manufacturing/supply-chain knowledge workflows, subject to data governance and a defined synchronization contract. |

## Integration guidance

The strongest initial portfolio links are **RP1_test ↔ JFXRTESS** for test evidence, **robot_supplychain ↔ JFXOSMS** for manufacturing and quality workflows, and **vistream / selected policy projects ↔ JFXLMS4AIR** for research evaluation. These are proposed destinations based on published descriptions, not implemented connections.

Before selecting any item, record the exact upstream/repository revision, license, platform and runtime requirements, data/model provenance, hardware assumptions, and a measurable acceptance case. Keep motion-policy training, simulation, hardware deployment and safety validation as separate stages. Do not treat a public repository link as proof that a component is an installable package or ready for operational use.

The source attachment contains URLs only; it provides no version pins, license decisions, compatibility evidence or deployment authorization.
