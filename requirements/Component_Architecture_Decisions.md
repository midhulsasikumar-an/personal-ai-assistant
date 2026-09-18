Component_Architecture_Decisions.md
MATRIX 1 — COMPONENT DECISIONS
Subsystem	Source	Decision	Reason	Risk	Priority
Agent Runtime	OpenJarvis	USE OPENJARVIS AS-IS	Mature, flexible agent lifecycle, executor handles cross‑cutting concerns	Low	High
Model Abstraction	OpenJarvis	USE OPENJARVIS AS-IS	12+ backends, hardware‑aware auto‑discovery, clean ABC	Low	High
Tools	OpenJarvis	MODIFY / EXTEND OPENJARVIS	Add per‑tool observability (latency/success) from Open Interpreter to feed learning system	Low	Medium
Memory	OpenJarvis	MODIFY / EXTEND OPENJARVIS	Layer Letta‑inspired memory editing tools, versioned archival store, and entity‑centric views atop existing FactStore/MemoryService	Low	High
Entity Memory	OpenJarvis	BORROW ARCHITECTURAL IDEA	Letta’s entity‑memory blocks give coherent entity dossiers; implement as derived view or memory‑editor tools	Low	High
History Tracking	OpenJarvis	USE OPENJARVIS AS-IS	Trace system already records full interactions; AgentContext provides session‑level history	Low	Medium
RAG	OpenJarvis	USE OPENJARVIS AS-IS	Seven backends, ingestion pipeline, context injection with source attribution	Low	High
Web Access	OpenJarvis	USE OPENJARVIS AS-IS	Web search, HTTP request, browser tools, 15+ channel integrations cover needs	Low	Medium
Background Processing	OpenJarvis	USE OPENJARVIS AS-IS	MemoryService fact extraction, AgentManager ticks, scheduler daemon, failure handling	Low	Medium
Scheduling	OpenJarvis	MODIFY / EXTEND OPENJARVIS	Enable scheduler by default, improve UI, add event‑driven option; already cron‑based with persistence	Low	Medium
Storage	OpenJarvis	USE OPENJARVIS AS-IS	Storage abstraction, multiple purpose‑built stores (facts, telemetry, traces, scheduler, audit), migration support	Low	High
Security	OpenJarvis	USE OPENJARVIS AS-IS	Composable GuardrailsEngine, capability policy, taint tracking, injection scanner, Docker sandbox, audit logging	Low	High
Observability	OpenJarvis	MODIFY / EXTEND OPENJARVIS	Extend ToolExecutor to publish per‑tool metrics (latency, success rate) to telemetry/traces (idea from Open Interpreter)	Low	Medium
APIs / Interfaces	OpenJarvis	USE OPENJARVIS AS-IS	CLI, SDK, HTTP API, WebSocket, 15+ Channels, Desktop GUI, clear interface independence	Low	High
Extension / Plugin Architecture	OpenJarvis	USE OPENJARVIS AS-IS	18 typed registries, skills system, MCP/A2A, maximum extensibility	Low	High
Configuration	OpenJarvis	MODIFY / EXTEND OPENJARVIS	Simplify TOML structure, add better validation and documentation; 2400‑line config is a barrier	Low	Medium
Testing	OpenJarvis	USE OPENJARVIS AS-IS	668 test files, CI pipeline, coverage ≥60%, Windows/Linux, clear organization	Low	High
Maintainability	OpenJarvis	USE OPENJARVIS AS-IS	Strong module boundaries, single responsibility, explicit interfaces, manageable technical debt	Low	High
MATRIX 2 — WHAT WE TAKE FROM EACH PROJECT
Project	Capability/Idea	Adopt?	How?
Letta	Memory editing functions (agent‑controlled updates via tools)	Yes	Add a set of memory‑editor tools (e.g., memory_update_entity, memory_decay_fact, memory_promote_event) that the agent can invoke through its normal tool‑calling loop.
Letta	Versioned/archival memory with temporal tracking	Yes	Layer an append‑only archival store (or time‑series log) atop the FactStore; expose via a tool (memory_archive_query) or as a view for temporal queries.
Letta	Entity‑centric memory blocks (coherent entity dossiers)	Yes	Implement a derived RetrievalTool or memory‑editor tool that aggregates facts by entity ID/name into a single textual block; optionally maintain via automatic summarization tool.
Letta	Autonomous memory evolution / summarization	Yes (investigate)	Add a background tool (e.g., memory_consolidate) invocable via the scheduler that runs summarization/deduplication passes.
Open Interpreter	OS‑native sandboxing (Seatbelt/Landlock/Windows restricted tokens)	Reference only	Use as a reference for future sandbox hardening; keep current Docker‑based GuardrailsEngine as default but document OS‑native alternatives.
Open Interpreter	Per‑tool observability (latency, success rate)	Yes	Extend OpenJarvis’ ToolExecutor to publish per‑tool invocation counts, latency, and success/failure to the telemetry/trace system; feed into learning for tool selection.
OpenHands	Workspace‑aware agent state (intrinsic working directory)	Yes	Add a workspace field to AgentContext; default file‑system tools (read/write, glob) to operate relative to this workspace unless overridden.
crewAI	Declarative workflow engine (sequential/conditional agent pipelines)	Yes	Add a lightweight YAML‑definable workflow layer that can invoke agents/tools via the AgentRegistry and ToolExecutor, with state passing between steps.
crewAI	Automated state sharing / context passing in workflows	Yes	Implement workflow steps that implicitly read/write a shared context store (e.g., a temporary memory scope) to reduce boilerplate.
crewAI	Role‑based agent organization (teams, leaders)	No	Low immediate value for awareness system; defer to later if hierarchical agent teams become necessary.
OpenHands	Pre‑bundled skill system (build‑time import)	No	Sacrifices runtime flexibility needed for dynamic awareness domains; prefer OpenJarvis’ runtime‑resolvable skills from Hermes/OpenClaw.
Open Interpreter	Default‑on security philosophy	No	Our composable opt‑in model provides flexibility; we can instead improve secure defaults via configuration (e.g., enable GuardrailsEngine by default).
MATRIX 3 — WHAT WE DO NOT TAKE
Project	Capability	Reason
Letta	Full MemGPT‑style memory subsystem replacement	OpenJarvis’ memory infrastructure (FactStore, MemoryService, automatic fact extraction, provenance tracking) is already mature; wholesale replacement would discard working components for marginal gain.
Open Interpreter	Replace OpenJarvis tool execution with its agent loop	OpenJarvis’ ToolExecutor and agent‑tool calling (function‑calling + structured modes) are mature and sufficient; the added sandboxing benefit does not justify rewriting the tool layer.
OpenHands	Tight agent‑tool integration for coding loops	Specialized for software engineering (file edit, terminal, etc.); irrelevant to an awareness/monitoring use case.
OpenHands	Pre‑bundled skill system (@openhands/extensions)	Build‑time binding removes runtime flexibility needed to load new skills as awareness domains evolve.
crewAI	Flows as a wholesale replacement for agent orchestration	OpenJarvis already provides orchestration via OperativeAgent, MonitorOperativeAgent, and the A2A protocol; augmenting with a workflow layer is preferable to replacement.
crewAI	Role‑based agent organization (teams, scopes)	Low priority; awareness system can initially operate with peer agents; hierarchical roles can be revisited later if needed.
OpenHands	Desktop‑first UI assumptions (Electron/React)	Our architecture already supports multiple interfaces (CLI, HTTP, SDK, Channels, Desktop GUI); adopting OpenHands’ UI would lock us into a specific frontend stack.
Letta	Automatic memory versioning without tool exposure	OpenJarvis prefers explicit tool‑based control so the learning system can discover useful memory‑editing patterns; fully automatic versioning hides the mechanism from learning.
MATRIX 4 — WHAT WE BUILD OURSELVES
Component	Why custom	Dependency	Priority
Memory‑editor toolset (update, decay, promote, consolidate)	Provides agent‑controlled memory editing inspired by Letta; not present in OpenJarvis	Depends on existing FactStore/MemoryService; no new external dependencies	High
Temporal/archival memory view	Enables time‑series queries for monitoring developments and impact	Built atop FactStore; requires append‑only log or versioned table	High
Entity‑dossier tool (aggregate facts by entity)	Supplies coherent entity context for relevance judgments	Uses existing metadata/indexing in FactStore	High
Per‑tool observability extensions to ToolExecutor	Feeds granular tool performance into learning system for smarter tool selection	Extends ToolExecutor; publishes to existing telemetry/trace	Medium
Workspace‑aware AgentContext field	Reduces friction for file‑system monitoring tasks	Simple field addition; file tools check for workspace default	Medium
Declarative workflow engine (YAML‑defined agent/tool sequences)	Simplifies multi‑step awareness pipelines (monitor→filter→extract→alert)	Calls existing AgentRegistry and ToolExecutor; persists workflow definitions via storage	Medium
Automatic memory‑consolidation background tool	Optional self‑optimizing memory to manage size and salience	Scheduler‐invokable tool; uses existing memory‑editor primitives	Low (investigate)
Secure‑by‑default configuration profile	Provides out‑of‑the‑box safe defaults without sacrificing opt‑in flexibility	Modifies default config.toml values (e.g., enable GuardrailsEngine, sandbox)	Low