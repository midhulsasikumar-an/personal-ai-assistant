Open Source Component and Architecture Strategy
1. Executive Summary
OpenJarvis is selected as the PRIMARY FOUNDATION because it provides a mature, modular, and extensible Python framework with strong local‑first support, a registry‑based plugin system, comprehensive tool and memory infrastructure, and clear separation of concerns. Rather than merging complete frameworks, we adopt a selective reuse approach: we keep OpenJarvis core intact, borrow specific architectural ideas from other projects where they demonstrate a clear advantage, integrate only narrowly justified external components, and build a small set of custom foundation components to address gaps identified in Phases 2‑3. This strategy preserves upgradeability, minimizes coupling, and focuses effort on value‑added improvements.
2. Foundation Strategy
OpenJarvis supplies the runtime, abstraction layers, tool/memory subsystems, security, observability, APIs, extension mechanism, configuration, testing, and maintainability. Our awareness‑relevance system will sit atop this foundation, using its agent runtime, tool calling, memory, and scheduling primitives while enhancing memory with Letta‑inspired editing and temporal views, adding per‑tool observability, workspace‑aware context, and a lightweight workflow layer for multi‑step pipelines.
3. OpenJarvis Components Used As-Is
Agent Runtime (BaseAgent, ToolUsingAgent, AgentExecutor, AgentManager)  
Model / LLM Abstraction (InferenceEngine ABC, 12+ backends, hardware auto‑discovery)  
Tool Architecture (BaseTool ABC, ToolExecutor, 50+ built‑in tools, MCP adapter)  
RAG / Retrieval (seven storage backends, ingestion pipeline, context injection)  
Web / Information Access (web_search, http_request, browser tools, 15+ channel integrations)  
Background Processing (MemoryService fact extraction, AgentManager tick‑based execution, scheduler daemon)  
Scheduling (cron‑based scheduler with SQLite persistence and MCP tools – to be enabled by default)  
Storage / Database (storage abstraction, purpose‑built stores for facts, telemetry, traces, scheduler, audit)  
Security / Execution Isolation (GuardrailsEngine wrapper, capability policy, taint tracking, injection scanner, Docker sandbox, audit logging, SSRF protection, signing)  
Observability (Telemetry store, Trace store, EventBus, structured logging)  
APIs / Interfaces (CLI, SDK, HTTP API + WebSocket, 15+ Channels, Desktop GUI)  
Extension / Plugin Architecture (18 typed registries, skills system, MCP client/server, A2A protocol)  
Configuration (central TOML config with environment overrides and secret separation)  
Testing (668 test files, CI pipeline with Ruff, pytest‑xdist, coverage ≥60%, Windows/Linux)  
Maintainability (clear module boundaries, single responsibility, explicit interfaces)
4. OpenJarvis Components We Will Modify
Subsystem	Why Modify	Planned Changes
Tools	Add per‑tool observability to feed learning system	Extend ToolExecutor to publish per‑tool latency, success/failure counts to telemetry/trace
Memory	Enable agent‑controlled editing, temporal tracking, and entity‑centric views	Layer memory‑editor tools, append‑only archival store, and entity‑dossier derivation atop existing FactStore/MemoryService
Scheduling	Make scheduler default‑on and improve usability	Set scheduler.enabled = true in default config, add CLI conveniences, explore event‑driven option
Observability	Feed granular tool metrics into learning system for smarter tool selection	Extend ToolExecutor to emit per‑tool stats; ensure TraceCollector picks them up
Configuration	Reduce complexity and improve validation	Refine TOML structure, add schema validation, improve documentation, provide secure‑by‑default profile
5. Architectural Ideas Borrowed From Other Projects
Project	Idea	Why Useful	Influence on Design
Letta	Memory editing functions (agent‑controlled updates via tools)	Lets the agent actively maintain its long‑term store (merge entities, decay facts, promote events)	Implement as a set of tools (memory_update_entity, memory_decay_fact, memory_promote_event, memory_consolidate) invocable through the normal tool‑calling loop
Letta	Versioned/archival memory with temporal tracking	Supports monitoring how facts evolve over time (“what did we know yesterday?”)	Add an append‑only archival log or versioned table; expose via memory_archive_query tool or as a temporal view
Letta	Entity‑centric memory blocks (coherent entity dossiers)	Provides a unified view of all facts about an entity for relevance judgments	Create a derived RetrievalTool or memory‑editor tool that aggregates facts by entity ID/name into a single textual block; optionally maintain via automatic summarization
Open Interpreter	Per‑tool observability (latency, success rate)	Enables the learning system to prefer faster, more reliable tools	Extend ToolExecutor to publish per‑tool invocation metrics; feed into TraceAnalyzer/TelemetryAggregator for tool‑selection policies
OpenHands	Workspace‑aware agent state (intrinsic working directory)	Reduces friction for file‑system monitoring tasks	Add a workspace field to AgentContext; default file tools (file_read, file_write, glob) to operate relative to this workspace unless overridden
crewAI	Declarative workflow engine (sequential/conditional agent/tool pipelines)	Simplifies multi‑step awareness pipelines (monitor → filter → extract → alert)	Add a lightweight YAML‑definable workflow layer that invokes agents/tools via the AgentRegistry and ToolExecutor, with state passing between steps
crewAI	Automated state sharing / context passing in workflows	Cuts boilerplate for pipelines where intermediate results feed downstream steps	Implement workflow steps that implicitly read/write a shared context store (e.g., a temporary memory scope)
6. External Components We Will Actually Integrate
No complete external components are integrated. All enhancements are built as extensions or modifications to OpenJarvis core. The only external consideration is the optional use of OS‑native sandboxing primitives (Seatbelt/Landlock/Windows restricted tokens) as a reference for future hardening, but we will retain the Docker‑based GuardrailsEngine as the default.
7. Components We Will Build Ourselves
Infrastructure‑level components (foundation extensions):
Memory‑editor toolset (update, decay, promote, consolidate) – built atop FactStore/MemoryService  
Temporal/archival memory view – append‑only log or versioned table layered on FactStore  
Entity‑dossier tool – aggregation of facts by entity using existing metadata/indexing  
Per‑tool observability extensions to ToolExecutor – emit per‑tool metrics to telemetry/trace  
Workspace‑aware AgentContext field – simple field addition with defaulting logic for file tools  
Declarative workflow engine (YAML‑defined agent/tool sequences) – calls existing registries, stores workflow definitions via storage  
Automatic memory‑consolidation background tool – scheduler‑invokable tool using memory‑editor primitives (investigate)  
Secure‑by‑default configuration profile – adjusted default config.toml values (e.g., enable GuardrailsEngine, sandbox)
These components are clearly separate from the future custom world‑awareness intelligence (monitoring, relevance, impact assessment, notification) which will be implemented as custom agents, tools, and workflows using the foundation.
8. Components We Will NOT Combine
We deliberately avoid merging whole frameworks because:
Complete framework integration would introduce high coupling, duplicated responsibilities, and upgrade complexity.  
OpenJarvis already satisfies the core infrastructure needs; wholesale replacement would discard working components for uncertain gain.  
The awareness‑relevance system’s unique value lies in its custom intelligence layers, not in re‑implementing generic agent runtime, tool calling, or memory subsystems.  
Selective reuse lets us adopt proven improvements while preserving a clear upgrade path to future OpenJarvis releases.
9. Final Component Matrix
Subsystem	Source	Decision	Priority	Risk
Agent Runtime	OpenJarvis	USE AS‑IS	High	Low
Model Abstraction	OpenJarvis	USE AS‑IS	High	Low
Tools	OpenJarvis	MODIFY / EXTEND	Medium	Low
Memory	OpenJarvis	MODIFY / EXTEND	High	Low
Entity Memory	OpenJarvis	BORROW IDEA	High	Low
History Tracking	OpenJarvis	USE AS‑IS	Medium	Low
RAG	OpenJarvis	USE AS‑IS	High	Low
Web Access	OpenJarvis	USE AS‑IS	Medium	Low
Background Processing	OpenJarvis	USE AS‑IS	Medium	Low
Scheduling	OpenJarvis	MODIFY / EXTEND	Medium	Low
Storage	OpenJarvis	USE AS‑IS	High	Low
Security	OpenJarvis	USE AS‑IS	High	Low
Observability	OpenJarvis	MODIFY / EXTEND	Medium	Low
APIs / Interfaces	OpenJarvis	USE AS‑IS	High	Low
Extension / Plugin Architecture	OpenJarvis	USE AS‑IS	High	Low
Configuration	OpenJarvis	MODIFY / EXTEND	Medium	Low
Testing	OpenJarvis	USE AS‑IS	High	Low
Maintainability	OpenJarvis	USE AS‑IS	High	Low
10. Implementation Priority
Immediate (0‑1 months):  
Enable scheduler by default  
Add workspace field to AgentContext and default file tools  
Refine configuration defaults to a secure‑by‑established profile
Core Development (1‑3 months):  
Build memory‑editor toolset (update, decay, promote)  
Implement temporal/archival memory view  
Create entity‑dossier tool  
Extend ToolExecutor for per‑tool observability  
Implement declarative workflow engine (YAML‑defined agent/tool sequences)
Later (3‑6 months):  
Investigate and potentially add automatic memory‑consolidation background tool  
Enhance configuration schema validation and documentation  
Evaluate OS‑native sandboxing primitives as a hardening alternative (reference only)
Validation Only:  
Long‑term scalability of memory archival and entity‑dossier under high volume  
Cross‑platform behavior of new memory‑editor tools  
Effectiveness of per‑tool observability on learning‑driven tool selection
11. Final High-Level Architecture
CUSTOM INTELLIGENCE
        ↓
CUSTOM EXTENSION LAYER  (memory‑editor tools, temporal view, entity‑dossier, workflow engine,
                         per‑tool observability, workspace‑aware context, scheduler tweaks)
        ↓
OPENJARVIS FOUNDATION
        ↓
SELECTIVE COMPONENTS / ARCHITECTURAL IMPROVEMENTS
        (e.g., per‑tool metrics, archival store, entity‑dossier view, workflow definitions,
         workspace field, secure‑by‑default config)
All layers are clearly separated; the custom intelligence layer consists of agents, tools, and workflows that address monitoring, relevance, impact, and notification—not part of this foundation strategy.
12. Dependency and Maintenance Strategy
To preserve future OpenJarvis upgradability:
All modifications are implemented as additive extensions (new tools, new memory views, new config defaults) rather than altering existing OpenJarvis source files where possible.  
Where existing files must be touched (e.g., ToolExecutor, AgentContext), changes are minimal, isolated, and clearly marked with comments indicating project‑specific extensions.  
We will maintain a patch‑baseline against the upstream OpenJarvis repository, regularly rebasing to capture upstream improvements while reapplying our extensions.  
The extension mechanism (registries) is used wherever feasible (new tools via @ToolRegistry.register, new memory backends via @MemoryRegistry.register), ensuring zero‑touch upgrades for core components.  
Documentation will explicitly note which files are project‑specific extensions to aid future maintainers.
13. Remaining Unknowns
Long‑term performance and storage impact of the archival/versioned memory store under high write volume.  
Effectiveness of entity‑dossier aggregation when facts are numerous and noisy.  
Whether per‑tool observability significantly improves learning‑based tool selection in practice.  
Cross‑platform reliability (Windows/macOS/Linux) of the new memory‑editor tools and workflow engine.  
Optimal configuration defaults for the secure‑by‑default profile across varied deployment scales.
14. Final Engineering Recommendation
Adopt OpenJarvis as the immutable foundation, extend it with a focused set of memory‑centric and observability enhancements inspired by Letta and Open Interpreter, add workspace awareness and a lightweight workflow layer from OpenHands and crewAI, and keep all other subsystems as‑is. This yields a strong, upgradeable platform tailored to our awareness and relevance requirements while minimizing integration risk and maintenance overhead.
COMPONENT ARCHITECTURE STRATEGY COMPLETE