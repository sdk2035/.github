# Model Serving and an Open-Model Application Stack

**Review date:** 2026-10-17  
**Status:** Architecture reference. This document records options to evaluate; it does not claim an adopted stack, a production deployment, or a benchmark result.

This note consolidates two Spanish TXT files added to the repository on 2026-10-17. Their useful ideas are retained as a layered architecture, with product roles and operational claims qualified where the source text treated them as universal.

## Terminology and scope

“Open source” and “open weights” are not interchangeable labels. The [Open Source AI Definition 1.0](https://opensource.org/ai/open-source-ai-definition) describes access to the code and parameters, along with the information needed to study and modify the system. A downloadable checkpoint alone does not establish that a model meets that definition. Check the terms and artifacts for the exact model release, including its weights, tokenizer, inference code, training information, and any use restrictions.

A self-hosted model can reduce dependence on a hosted inference API, but it does not guarantee lower total cost, portability, privacy, or freedom from vendor constraints. Network access, telemetry, model terms, storage, accelerator supply, operations, and application architecture still matter.

## Layered architecture

| Layer | Responsibility | Examples and decision boundary |
| --- | --- | --- |
| Model and artifacts | Select a model, tokenizer, embedding model, or classifier and record its exact revision and terms. | Llama, Mistral, Qwen, Gemma, Phi, LLaVA, CLIP, Sentence Transformers, Nomic Embed, and Jina are examples from the source notes, not a license or compatibility review. |
| Inference runtime | Load model artifacts and expose a request interface. | vLLM, NVIDIA Triton, Hugging Face Text Generation Inference (TGI), and llama.cpp/Ollama serve different model and hardware profiles. Compare supported model features, platforms, memory use, interfaces, and operations. TGI documentation says it entered maintenance mode on 2025-12-11; consider it for existing deployments or compatibility cases, not as an assumed default for a new service. |
| Retrieval and data | Ingest approved sources, prepare chunks and embeddings, retrieve relevant passages, and preserve source references. | Qdrant, Milvus, Weaviate, Chroma, FAISS, and PostgreSQL with pgvector are possible retrieval components. LangChain, LlamaIndex, and Haystack provide application or ingestion patterns. These do not replace the model server or the system of record. |
| Application and orchestration | Apply authentication, authorization, business rules, request validation, retries, and model selection. | FastAPI, Node.js, LangChain, LlamaIndex, Haystack, CrewAI, and AutoGen are examples, not a requirement to deploy several orchestration frameworks together. Keep a stable API between the application and an inference worker. |
| Tool execution | Expose explicitly permitted operations to an agent or application. | MCP servers can provide tools and resources. Docker MCP Toolkit manages containerized MCP servers from Docker Desktop; it is a separate function from serving model weights. |
| User experience | Present chat, workflow, or domain-specific interfaces. | Open WebUI, Next.js, Streamlit, and Gradio are examples. A user interface does not replace server-side authorization or input validation. |
| Operations and security | Observe quality, latency, resource use, failures, and access; manage releases and recovery. | OpenTelemetry, Langfuse, Sentry, API gateways, Docker, Kubernetes, and CI/CD may support parts of this layer. Their presence alone does not establish safety, compliance, or production readiness. |

Weight formats and quantization methods are runtime-specific. Examples such as GGUF, AWQ, and GPTQ must be checked against the chosen engine and checkpoint. Quantization can change quality, memory use, and performance; measure those effects on the target task rather than assuming there is no material quality loss.

## Separate the model server from tools and retrieval

The inference server accepts a model request and returns a model response. A retrieval layer locates evidence. The application owns user and domain policy. An MCP server executes only the tools made available to it. These responsibilities can be deployed separately or combined when there is a concrete operational reason, but one does not automatically provide the controls of another.

The Docker documentation currently labels MCP Toolkit **Beta** and describes the Docker Desktop 4.62-and-later interface. It manages profiles of containerized MCP servers and connects clients to them; it is not an inference engine. Container isolation can reduce exposure, but is not a blanket security guarantee. Review image provenance, privileges, filesystem mounts, network access, credentials, server behavior, and the actual Docker Desktop version and configuration.

## Deployment and scaling

- Pin the model revision, serving image, configuration, and accelerator/runtime versions. Check each engine’s supported model and hardware matrix before choosing a CPU, GPU, or other accelerator profile.
- Keep large model artifacts outside the runtime image unless there is a specific reason to package them. Use controlled artifact storage or mounted volumes, record checksums and revisions, and protect credentials used to fetch gated or private models.
- Grant only the accelerator devices and host resources the service needs. Engine documentation may recommend shared-memory settings for particular workloads; the vLLM Docker example uses --ipc=host, but that is an engine-specific configuration choice, not a universal requirement for model serving. Review the current [vLLM Docker guidance](https://docs.vllm.ai/en/stable/deployment/docker/) before adapting its command.
- Put a versioned API in front of the serving worker. Add a message queue only when the job’s latency, ordering, retry, and failure semantics permit asynchronous handling. A queue does not itself make a transactional or real-time decision safe.
- Start with the simplest deployment that meets the measured need. Kubernetes, KServe, distributed serving, load balancers, and custom autoscaling add operational responsibilities. Scale on measured queue, latency, and accelerator signals, and account for model loading, warm-up, readiness, and capacity limits.
- Treat examples using “millions of transactions,” “extreme” throughput, or fixed cost savings as workload hypotheses. They require a defined workload and reproducible measurements before they can describe a real system.

## Evaluation and acceptance

Compare candidate engines with the same task, model revision, prompt or input distribution, hardware, concurrency, and quality criteria. Record at least:

1. Supported model features, hardware, dependencies, terms, and operational ownership.
2. Quality against a task-specific reference set, including failures and unsupported outputs.
3. Time to first token, inter-token latency, request latency at p50 and p95, throughput, queue time, and error rate.
4. GPU/CPU and memory use, startup and model-load time, storage, and recovery behavior.
5. Security controls, data flows, secret handling, update and rollback procedures, and tool permissions.
6. Total operating cost, including compute, storage, network, support, and maintenance, compared with an appropriate hosted alternative.

Do not infer that a model is safe for fraud decisions or another mission-critical workload because it can be containerized or scaled. Domain validation, human oversight, fallback behavior, and acceptance thresholds belong to the application owner.

## Relationship to the sdk2035/JFX proposal

The [JFX portfolio integration proposal](propuesta-integracion-portafolio-jfx.md#56-shared-ai-services) assigns shared RAG, inference, and tool work to JFXAI4ARCH and lists KServe and llm-d as conditional deployment options. Those references remain proposals and are not evidence that a serving cluster or model server has been deployed. This note adds a runtime and architecture comparison; it does not add a new adopted dependency or change the proposal’s candidate counts.

## Source provenance

The consolidated material comes from these files in commit [f11c2a9](https://github.com/sdk2035/sdk2035/commit/f11c2a9259ddf8baf53cb5461bcbe36d828606bb):

- [A continuación se integra.txt](https://github.com/sdk2035/sdk2035/blob/f11c2a9259ddf8baf53cb5461bcbe36d828606bb/docs/A%20continuaci%C3%B3n%20se%20integra.txt) — blob SHA 100983062d3fa85178211445bf4e198e52f60546.
- [Una arquitectura de Open.txt](https://github.com/sdk2035/sdk2035/blob/f11c2a9259ddf8baf53cb5461bcbe36d828606bb/docs/Una%20arquitectura%20de%20Open.txt) — blob SHA 0e4d6e33e886c90f36343db0a1b22b9c398289e4.

The source files are removed from the working documentation tree after this replacement is indexed; their original commit and blobs remain the provenance record. No container, model, benchmark, or deployment example was run for this documentation change.

## Primary references

- [Open Source AI Definition 1.0 — Open Source Initiative](https://opensource.org/ai/open-source-ai-definition)
- [Using Docker — vLLM documentation](https://docs.vllm.ai/en/stable/deployment/docker/)
- [Text Generation Inference documentation](https://huggingface.co/docs/text-generation-inference/index)
- [Triton architecture — NVIDIA documentation](https://docs.nvidia.com/deeplearning/triton-inference-server/user-guide/docs/user_guide/architecture.html)
- [Docker MCP Toolkit — Docker documentation](https://docs.docker.com/ai/mcp-catalog-and-toolkit/toolkit/)
- [Run LLMs Locally with Docker: A Quickstart Guide to Model Runner](https://www.docker.com/blog/run-llms-locally/)
- [Getting Started with Ollama: Running Language Models in Docker and Python](https://medium.com/@aziza.bibin/getting-started-with-ollama-running-language-models-in-docker-and-python-5dda8335d85f) — Community tutorial; it pulls `gemma3:4b` but its Python sample requests `gemma:2b`, so recheck the model tag, API, and Streamlit run command before adapting it.
