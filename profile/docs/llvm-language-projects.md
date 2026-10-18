# LLVM language project supplement

Added 2026-10-12: two personal repositories, one per language. This supplements the historical 2026-10-11 public-project register; its issue and project counts have not been re-audited.

### Rust and Go on LLVM / Sulong

One repository per language: **TruffleRust** and **TruffleGo** are conceptual
aliases inside GraalRust and GraalGo. These are independent portfolio initiatives,
not official GraalVM language distributions or delivered AST interpreters.

| Project | Target language | Proposed GraalVM mechanism | Technical status | Next task |
| --- | --- | --- | --- | --- |
| [GraalRust](https://github.com/sdk2035/GraalRust) · TruffleRust alias | Rust 🦀 | rustc → LLVM bitcode → Sulong (Truffle) | Experimental / academic workstream; architecture documented; execution unvalidated | GRUST-001: pin compatible versions and validate one bounded native/bitcode fixture. |
| [GraalGo](https://github.com/sdk2035/GraalGo) · TruffleGo alias | Go 🐹 | Candidate LLVM frontend (TinyGo) → LLVM bitcode → Sulong (Truffle) | Experimental / research; runtime compatibility unvalidated | GGO-001: assess TinyGo artifacts, runtime dependencies and one differential fixture. |

Rust requires explicit checks for LLVM versions, ABI, linking and panic/runtime
dependencies. Go uses TinyGo as a research candidate; the standard Go compiler
is not the proposed bitcode producer. Bitcode generation alone establishes
neither full language support nor successful Sulong execution.



## Validation gate

Both projects currently provide architecture and requirements documentation. A first pilot requires pinned toolchains, native reference results, inspected bitcode, successful Sulong execution and committed logs. No implementation percentage or performance claim is assigned.
