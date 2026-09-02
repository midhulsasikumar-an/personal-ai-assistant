PHASE 1 — DOCUMENT UNDERSTANDING AND SCOPE MAPPING
1. DOCUMENT INVENTORY
Document	Purpose	Candidate Covered	Authority
Open_Source_AI_Foundation_Requirements.md	Authoritative evaluation specification defining technical, architectural, engineering, and project-health capabilities expected from an open-source AI agent/framework	N/A — the standard itself	Authoritative — the single evaluation standard all audits must normalize to
OpenJarvis Audit.md	Comprehensive repository audit against the requirements document	OpenJarvis (Stanford/Hazy Research)	Evidence-only — audit report with findings, scores, evidence references
openhands.md	Forensic engineering audit of OpenHands repository	OpenHands (All Hands AI)	Evidence-only — audit report with matrix, scores, evidence
OPEN INTERPRETER.md	Technical foundation audit report	Open Interpreter / codex-rs (OpenAI Codex fork)	Evidence-only — audit report with requirement-by-requirement evaluation
Letta-ai .md	Technical assessment against requirements	Letta-ai / Letta Code	Evidence-only — audit report with capability matrix, strengths/weaknesses
crewAI.md	Technical assessment summary and requirement audit	crewAI (crewAI Inc.)	Evidence-only — audit report with classification overview and detailed audit
2. AUTHORITATIVE REQUIREMENT FRAMEWORK
Document: Open_Source_AI_Foundation_Requirements.md (1,319 lines)
Total Requirements: ~100+ individual requirements across 30+ categories
Requirement Categories (30 sections):
 1. Architecture (ARCH-001 to ARCH-007) — 7 requirements
 2. Agent Runtime (AGENT-001 to AGENT-008) — 8 requirements
 3. LLM/Model Abstraction (LLM-001 to LLM-006) — 6 requirements
 4. Tool Framework (TOOL-001 to TOOL-006) — 6 requirements
 5. Memory Infrastructure (MEM-001 to MEM-005) — 5 requirements
 6. Retrieval/RAG (RAG-001 to RAG-005) — 5 requirements
 7. Voice Infrastructure (VOICE-001 to VOICE-005) — 5 requirements
 8. Browser/Web (WEB-001 to WEB-004) — 4 requirements
 9. File System (FILE-001 to FILE-004) — 4 requirements
10. Code Execution (CODE-001 to CODE-003) — 3 requirements
11. Background Processing (BG-001 to BG-004) — 4 requirements
12. Scheduling (SCHED-001 to SCHED-003) — 3 requirements
13. Plugin/Extension (EXT-001 to EXT-004) — 4 requirements
14. API/Interface (API-001 to API-005) — 5 requirements
15. Storage (STORE-001 to STORE-004) — 4 requirements
16. Configuration (CONFIG-001 to CONFIG-004) — 4 requirements
17. Security/Permissions (SEC-001 to SEC-005) — 5 requirements
18. Logging/Observability (OBS-001 to OBS-004) — 4 requirements
19. Testing (TEST-001 to TEST-004) — 4 requirements
20. Documentation (DOC-001 to DOC-006) — 6 requirements
21. Deployment (DEP-001 to DEP-004) — 4 requirements
22. Performance (PERF-001 to PERF-004) — 4 requirements
23. Maintainability (MAINT-001 to MAINT-005) — 5 requirements
24. Repository Health (HEALTH-001 to HEALTH-005) — 5 requirements
25. Licensing (LIC-001 to LIC-004) — 4 requirements
Priority Distribution:
- Critical: ~40 requirements (ARCH all, AGENT-001/003, LLM-001/002/003/006, TOOL-001/002, MEM-001, BG-001, EXT-001/002, API-004, SEC-001/002/003/005, TEST-001, DOC-001/002, DEP-003, MAINT-001/003/004, HEALTH-001/005, LIC-001/002)
- High: ~35 requirements
- Medium: ~20 requirements
- Low: ~5 requirements
Classification System (6 levels):
Classification	Definition
Native	Implemented directly and usable
Integratable	Not native, but supported through clean/practical integration
Modifiable	Partial capability exists, meaningful engineering changes required
Missing	No meaningful implementation or supported mechanism
Incompatible	Exists but architecturally unsuitable
Unknown	Evidence insufficient or contradictory
Scoring Methodology (Section 35-36):
- 0 = Missing
- 1 = Experimental/poor
- 2 = Basic implementation
- 3 = Usable implementation
- 4 = Mature implementation
- 5 = Mature, extensible, well-documented
Evidence Standard (Section 5): Source code > Official docs > Tests > Working examples > Official integrations > Release history
Evaluation Principle (Section 39): Select based on Architecture + maturity + extensibility + maintainability + openness + engineering value
3. CANDIDATE INVENTORY
Candidate	Repository/Project Type	Main Architectural Orientation	Audit Completeness	Internal Consistency
OpenJarvis	Python framework (Stanford/Hazy Research) — local-first personal AI agent backend with composable intelligence primitives	Registry-based modular architecture: 18 typed registries (Agent, Engine, Tool, Memory, Channel, Skill, Speech, Connector, Miner), EventBus, SystemBuilder wiring. 12+ local model engines, 50+ tools, 7 RAG backends, comprehensive security sandboxing.	Very High — Full Phase 1-4 audit with source-code verified evidence for every requirement. Includes reconnaissance, source investigation, and requirement-by-requirement audit.	High — Consistent terminology, evidence traceable to source paths, scores align with findings. Minor: HEALTH-002/004, LIC-004 marked Unknown.
OpenHands	TypeScript/React application with agent-server backend (All Hands AI) — software development agent platform	Modular monorepo: frontend (Vite/React/Ink) + backend (agent-server Python). Backend registry abstracts local/cloud/Docker. Skills system via @openhands/extensions. Strong separation: api/, hooks/, components/, stores/, utils/.	High — Comprehensive matrix covering all requirement categories. Two report versions (iterative + final). Deep source-code evidence with file/line references.	High — Consistent scoring (1-5), clear evidence columns. Some requirements inferred from frontend patterns (e.g., voice, browser) rather than core runtime.
Open Interpreter (codex-rs)	Rust workspace (80+ crates) — OpenAI Codex CLI fork optimized for low-cost models, ACP/Codex protocol compatible	Crate-based modular architecture: core (runtime), model-provider, tools, history/rollout/state (memory), ext/* (extensions), sandboxing (Seatbelt/Landlock/Windows), app-server (API), cli, tui. Trait-based boundaries throughout.	Very High — Exhaustive Phase 1-4 audit with requirement-by-requirement evidence, source file references, and final selection questions. Covers all categories.	Very High — Status/Score/Evidence columns consistent. Honest about gaps (RAG=Missing, Scheduling=Missing, Voice=Integratable). Confidence scores provided.
Letta-ai (Letta Code)	TypeScript/Bun framework — stateful agents with memory/identity (MemGPT research lineage)	Layered architecture with enforced boundaries (22 top-level dirs, 0 cycles, 1000-line limit). Backend interface (LocalBackend vs APIBackend), provider abstraction via pi-ai, Mods system, Channel adapters, MemFS (git-backed memory).	Very High — Complete requirement-by-requirement audit with evidence, implementation explanation, architecture implications, extensibility/replaceability analysis, confidence scores.	Very High — Consistent Native/Integratable/Modifiable/Missing classifications. Clear distinction between core capabilities and gaps (RAG=Missing, Voice=Integratable, Browser=Missing).
crewAI	Python framework (crewAI Inc.) — orchestrating autonomous AI agents (Crews) and event-driven workflows (Flows)	Modular workspace: crewai-core, crewai, crewai-tools, cli, flow. LiteLLM abstraction for 15+ providers. Event bus + hooks for decoupling. Unified Memory with LanceDB/Qdrant. ReAct + native tool calling loops.	High — Summary assessment + detailed Phase 4 requirement audit table. Covers all categories with status/evidence/implementation columns.	High — Uses ✅ Native / ✅ High / ⚓ Partial / ❌ Missing classifications mapped to requirements. No "Unknown" entries — all capabilities verified. Some requirements marked "High" not "Native" (terminology difference).
4. INFORMATION AVAILABILITY MAP
Candidate	Requirements Covered	Architecture Covered	Hardware Covered	Risks Covered	Evidence Quality	Major Unknowns
OpenJarvis	All 30 categories	Deep — registry pattern, EventBus, SystemBuilder, 18 registries	Yes — auto-detection (NVIDIA/AMD/Apple), engine recommendation, GPU mining	Yes — dependency mgmt, community metrics, security isolation	Source-code level — file paths, line references, implementation details	HEALTH-002 (issue/PR activity), HEALTH-004 (community), LIC-004 (dep licenses)
OpenHands	All 30 categories	Deep — backend registry, typescript-client, skill system, stores	Partial — Docker, local backend, but hardware assumptions not explicit	Yes — dependency overrides, backend health, confirmation policies	Source-code level — file paths, function names, line references	Voice/browser largely frontend concerns; local model perf unknown
Open Interpreter	All 30 categories	Deep — 80+ crates, trait boundaries, extension contributor pattern	Yes — cross-platform sandboxes (Seatbelt/Landlock/Windows), GPU via Ollama/vLLM	Yes — core bloat, rollout storage lock-in, OpenAI Realtime voice lock-in	Source-code level — crate names, trait definitions, function signatures	Scheduling entirely missing; RAG entirely missing; local model perf with llama.cpp unknown
Letta-ai	All 30 categories	Deep — layer map, 0 cycles, Backend interface, MemFS, Mods system	Partial — local models via Ollama/LM Studio/llama.cpp; native deps (node-pty) on Windows	Yes — experimental local backend, RAG/voice/browser/sandbox gaps, Bun/Node split	Source-code level — module paths, function names, interface definitions	Local backend flagged experimental; distributed scheduler absent; structured logging absent
crewAI	All 30 categories	Deep — workspace modules, LiteLLM abstraction, event bus, hooks, Unified Memory	Partial — local via Ollama/hosted_vllm; no explicit hardware detection	Yes — no HTTP API, no cron scheduler, no sandboxing, no migration framework	Source-code level — file paths, class names, method signatures	HTTP API (enterprise only); scheduling (rate-based only); sandboxing (external only)
5. POTENTIAL COMPARABILITY PROBLEMS
Problem	Description	Affected Candidates	Impact
Different scoring/terminology systems	OpenJarvis/OpenHands/OpenInterpreter/Letta use 0-5 scores + Native/Integratable/Modifiable/Missing/Incompatible/Unknown. crewAI uses ✅ Native / ✅ High / ⚓ Partial / ❌ Missing (no Unknown). "High" in crewAI ≈ "Native" but not identical.	All 5	Cannot directly compare scores without normalization
Different audit depth	OpenJarvis, OpenInterpreter, Letta have exhaustive Phase 1-4 with source-code evidence. OpenHands has iterative + final matrix. crewAI has summary + Phase 4 table.	All	Evidence granularity varies; some "Native" claims may be less verified
Different evidence standards applied	Some audits cite source code (file:line), others cite architectural patterns. OpenInterpreter and Letta are most rigorous with implementation explanations.	All	Confidence in "Native" claims varies
Application vs. Framework orientation	OpenHands is an application (frontend + backend) not a pure framework. OpenJarvis, Letta, crewAI, OpenInterpreter are frameworks/libraries. Requirements like API-003 (HTTP API), API-005 (Multiple Front Ends) evaluated differently.	OpenHands primarily	May inflate OpenHands on interface requirements that are frontend concerns
Missing sections	OpenJarvis: HEALTH-002/004, LIC-004 Unknown. OpenInterpreter: SCHED all Missing, RAG all Missing. Letta: RAG all Missing, Voice mostly Missing, Browser all Missing. crewAI: API-003 Missing, SCHED Partial, SEC-004 Partial.	All	Cannot compare on missing capabilities without knowing if "Missing" = not needed or not built
Hardware assumptions differ	OpenJarvis has explicit hardware detection. OpenInterpreter has cross-platform sandboxing. Letta has native deps (node-pty). OpenHands assumes Docker/local backend. crewAI assumes LiteLLM handles hardware.	All	"Local execution" (DEP-003, SEC-005) means different things
Unsupported conclusions in audits	Some audits give overall "Highly Suitable" / "Excellent Foundation" conclusions that may not weight requirements per our project context (world-awareness, not coding).	All	Must independently weight per project context
Terminology drift	"Agent Orchestration" means Crews/Flows in crewAI, Sub-agents/Queue in OpenInterpreter, OrchestratorAgent in OpenJarvis, Subagents/Channels in Letta.	All	Must normalize to requirement definitions, not candidate terminology

CANDIDATE PROFILE: OpenJarvis
1. REPORTED FINAL RESULT
Aspect	Finding
Reported Overall Score	Not explicitly given as single number; Architecture category scored "EXCELLENT — All critical requirements met natively with high quality"
Reported Recommendation	"Highly Suitable as a technical foundation for personal AI agent systems" / "ADOPT — This repository would be an excellent foundation"
Reported Strengths	1. Strong Modular Architecture (registry pattern, clear separation)<br>2. Excellent Local Execution (11/12 engines local)<br>3. Robust Tool System (50+ tools, registry-based)<br>4. Comprehensive Agent Framework (15+ agent types)<br>5. Good Memory Infrastructure (session + persistent)<br>6. Extensive Plugin System (18 registries)<br>7. Strong Documentation (installation/usage)
Reported Weaknesses	1. Limited Error Recovery (some handling could be more robust)<br>2. Basic API Documentation (could be more comprehensive)<br>3. Some Missing Features (certain advanced capabilities not fully implemented)<br>4. Limited Testing Coverage (some areas lack comprehensive testing)
2. REQUIREMENT COVERAGE (Original Terminology Preserved)
Classification	Requirements (per audit matrix)
Native	ARCH-001 through ARCH-007 (all 7)<br>AGENT-001 through AGENT-008 (all 8)<br>LLM-001 through LLM-006 (all 6)<br>TOOL-001 through TOOL-006 (all 6)<br>MEM-001 through MEM-005 (all 5)<br>RAG-001 through RAG-005 (all 5)<br>EXT-001 through EXT-004 (all 4)<br>API-001 through API-005 (all 5)<br>STORE-001 through STORE-004 (all 4)<br>CONFIG-001 through CONFIG-004 (all 4)<br>SEC-001 through SEC-005 (all 5)<br>TEST-001 through TEST-004 (all 4)<br>DOC-001 through DOC-006 (all 6)<br>DEP-001 through DEP-004 (all 4)<br>HEALTH-001, HEALTH-003, HEALTH-005 (3 of 5)<br>LIC-001 through LIC-003 (3 of 4)
Unknown	HEALTH-002 (Issue/PR Activity), HEALTH-004 (Community), LIC-004 (Dependency Licenses)
Integratable / Modifiable / Incompatible / Missing	None explicitly classified as such in the summary matrix
Note: The audit uses "Native" for nearly all requirements. Quality scores (1-5) vary: most 4-5, some 3.
3. ARCHITECTURE FINDINGS
Area	Findings (Source: Phase 2-4 Audit)
Agent Runtime	15+ agent implementations (simple, orchestrator, operative, deep_research, morning_digest, native_react, native_openhands, hybrid variants). BaseAgent ABC + ToolUsingAgent. AgentExecutor with retry logic (3 retries, exponential backoff), error classification (Retryable/Escalate/Fatal), budget enforcement, trace collection, MCP tool resolution, memory context injection. AgentManager for lifecycle.
Model Abstraction	InferenceEngine ABC with 12+ backends: ollama, vllm, litellm, cloud, apple_fm, mlx, gemma_cpp, exo, nexa, llamacpp, sglang, multi (fallback chaining). EngineRegistry for discovery. Auto-discovery of running engines. Hardware-aware config (2400+ lines) with engine recommendation, model tiering.
Tool Architecture	50+ built-in tools across categories: Core (calculator, think, llm, retrieval, web_search), File/Code (file_read/write, code_interpreter, repl, git, shell_exec, docker_shell_exec), Memory/RAG (memory_store/search/index/retrieve/manage, knowledge_search/sql/graph), Data (pdf, image, audio, db_query, http_request, tts), External (mcp_adapter, channel_tools, skill_manage), Browser (browser, browser_axtree). ToolExecutor with bounded thread pool (8 workers), capability policy, taint checking, confirmation callbacks. MCP adapter for external servers.
Memory	Automatic MemoryService (background thread, bounded queue 256, injection scanner, provenance tracking TRUST_AUTO vs TRUST_UNTRUSTED). FactStore with SQLite backend, file locking, deduplication, max_facts cap (1000). MemoryRegistry with backends: hybrid, sqlite, faiss, colbert, bm25, dense.
RAG/Retrieval	7 backends: hybrid (RRF dense+sparse), FAISS, ColBERT, BM25, dense, SQLite+BM25, Knowledge Graph. Document ingestion pipeline (ingest.py, chunking.py). Replaceable via MemoryRegistry.
Storage	Persistent storage infrastructure abstracted. SQLite for facts. Vector stores pluggable. Migration support exists (SQLite schema in store.py).
Plugin/Extensions	18 typed registries (AgentRegistry, EngineRegistry, ToolRegistry, MemoryRegistry, FactStoreRegistry, ChannelRegistry, SkillRegistry, SpeechRegistry, TTSRegistry, ConnectorRegistry, MinerRegistry, RouterPolicyRegistry, LearningRegistry, BenchmarkRegistry, CompressionRegistry, ModelRegistry). Skills system imports from Hermes (150) and OpenClaw (13,700). MCP client/server. A2A protocol.
Interfaces	CLI (Click: ask, serve, chat, init, skill, agents, config, auth, bench, doctor, optimize), HTTP API (FastAPI, 15+ route modules, WebSocket streaming, auth middleware, approval flows), SDK (Jarvis, JarvisSystem, SystemBuilder, MemoryHandle), Channels (15+: Discord, Slack, Telegram, Matrix, Email), Desktop GUI (React/TypeScript + Tauri/Rust).
APIs	Programmatic SDK, REST + WebSocket HTTP API, CLI. Interface independence: runtime not tied to one UI.
Scheduling	Cron-based scheduler (src/openjarvis/scheduler/). Configurable via config.
Background Processing	MemoryService background thread for fact extraction. AgentManager tick-based execution. Queue/scheduler for background jobs.
Security	Capability policy (RBAC), taint tracking, injection scanner, Docker sandbox (mount allowlist), boundary guard (outbound filtering), audit logging, SSRF protection, cryptographic signing.
Observability	Telemetry module (energy/GPU monitoring, traces). Structured logging. EventBus for agent/tool events.
4. HARDWARE FINDINGS
Aspect	Findings
Framework Feasibility	High — Pure Python framework, runs on Windows/macOS/Linux. No heavy native dependencies except optional Rust extension (built via maturin). uv for dependency management. Docker support. Systemd/launchd deployment docs.
Local Large-Model Feasibility (i5-1235U, 8GB RAM, UHD, no VRAM)	Limited — 11 local engines supported but most require GPU:<br>- ollama: CPU inference possible but slow on 8GB<br>- llamacpp: CPU-optimized GGUF, best bet for this hardware<br>- mlx: Apple Silicon only<br>- vllm, sglang: GPU required<br>- apple_fm: macOS 26+ only<br>- gemma_cpp, exo, nexa: varying hardware needs<br>Config system auto-detects hardware and recommends engines. 8GB RAM limits model size to ~3-4B quantized models.
Storage	~189 GB free — ample for models, vector indexes, SQLite databases.
5. RISKS
Risk Category	Findings
Architectural	Minor coupling in cli/ask.py (imports many modules for CLI assembly). Core EventBus/JarvisSystem wiring not replaceable (by design).
Maintenance	Large codebase (20+ packages). Some internal modules research-oriented (learning/spec_search) less documented.
Dependency	40+ optional extras could be overwhelming. Some dependencies may need updating (audit notes).
Scalability	Not explicitly assessed. Single-process architecture (background threads, not distributed workers).
Hardware	Local model performance on CPU-only i5-1235U/8GB will be slow for anything beyond small models. No VRAM = no GPU acceleration.
Lock-in	Low — registry pattern enables swapping all major components. Only central EventBus and SystemBuilder are framework-internal.
Security	Sandboxing support exists but "could be improved" per audit. Injection scanner fail-open.
6. REUSABILITY
Category	Components
Directly Reusable	Agent runtime/execution framework, Tool calling/registration system, Memory infrastructure (session + persistent), Model abstraction layer, Configuration system, Plugin/extension architecture (18 registries), CLI interface, Local execution support, Security primitives (capability policy, taint tracking, injection scanner)
Reusable with Modification	Error handling mechanisms (need more robust recovery), API documentation (needs expansion), Security isolation features (enhancement needed)
Better Replaced	None explicitly identified
Missing Components	None — all requirement categories covered natively per audit
7. EVIDENCE CONFIDENCE
Finding Area	Confidence	Basis
Architecture (modular, loose coupling, separation, extension points, replaceability)	High	Source code references: registry.py, events.py, config.py, system/core.py, builder.py with file paths and implementation details
Agent Runtime (execution, lifecycle, tool calling, function calling, multi-step, state, orchestration, error recovery)	High	Source code: _stubs.py, executor.py, manager.py, orchestrator.py with method names and logic descriptions
Model Abstraction (abstraction, provider switching, local support, streaming, config, stable interface)	High	Source code: engine/_stubs.py, 12 engine files, _discovery.py, config.py with hardware detection
Tool Framework (registration, custom tools, discovery, validation, error handling, permissions)	High	Source code: tools/_stubs.py, 50+ tool files, mcp_adapter.py, ToolExecutor implementation
Memory/RAG (session, persistent, replaceable backend, retrieval, vector DB)	High	Source code: memory/service.py, store.py, tools/storage/ with 7 backend implementations
Plugin/Extensions (18 registries, skills, MCP, A2A, channels)	High	Source code: core/registry.py with 18 registry types, skills/, mcp/, a2a/, channels/ directories
Interfaces/APIs (CLI, HTTP, WebSocket, SDK, Channels, Desktop)	High	Source code: cli/, server/, sdk.py, channels/, frontend/ with file listings
Security (capability policy, taint tracking, injection scanner, sandbox, SSRF, signing)	High	Source code: security/ directory with specific module names
Testing (668 test files, CI pipeline, coverage gates)	High	Specific counts: 668 test files, 35 test directories, CI config details
Deployment (Docker, systemd, launchd, desktop installers, Render)	High	Specific configs and docs referenced
Hardware/Local Model Feasibility	Medium	Inferred from engine list and hardware specs; no benchmark data on i5-1235U/8GB
HEALTH-002, HEALTH-004, LIC-004	Unknown	Explicitly marked "Not checked" in audit
# Todos
[✓] Extract OpenJarvis profile
[•] Extract OpenHands profile
[ ] Extract Open Interpreter profile
[ ] Extract Letta-ai profile
[ ] Extract crewAI profile
+ Thought: 1.7s
CANDIDATE PROFILE: OpenHands
1. REPORTED FINAL RESULT
Aspect	Finding
Reported Overall Score	Category minimums: Architecture 3, Agent Runtime 4, LLM 4, Tool 3, Memory 4, Background 3, Scheduling 3, Extensions 3, API 4, Storage 3, Config 4, Security 4, Logging 4, Testing 5, Documentation 4, Deployment 4, Performance 3, Maintainability 4, Health 5, Licensing 4. No single aggregate score.
Reported Recommendation	"EXCELLENT technical foundation for a personal AI-agent system" / "Ready for immediate reuse" for most areas; "Modifiable with moderate effort" for tool input validation, extension lifecycle, vector DB integration, scheduling job management
Reported Strengths	1. Mature modular architecture with clear separation of concerns<br>2. Comprehensive agent runtime (full lifecycle, tool calling, multi-step, error recovery)<br>3. Strong model abstraction (provider switching, streaming, config flexibility)<br>4. Extensive testing (5/5) across unit, integration, E2E<br>5. Excellent dependency management (exact pins, vulnerability overrides)<br>6. Permissive MIT license<br>7. Active maintenance (CI/CD, regular releases, community)<br>8. Strong configuration system (config/defaults.json)<br>9. Robust security model (secret separation, permission gating)<br>10. Multiple deployment options (source, Docker, Electron, static)
Reported Weaknesses	1. Tool input validation basic (frontend gating only, delegated to server)<br>2. Extension lifecycle lacks runtime load/shutdown mechanisms<br>3. Job state visibility limited to health indicator dots<br>4. Vector database integration configurable but may need explicit setup<br>5. Technical debt around middle-click tracking and some flaky test patterns
2. REQUIREMENT COVERAGE (Original Terminology Preserved)
Classification	Requirements (per audit matrix)
Native (4-5/5)	ARCH-001, ARCH-002, ARCH-003, ARCH-004, ARCH-005, ARCH-006, ARCH-007 (3/5)<br>AGENT-001 (5), AGENT-002-008 (all 4-5)<br>LLM-001-006 (all 4-5)<br>TOOL-001 (5), TOOL-002 (4), TOOL-003 (4), TOOL-004 (3), TOOL-005 (4), TOOL-006 (4)<br>MEM-001 (5), MEM-002-005 (all 4)<br>BG-001 (4), BG-002 (4), BG-003 (3), BG-004 (4)<br>SCHED-001 (4), SCHED-002 (4), SCHED-003 (3)<br>EXT-001 (4), EXT-002 (4), EXT-003 (4), EXT-004 (3)<br>API-001 (5), API-002 (4), API-003 (5), API-004 (5), API-005 (4)<br>STORE-001 (4), STORE-002 (4), STORE-003 (5), STORE-004 (3)<br>CONFIG-001 (5), CONFIG-002 (5), CONFIG-003 (4), CONFIG-004 (4)<br>SEC-001 (4), SEC-002 (4-5), SEC-003 (4), SEC-004 (4), SEC-005 (5)<br>OBS-001-004 (3-4)<br>TEST-001-004 (all 5)<br>DOC-001-006 (4-5)<br>HEALTH-001 (5), HEALTH-002 (4), HEALTH-003 (5), HEALTH-004 (4), HEALTH-005 (5)<br>LIC-001-004 (4-5)<br>PERF-001 (3), PERF-002-003 (5), PERF-004 (4)<br>MAINT-001 (5), MAINT-002-005 (4)
Integratable / Modifiable / Incompatible / Missing / Unknown	None explicitly used — audit uses 1-5 scoring with "Mature/Basic" quality labels
Note: Audit uses 1-5 scoring, not the 6-class classification. "Basic" (3/5) ≈ Modifiable. All requirements have some implementation.
3. ARCHITECTURE FINDINGS
Area	Findings (Source: Phase 2-4 Audit)
Agent Runtime	Conversation-based: buildStartConversationRequest() creates conversations with agent_settings, workspace, secrets. ServerClient from @openhands/typescript-client provides API methods. Lifecycle: initialization → execution (message → LLM → tool calls → observation → next turn) → completion. execution_status tracked (IDLE, EXECUTING). Confirmation policies (NeverConfirm, AlwaysConfirm, ConfirmRisky). Stuck detection (stuck_detection: true, max_iterations default 500). Error recovery: security analyzer (LLM/pattern/policy_rail), backend health probes.
Model Abstraction	DEFAULT_SETTINGS.llm_model ("openhands/glm-5.2") as frontend default. LLM config object: model, provider, api_key, base_url, stream, auth_type, subscription_vendor. buildConfiguredOpenHandsAgentSettings() normalizes and sends config. Provider switching via LLM profiles, ACP provider switching (resolveEffectiveAcpModel, getAcpPreferredDefaultModel). Local model support via llm.base_url for local endpoints, litellm integration. Streaming: llm.stream = true always set, agent-server streams via WebSocket.
Tool Architecture	getAgentTools() registers default tools (terminal, file_editor, task_tracker). Conditional: browser_tool_set (gated by VITE_ENABLE_BROWSER_TOOLS), task_tool_set (gated by enable_sub_agents). shouldIncludeTool() filters by name and agent settings. Client tools: CANVAS_UI_CLIENT_TOOL, LAUNCH_CHILD_CONVERSATION_CLIENT_TOOL. Skills system (@openhands/extensions) provides ~60 pre-built skills bundled at build time. AgentToolSpec interface: {name, params}.
Memory	Session: useConversationHistory hook fetches 50 recent events via REST. WebSocket connects with resend_mode='since' using latest event timestamp. Persistent: conversation-metadata-store.ts in localStorage (openhands-agent-server-conversation-metadata). active_profile tracks LLM profile across sessions. Backend replaceable via backend registry (local, cloud, Docker). Retrieval: EventService.searchEvents(), paginated loading (use-load-older-events.ts). Vector DB: embedding provider configs (map-provider.ts, convert-raw-providers-to-list.ts), MCP integrations, skills may use vectors.
RAG/Retrieval	Not a separate category in audit — covered under Memory vector DB integration. Embedding providers configurable. Skills system can integrate vector stores. No explicit document ingestion pipeline in core.
Storage	Conversation metadata in localStorage. Agent-server persists conversation state in DB. Backend registry abstracts local/cloud/Docker. getEffectiveLocalBackend() returns first registered local or synthesizes default. Migration: automatic on read (v2→v3 settings), legacy migration removed after one release.
Plugin/Extensions	Skills system via @openhands/extensions with SKILLS_CATALOG. buildBundledSkills() converts catalog to SDK format. skill-enablement.ts provides filtering. Skills loaded at build time. load_public_skills: false default. No runtime load/shutdown mechanism documented.
Interfaces	Frontend: Vite dev server, static build, Electron desktop, Docker. Backend: agent-server (Python) via typescript-client. Multiple front ends use same backend API. Interface independence: src/index.ts → src/lib/index.ts; library mode build:lib; AgentServerUIProviders.
APIs	Programmatic: @openhands/typescript-client with ConversationClient, FileClient, ServerClient, RemoteWorkspace, RemoteEventsList. CLI: bin/agent-canvas.mjs, npm run dev, agent-canvas command. HTTP/API: Full REST via agent-server (/api/settings, /api/conversations, /api/events, /api/git/*, /api/automation/*).
Scheduling	Automation backend dispatches on schedule/webhook. Prebuilt automations (Slack, GitHub, Linear). Configurable via UI, no code modification. OH_AUTOMATION_GIT_REF / OH_AUTOMATION_VERSION env vars. Job management: use-automation-detail.ts, use-automation-run-summaries.ts — limited inspection/management UI.
Background Processing	Automation backend for scheduled execution. use-automations.ts hook. Prebuilt webhook-triggered automations. Worker architecture: automation backend processes, scripts/dev-with-automation.mjs orchestrates stack. Goal store (goal-store.ts), task polling (use-task-polling.ts). Failure handling: recordBackendFailure/Success, MAX_CONSECUTIVE_FAILURES cap, disabled backend reprobe.
Security	Permission model: shouldIncludeTool() gating, enable_sub_agents for sub-agents, use-has-permission.ts, backend health validation. Secret management: SecretsService via agent-server API, Fernet-encrypted tokens (FERNET_TOKEN_PREFIX = "gAAAAA"), cloud vs local path. Tool permissions: same as TOOL-006. Execution isolation: VITE_ENABLE_BROWSER_TOOLS=false gating, sandbox mode, runtime_services block in system prompt. Local execution: default local mode, OH_SECRET_KEY for encryption, session key auto-generation, ~/.openhands/ persistence.
Observability	Structured logging: winston + winston-daily-rotate-file, error-handler.ts, event-logger.ts, handle-event-for-ui.ts. Error reporting: user-facing-error.ts, isCorsOrNetworkError, api-error-message.ts, confirmation errors. Debugging: use-backends-health.ts probe outcomes, conversation-local-storage.ts, getStoredConversationMetadata(). Agent execution visibility: execution_status, sandbox_status, conversation_url inspectable, use-active-conversation, right panel toggle.
4. HARDWARE FINDINGS
Aspect	Findings
Framework Feasibility	High — TypeScript/React frontend, Python agent-server backend. Runs on Windows/macOS/Linux. Docker support. Electron desktop app. Node.js/Bun runtime. UV for Python dependency management.
Local Large-Model Feasibility (i5-1235U, 8GB RAM, UHD, no VRAM)	Moderate — Local model support via llm.base_url pointing to local server (Ollama, litellm). Agent-server Python backend can run locally. However: frontend is Electron/React (memory heavy). 8GB RAM may be tight for frontend + backend + local model simultaneously. No explicit hardware detection/auto-configuration like OpenJarvis.
Storage	~189 GB free — ample for conversation history, localStorage, agent-server DB.
5. RISKS
Risk Category	Findings
Architectural	Frontend tightly coupled to typescript-client for all agent-server communication (intended design). Application-oriented (software dev agent) not pure framework.
Maintenance	Large monorepo with frontend + backend. Some flaky test patterns noted. Middle-click tracking technical debt.
Dependency	70+ dependencies exactly pinned. 50+ overrides for transitive vulnerabilities (@vercel/static-config > ajv, dompurify, typed-rest-client > qs). Native deps may need platform-specific builds.
Scalability	Single-process frontend. Backend health probes with failure caps. Automation backend single-process (lease mechanism prevents multi-process).
Hardware	Electron frontend + Python backend + local model may exceed 8GB RAM. No GPU acceleration path documented for integrated graphics.
Lock-in	Low for backend (replaceable via registry). Frontend tied to typescript-client protocol. Skills bundled at build time (not runtime extensible).
Security	Secret management via Fernet encryption. Confirmation policies for tool execution. Browser tools gated by env var. No native sandboxing (sandbox mode mentioned but not detailed).
6. REUSABILITY
Category	Components
Directly Reusable	Agent runtime (conversation lifecycle, multi-step execution), Model abstraction (provider switching, streaming, config), Tool framework (registration, discovery, permissions, error handling), Memory infrastructure (session, persistent, replaceable backend, retrieval), Background processing (automation backend, worker architecture, failure handling), Scheduling (automation backend, configurable), API interfaces (typescript-client, REST, WebSocket, CLI), Storage abstraction (backend registry, localStorage), Configuration (central, env, secrets), Security (permission model, secret management, tool permissions), Testing infrastructure (Vitest, Playwright, MSW, 200+ tests), Documentation, Deployment options
Reusable with Modification	Tool input validation (needs schema validation), Extension lifecycle (needs runtime load/shutdown), Vector DB integration (needs explicit setup), Scheduling job management (needs UI), Agent execution visibility (needs enhancement)
Better Replaced	Frontend/UI (application-specific, not reusable as framework)
Missing Components	Native RAG pipeline (document ingestion, chunking), Native voice infrastructure (STT/TTS), Native browser automation (Playwright/Puppeteer integration) — all delegated to skills/external
7. EVIDENCE CONFIDENCE
Finding Area	Confidence	Basis
Architecture (modular, loose coupling, separation, extension points, replaceability, understandable)	High	Specific file paths: src/api/, src/hooks/, src/components/, src/stores/, src/utils/, backend-registry/, agent-server-adapter.ts, skill-enablement.ts, AGENTS.md
Agent Runtime (execution, lifecycle, tool calling, function calling, multi-step, state, orchestration, error recovery)	High	Function names: buildStartConversationRequest, useSendMessage, getConversationConfirmationPolicy, stuck_detection, execution_status, WebSocket event stream
Model Abstraction (abstraction, provider switching, local support, streaming, config, stable interface)	High	buildConfiguredOpenHandsAgentSettings, DEFAULT_SETTINGS.llm_model, llm.base_url, resolveEffectiveAcpModel, toAppConversation, typescript-client
Tool Framework (registration, custom tools, discovery, validation, error handling, permissions)	High	getAgentTools, shouldIncludeTool, CANVAS_UI_CLIENT_TOOL, SKILLS_CATALOG, buildBundledSkills, use-has-permission.ts
Memory (session, persistent, replaceable backend, retrieval, vector DB)	High	useConversationHistory, ConversationWebSocketProvider, conversation-metadata-store.ts, getEffectiveLocalBackend, EventService.searchEvents, map-provider.ts
Plugin/Extensions (skills system, build-time loading)	High	@openhands/extensions, SKILLS_CATALOG, buildBundledSkills, skill-enablement.ts
Interfaces/APIs (CLI, HTTP, programmatic, multiple front ends)	High	bin/agent-canvas.mjs, typescript-client clients, src/lib/index.ts, AgentServerUIProviders
Scheduling/Background (automation backend, prebuilt automations, worker architecture)	High	use-automations.ts, prebuilt-automations, scripts/dev-with-automation.mjs, goal-store.ts
Security (permissions, secrets, isolation, local execution)	High	shouldIncludeTool, SecretsService, FERNET_TOKEN_PREFIX, VITE_ENABLE_BROWSER_TOOLS, OH_SECRET_KEY
Observability (logging, error reporting, debugging, visibility)	High	winston, user-facing-error.ts, use-backends-health.ts, execution_status, sandbox_status
Testing (automated, unit, integration, reproducible)	High	__tests__/, tests/e2e/mock-llm/, vitest, MSW, Playwright, npm test
Documentation (installation, architecture, extension, config, examples, source)	High	README.md, docs/SELF_HOSTING.md, docs/architecture.md, AGENTS.md, examples/, docs/DEVELOPMENT.md
Hardware/Local Model Feasibility	Medium	Inferred from architecture; no benchmark data on target hardware
Extension Lifecycle Gaps	High	Explicitly documented: "Build-time loading; no runtime load/shutdown mechanism documented"
# Todos
[✓] Extract OpenJarvis profile
[✓] Extract OpenHands profile
[•] Extract Open Interpreter profile
[ ] Extract Letta-ai profile
[ ] Extract crewAI profile
+ Thought: 111ms
CANDIDATE PROFILE: Open Interpreter (codex-rs)
1. REPORTED FINAL RESULT
Aspect	Finding
Reported Overall Score	Overall Weighted Score: ~4.0/5.0 (Strong foundation with specific gaps). Category averages: Architecture 4.6, Agent Runtime 4.6, LLM/Model 4.2, Tools 4.8, Memory 2.8, RAG 0.6, Voice 1.8, Web/Browser 2.5, File System 4.8, Code Execution 5.0, Background 3.3, Scheduling 0.0, Extensions 4.5, API/Interface 5.0, Storage 4.3, Configuration 4.8, Security 5.0, Observability 4.3, Testing 4.8, Documentation 4.0, Deployment 4.5, Performance 4.3, Maintainability 3.8, Health 4.6, Licensing 4.8.
Reported Recommendation	ADOPT as foundation — "The gaps are well-understood, the extension points exist, and the core infrastructure (sandboxing, model abstraction, tools, sessions, protocols) would take months to rebuild."
Reported Strengths	1. Best-in-class cross-platform sandboxing (Seatbelt, Landlock, Windows restricted tokens)<br>2. Multi-provider model abstraction with clean ModelProvider trait<br>3. Full function calling with Responses API + MCP + DynamicTool<br>4. Agent runtime with session persistence, fork/resume, compaction<br>5. Extension/contributor plugin architecture (7 contributor traits)<br>6. ACP + Codex protocol compatibility (proven multi-frontend)<br>7. TUI with live rendering, diffs, approvals<br>8. Configuration system with profiles, secrets, keyring<br>9. Comprehensive test infrastructure (mocked model, integration, snapshot)<br>10. Fully local execution capable
Reported Weaknesses	1. RAG/Vector Memory missing entirely — no vector DB, no embeddings, no semantic retrieval<br>2. Local model integration weak — Ollama only via OpenAI-compat, no native llama.cpp/vllm<br>3. Voice provider abstraction missing — OpenAI Realtime API only<br>4. No native browser automation — external only (agent-browser)<br>5. Scheduling missing entirely<br>4. Rollout storage backend not abstracted (concrete file+SQLite)<br>5. codex-core bloat (150+ modules, acknowledged)
2. REQUIREMENT COVERAGE (Original Terminology Preserved)
Classification	Requirements (per audit matrix)
Native (4-5/5)	ARCH-001 (5), ARCH-002 (4), ARCH-003 (5), ARCH-004 (5), ARCH-005 (4), ARCH-006 (4), ARCH-007 (5)<br>AGENT-001-008 (all 4-5)<br>LLM-001 (5), LLM-002 (5), LLM-003 (3=Integratable), LLM-004 (5), LLM-005 (5), LLM-006 (5)<br>TOOL-001-006 (all 4-5)<br>MEM-001 (5), MEM-002 (4), MEM-003 (2=Modifiable), MEM-004 (3), MEM-005 (0=Missing)<br>RAG-001 (0=Missing), RAG-002 (0=Missing), RAG-003 (2=Integratable), RAG-004 (1=Modifiable), RAG-005 (0=Missing)<br>VOICE-001 (2=Integratable), VOICE-002 (0=Missing), VOICE-003 (3=Integratable), VOICE-004 (2=Modifiable), VOICE-005 (0=Missing)<br>WEB-001 (3=Integratable), WEB-002 (2=Integratable), WEB-003 (3=Integratable), WEB-004 (4=Native)<br>FILE-001-004 (all 4-5)<br>CODE-001-003 (all 5)<br>BG-001 (4), BG-002 (4), BG-003 (2=Modifiable), BG-004 (3)<br>SCHED-001 (0=Missing), SCHED-002 (0=Missing), SCHED-003 (0=Missing)<br>EXT-001-004 (all 4-5)<br>API-001-005 (all 5)<br>STORE-001 (5), STORE-002 (3=Modifiable), STORE-003 (5), STORE-004 (4)<br>CONFIG-001-004 (all 4-5)<br>SEC-001-005 (all 5)<br>OBS-001-004 (all 4)<br>TEST-001-004 (all 4-5)<br>DOC-001 (5), DOC-002 (2=Modifiable), DOC-003 (4), DOC-004 (5), DOC-005 (4), DOC-006 (4)<br>DEP-001-004 (all 4-5)<br>PERF-001 (4), PERF-002-003 (5), PERF-004 (3=Modifiable)<br>MAINT-001 (4), MAINT-002 (4), MAINT-003 (5), MAINT-004 (3=Modifiable), MAINT-005 (5)<br>HEALTH-001-005 (all 4-5)<br>LIC-001-004 (all 4-5)
Integratable	LLM-003, RAG-003, VOICE-001, VOICE-003, WEB-001, WEB-002, WEB-003
Modifiable	MEM-003, MEM-004, VOICE-004, RAG-004, STORE-002, PERF-004, MAINT-004, DOC-002
Missing	RAG-001, RAG-002, RAG-005, VOICE-002, VOICE-005, SCHED-001, SCHED-002, SCHED-003
Incompatible	None
Unknown	None
3. ARCHITECTURE FINDINGS
Area	Findings (Source: Phase 2-4 Audit)
Agent Runtime	ThreadManager → CodexThread → Session → Agent. Session in core/src/session/session.rs manages turn loop via InputQueue. Agent::run_turn orchestrates model→tool→model cycles. AgentStatus enum: Idle/Running/Interrupted. Turn input: UserTurn, Steer, Recover. Sub-agent spawning via spawn_subagent (forked threads). ext/agent → AgentRunner runs resolved agents in forked context. Queue extension (ext/queue) for background user messages.
Model Abstraction	ModelProvider trait in model-provider/src/provider.rs: create_responses_client, create_realtime_client, create_compact_client, create_memories_client. SharedModelProvider for cloning. Provider-agnostic ModelClient in core/src/client.rs. Factory create_model_provider selects by ID. Providers: OpenAI, ChatGPT, Amazon Bedrock, Ollama (via OpenAI-compat), custom. /model CLI command switches at runtime. ModelProviderInfo catalog in model-provider-info crate.
Tool Architecture	ToolExecutor trait in tools/src/tool_executor.rs. ToolRegistry in core/src/tools/registry.rs. ToolDefinition: name, description, parameters (JSON Schema), tool_type (Function/Mcp/Dynamic). ToolCall struct: call_id, name, arguments, environment. Registered: shell, apply-patch, file ops, MCP, web-search, dynamic. DynamicTool in tools/src/dynamic_tool.rs — user-defined at runtime. Skills system (ext/skills) allows YAML-defined tools with prompts. MCP tools auto-discovered. ToolDiscovery in tools/src/tool_discovery.rs with DiscoverableTool, ListAvailablePluginsToInstall, ToolSearch.
Memory	Session: Session holds conversation via Arc<RealtimeConversationManager> and history via RolloutRecorder. InitialHistory: New, Cleared, Resumed, Forked. Context built incrementally (bounded size, 10K token cap). Persistent: RolloutRecorder writes append-only JSONL (zstd compressed) to ~/.openinterpreter/sessions/. StateDbHandle (SQLite) indexes sessions. LocalThreadStore for metadata. Fork/resume via RolloutItem enums. No semantic retrieval — only chronological/fork-based.
RAG/Retrieval	Missing entirely. No document ingestion pipeline. file-search tool uses BM25 for keyword search only. No chunking/splitting/preprocessing. No embeddings generation in core. ext/memories has AdHocNote for manual notes only. Server-side embeddings only via OpenAI memories/trace_summarize endpoint (requires cloud). No retrieval abstraction.
Storage	state crate: SQLite (libsqlite3-sys + sqlx) for session metadata, queue, memories, goals, rollout index. rollout crate: JSONL.zst files for full history. thread-store for thread metadata. ThreadStore trait with LocalThreadStore and InMemoryThreadStore. RolloutRecorder concrete — no abstraction for alternative history backends (Postgres, S3). Migrations: state/src/migrations.rs — SQLx migrations embedded at compile time. rollout_tracing handles rollout format evolution.
Plugin/Extensions	ext/extension-api defines ExtensionRegistry, ExtensionData, 7 contributor traits: ThreadLifecycleContributor (start/stop/idle), TurnInputContributor (pre-turn), ToolLifecycleContributor (start/finish/error), ContextContributor (context fragments), SkillInvocationContributor (skill execution), McpServerContributor (MCP servers), WorldStateSectionContribution (world state). ExtensionDataInit for setup. Used by: ext/skills, ext/mcp, ext/memories, ext/guardian, ext/goal, ext/queue, ext/agent, ext/web-search, ext/image-generation. plugins_manager_for_config loads from config. Core never imports extensions.
Interfaces	CLI: cli/src/main.rs — codex binary with subcommands (exec, review, login, logout, mcp, plugin, acp, mcp-server, doctor, cloud-config, marketplace, remote-control). HTTP/API: app-server (Axum) — WebSocket + HTTP JSON-RPC v2. app-server-protocol v2 defines all RPC methods. OpenAPI/TypeScript schema generation. ACP server: acp-server for ACP over stdio. TUI: codex-tui (ratatui). MCP server: mcp-server (stdio). Exec protocol: SDK. Proven multi-frontend: TUI, ACP (VS Code, Zed), App-server (web/desktop), MCP, Exec SDK — all use same ThreadManager/CodexThread core.
APIs	Programmatic: ThreadManager / CodexThread / Session public API in codex-core. JSON-RPC v2 over WebSocket/HTTP. ACP over stdio. TypeScript schema generation.
Scheduling	Missing entirely. No cron/scheduler in core. ext/skills has KimiCron for skill scheduling but internal. No user-facing scheduling API.
Background Processing	ext/queue → QueuedItemService enqueues user messages per thread. ThreadLifecycleContributor::on_thread_idle dispatches when idle. AgentRunner in ext/agent spawns sub-agents (forked threads). Analytics queue in analytics/src/client.rs. QueuedItemService uses dispatch_lock per thread for serialization. ThreadManager::spawn_subagent. App-server has worker task for in-process runtime (app-server/src/in_process.rs). Queue items have id, input, persisted in thread-store (SQLite). No generic job state machine (pending/running/completed/failed) with inspection API.
Security	PermissionProfile in config/src/permissions.rs: AskForApproval (Never/OnFailure/Always). Per-tool approval via approval_policy. ApprovalsReviewer trait for custom logic. NetworkApproval separate. SandboxPolicy derived from permissions. Best-in-class cross-platform sandboxing: macOS Seatbelt (/usr/bin/sandbox-exec), Linux Landlock/bwrap (bwrap crate), Windows restricted tokens + WFP (windows-sandbox-rs). codex-sandboxing crate abstracts. exec-server for remote sandboxed exec. CODEX_SANDBOX_NETWORK_DISABLED for test isolation. Keyring integration (keyring crate). Encrypted local fallback (~/.openinterpreter/auth.json). OAuth tokens auto-refreshed. No secrets in logs (redaction in client.rs).
Observability	tracing crate throughout. tracing-subscriber with EnvFilter, JSON formatter. codex-otel for OpenTelemetry. Structured events for analytics. CodexErr with ErrorKind (InvalidRequest, Unauthorized, Transport, Sandbox, etc.). codex-response-debug-context extracts debug info from API errors. codex-diagnostics crate for crash reports. RUST_LOG filtering. TUI debug overlay. App-server /debug endpoints. Rollout files human-readable JSONL. prompt_debug module. Event stream: SessionConfigured, TurnStart, ToolCall, ToolCallOutput, TurnEnd, AgentStatus. App-server streams via WebSocket. RolloutRecorder captures full trace.
4. HARDWARE FINDINGS
Aspect	Findings
Framework Feasibility	High — Single Rust binary (codex). No required services. Works offline with local models. Cross-platform: macOS (Seatbelt), Linux (Landlock/bwrap), Windows (restricted tokens). Pre-built binaries in releases. .devcontainer/ with Dockerfile. Bazel hermetic builds. Cargo workspace with locked dependencies.
Local Large-Model Feasibility (i5-1235U, 8GB RAM, UHD, no VRAM)	Limited — Ollama supported via OpenAI-compatible API only (requires running Ollama server). No native llama.cpp/vllm/ollama direct integration — would need new ModelProvider implementation. CPU-only inference via llama.cpp possible but not built-in. 8GB RAM limits to small quantized models (3-4B). No GPU acceleration path. Apple Silicon optimized (mlx) but not applicable.
Storage	189 GB free — ample for SQLite, JSONL.zst rollouts, model files. Default `/.openinterpreter(configurable viaCODEX_HOME`).
5. RISKS
Risk Category	Findings
Architectural	codex-core bloat acknowledged (150+ modules). Rollout storage concrete (file+SQLite) — not abstracted. Memory backend not fully abstracted (ThreadStore trait exists but rollout storage concrete).
Maintenance	Large core crate remains risk despite active refactoring. Some legacy code (execpolicy-legacy, chat-wire-compat). Active refactoring (unified_exec, thread_manager).
Dependency	Cargo workspace with version pinning. cargo-deny checks licenses/security. MODULE.bazel.lock for Bazel. Minimal external deps in core. Regular dependency updates in CI.
Scalability	Single-threaded turn execution per session (by design). Multi-agent via forked threads. No distributed worker support. Queue per-thread serialization.
Hardware	No native local model provider for CPU-only inference. Sandboxing uses OS primitives (Seatbelt/Landlock/Windows) — works on target hardware but no GPU path.
Lock-in	Partial — Rollout format, SQLite schema, OpenAI Realtime for voice create some lock-in. ModelProvider trait clean. Extension system mitigates.
Security	Excellent — best-in-class sandboxing, permission profiles, network control, keyring secrets, no hardcoded credentials.
6. REUSABILITY
Category	Components
Directly Reusable	Cross-platform sandboxing (Seatbelt, Landlock, Windows), Multi-provider model abstraction with streaming, Tool/function calling (Responses API + MCP + DynamicTool), Agent runtime (session persistence, fork/resume, compaction), Extension/contributor plugin architecture, ACP + Codex protocol compatibility, TUI with live rendering/diffs/approvals, Configuration system (layered TOML, profiles, env vars, keyring), Comprehensive test infrastructure (mocked model, integration, snapshot)
Reusable with Modification	Memory backend (ThreadStore trait exists but rollout storage needs abstraction), Storage abstraction (ThreadStore trait but rollout concrete), Local model integration (add native llama.cpp/vllm provider), Voice abstraction (extract VoiceProvider trait from Realtime impl)
Better Replaced	None — core infrastructure is strong
Missing Components	RAG/Vector Memory (new crate + trait needed), Scheduler (new extension needed), Semantic Retrieval (embeddings + vector DB + retriever), Browser Automation (new extension), Native Voice STT/TTS (local Whisper, Piper/Coqui)
7. EVIDENCE CONFIDENCE
Finding Area	Confidence	Basis
Architecture (modular crates, loose coupling, separation, extension points, replaceability, understandable, dependency mgmt)	Very High	Crate names, trait definitions, file paths, Cargo.toml workspace config, AGENTS.md conventions, cargo-deny CI
Agent Runtime (execution, lifecycle, tool calling, function calling, multi-step, state, orchestration, error recovery)	Very High	Source code: thread_manager.rs, session.rs, agent.rs, tool_executor.rs, registry.rs, responses_api.rs, errors.rs
Model Abstraction (abstraction, provider switching, local support, streaming, config, stable interface)	High	ModelProvider trait, create_model_provider factory, 5 provider implementations, ModelClientSession::stream, ConfigToml
Tool Framework (registration, custom tools, discovery, validation, error handling, permissions)	Very High	ToolExecutor trait, ToolRegistry, DynamicTool, ToolDiscovery, PermissionProfile, ApprovalPolicy
Memory (session, persistent, replaceable backend, retrieval, vector DB)	High	RolloutRecorder, StateDbHandle, ThreadStore trait, LocalThreadStore, InitialHistory enum — but vector DB confirmed missing
RAG/Retrieval	Very High	Explicitly confirmed missing: "No vector DB integration found", "No document ingestion pipeline", "No retrieval abstraction exists"
Plugin/Extensions (7 contributor traits, ExtensionRegistry, examples)	Very High	ext/extension-api/src/lib.rs with all 7 traits listed, 9 extension crates named
Interfaces/APIs (CLI, HTTP, ACP, MCP, TUI, multi-frontend proven)	Very High	CLI subcommands listed, app-server Axum, acp-server, mcp-server, codex-tui, all use ThreadManager
Scheduling/Background	High	ext/queue, QueuedItemService, ThreadLifecycleContributor::on_thread_idle, AgentRunner, spawn_subagent — scheduling explicitly missing
Security (permissions, sandboxing, secrets, local execution)	Very High	PermissionProfile, AskForApproval, SandboxPolicy, Seatbelt/Landlock/Windows implementations, keyring integration
Observability (logging, error reporting, debugging, visibility)	High	tracing, codex-otel, CodexErr, codex-diagnostics, event stream types, rollout JSONL
Testing (automated, unit, integration, reproducible)	Very High	core_test_support, TestCodexBuilder, ResponseMock, core/tests/suite/, cargo-insta, just test
Documentation (installation, architecture, extension, config, examples, source)	Medium	User docs good (install.md, quickstart.md, config.md, plugins.md, skills.md, mcp.md, acp.md, sdk.md) but architecture docs missing (docs/architecture.md missing)
Hardware/Local Model Feasibility	Medium	Inferred from Ollama-only local support; no benchmarks on target hardware
codex-core Bloat	High	Acknowledged in AGENTS.md, 150+ modules counted
# Todos
[✓] Extract OpenJarvis profile
[✓] Extract OpenHands profile
[✓] Extract Open Interpreter profile
[•] Extract Letta-ai profile
[ ] Extract crewAI profile
+ Thought: 110ms
CANDIDATE PROFILE: Letta-ai (Letta Code)
1. REPORTED FINAL RESULT
Aspect	Finding
Reported Overall Score	4.2/5 (Mature, extensible, well-documented)
Reported Recommendation	ADOPT — Highly Suitable / "PROCEED WITH ADOPTION"
Reported Strengths	1. Exceptional Architecture — clean layer separation, enforced boundaries, zero circular dependencies, grep-discoverable code<br>2. Complete Local Execution — full agent runtime with local models (Ollama, LM Studio, llama.cpp) without cloud dependency<br>3. Sophisticated Memory System — MemFS (git-backed), memory blocks, personality defaults, cross-agent isolation<br>4. Comprehensive Tool Framework — 80+ built-in tools, model-specific toolsets, MCP integration, mod tools, permission system<br>5. Multi-Interface Support — CLI (TUI/headless), WebSocket listener, App Server (OpenAI-compat), 6 messaging channels, desktop app<br>6. Extensibility — Mods (tools/skills/hooks/permissions), skills (installable), custom channels, MCP<br>7. Production Hardening — lease-based cron scheduler, WebSocket reconnection, approval recovery, compaction, queue management<br>8. Strong Engineering Practices — strict TS, biome, architectural checks, test mock isolation, file size ratchet<br>9. Active Maintenance — regular releases, CI matrix, release automation, community integrations<br>10. Permissive License — Apache-2.0 with brand exclusion only
Reported Weaknesses	1. No Native RAG/Vector Search — requires external integration or custom mod development<br>2. No Browser Automation — no Playwright/Puppeteer/web search tools in core<br>3. Limited Voice Support — only STT via OpenAI in channels, no TTS, no local voice, no streaming audio<br>4. No Sandbox/Container Isolation — code execution uses node-pty only; no gVisor/Firecracker/Docker integration<br>5. Single-Process Scheduler — lease mechanism prevents horizontal scaling of cron workers<br>6. No Structured Log Aggregation — logs to stdout/stderr only; no OpenTelemetry, Loki, etc.<br>7. Experimental Local Backend — flagged as experimental (LETTA_LOCAL_BACKEND_EXPERIMENTAL=1)<br>8. Bun/Node Runtime Split — dev uses Bun, production bundle targets Node 22.19+; behavior differences possible
2. REQUIREMENT COVERAGE (Original Terminology Preserved)
Classification	Requirements (per capability matrix)
Native	ARCH-001 (5), ARCH-002 (5), ARCH-003 (5), ARCH-004 (5), ARCH-005 (5), ARCH-006 (5), ARCH-007 (4)<br>AGENT-001 (5), AGENT-002 (5), AGENT-003 (5), AGENT-004 (5), AGENT-005 (5), AGENT-006 (5), AGENT-007 (4), AGENT-008 (4)<br>LLM-001 (5), LLM-002 (5), LLM-003 (5), LLM-004 (5), LLM-005 (5), LLM-006 (5)<br>TOOL-001 (5), TOOL-002 (5), TOOL-003 (5), TOOL-004 (4), TOOL-005 (5), TOOL-006 (5)<br>MEM-001 (5), MEM-002 (5), MEM-003 (5), MEM-004 (4), MEM-005 (0=Missing)<br>RAG-001 (0=Missing), RAG-002 (0=Missing), RAG-003 (0=Missing), RAG-004 (0=Missing), RAG-005 (0=Missing)<br>VOICE-001 (2=Integratable), VOICE-002 (0=Missing), VOICE-003 (0=Missing), VOICE-004 (0=Missing), VOICE-005 (0=Missing)<br>WEB-001 (0=Missing), WEB-002 (0=Missing), WEB-003 (0=Missing), WEB-004 (0=Missing)<br>FILE-001 (5), FILE-002 (5), FILE-003 (5), FILE-004 (5)<br>CODE-001 (5), CODE-002 (3=Modifiable), CODE-003 (5)<br>BG-001 (5), BG-002 (4), BG-003 (4), BG-004 (4)<br>SCHED-001 (5), SCHED-002 (5), SCHED-003 (4)<br>EXT-001 (5), EXT-002 (5), EXT-003 (4), EXT-004 (4)<br>API-001 (5), API-002 (5), API-003 (5), API-004 (5), API-005 (5)<br>STORE-001 (5), STORE-002 (5), STORE-003 (5), STORE-004 (4)<br>CONFIG-001 (5), CONFIG-002 (5), CONFIG-003 (5), CONFIG-004 (4)<br>SEC-001 (5), SEC-002 (5), SEC-003 (5), SEC-004 (3=Modifiable), SEC-005 (5)<br>OBS-001 (4), OBS-002 (4), OBS-003 (4), OBS-004 (4)<br>TEST-001 (5), TEST-002 (5), TEST-003 (4), TEST-004 (5)<br>DOC-001 (5), DOC-002 (4), DOC-003 (4), DOC-004 (4), DOC-005 (4), DOC-006 (5)<br>DEP-001 (5), DEP-002 (4), DEP-003 (5), DEP-004 (4)<br>PERF-001 (4), PERF-002 (5), PERF-003 (5), PERF-004 (3=Modifiable)<br>MAINT-001 (5), MAINT-002 (5), MAINT-003 (5), MAINT-004 (4), MAINT-005 (4)<br>HEALTH-001 (5), HEALTH-002 (4), HEALTH-003 (5), HEALTH-004 (4), HEALTH-005 (5)<br>LIC-001 (5), LIC-002 (5), LIC-003 (5), LIC-004 (4)
Integratable	VOICE-001 (STT via OpenAI in channels)
Modifiable	CODE-002 (execution isolation), SEC-004 (execution isolation), PERF-004 (resource awareness)
Missing	RAG-001 through RAG-005 (all 5), VOICE-002 through VOICE-005 (4 of 5), WEB-001 through WEB-004 (all 4)
Incompatible	None
Unknown	None
3. ARCHITECTURE FINDINGS
Area	Findings (Source: Phase 2-4 Audit)
Agent Runtime	Full agent lifecycle in src/agent/create.ts, src/agent/modify.ts. LocalBackend implements complete agent CRUD. APIBackend proxies to Letta API. Subagents via src/agent/subagents/manager.ts. Initialization: createAgent() → loadTools() → startConnectedListenerRuntime(). Execution: Turn lifecycle in src/websocket/listener/turn-lifecycle.ts. Tool interaction: tools/manager.ts with approval pipeline. Completion: turn-completion.ts with reflection. Error handling: turn-recovery-policy.ts + recovery.ts. Shutdown: stopRuntime() in lifecycle.ts.
Model Abstraction	Backend interface abstracts model interaction. LocalBackend uses LocalPiModelsRuntime + provider registry. APIBackend delegates to Letta Cloud API. Provider catalog in src/backend/dev/pi-provider-registry.ts. /model command switches models at runtime. resolveModel() in model-catalog.ts handles aliases. BYOK providers in src/providers/byok-providers.ts (19 cloud, 15+ local). Provider type → base provider mapping.
Tool Architecture	TOOL_DEFINITIONS in tool-definitions.ts (80+ tools). loadTools() builds registry per model/toolset. External tools via registerExternalTools() (MCP). Mod tools via getAvailableModToolsRegistry(). Model-specific toolsets (Anthropic, OpenAI, Gemini, PascalCase variants). Tool filtering via toolFilter for per-agent restrictions. /tools slash command lists available tools. Mods can register tools via tool-registry.ts. Custom channels can expose tools.
Memory	Session: Conversation messages in LocalStore (local) or API (cloud). getConversationResumeTail() retrieves recent context. Transcript projection in local-message-projection.ts. Persistent: MemFS — git-tracked memory blocks in ~/.letta/memfs/{agentId}/. memory-git.ts handles commits, pushes, pulls. LocalStore stores agents/conversations in local-store.ts. Personality default files seeded on agent creation. Replaceable: Backend interface abstracts storage — LocalBackend (file-based) vs APIBackend (cloud). LocalStore swappable. MemFS can be disabled per-agent. Retrieval: /search command searches across all messages/agents. message-search.ts in backend API. Local backend has transcript-search.ts. No semantic/vector retrieval built-in.
RAG/Retrieval	Missing entirely — explicitly outside core scope per README. No document ingestion pipeline. No chunking/preprocessing. No embedding generation. Only keyword search (/search). No retrieval abstraction. Agents can use tools to implement RAG but no infrastructure provided.
Storage	Local: LocalStore (JSON files in ~/.letta/lc-local-backend/). Cloud: Letta Cloud PostgreSQL (managed). MemFS: Git repositories for memory. Backend interface with localMemfs/remoteMemfs capabilities. Migration: transcript migration in local/transcript-migration.ts, agent hot reload, compaction format versioning. No formal schema migration framework.
Plugin/Extensions	Mods: Self-contained packages with tools, skills, hooks, permissions. Skills: Markdown + scripts, installable from GitHub/registries (ClawHub, Hermes). Channels: Adapter interface for messaging platforms (Telegram, Slack, Discord, WhatsApp, Signal, Custom). MCP: Model Context Protocol server/client support. Mods loaded from ~/.letta/mods/, .agents/mods/, agent MemFS. Skills from ~/.letta/skills/, .agents/skills/, agent MemFS. Custom channels in ~/.letta/channels/<id>/. No core modification needed. Mod adapter reloads on connection. Skill discovery on startup + file watcher. Channel adapters have start()/stop() lifecycle. MCP servers managed externally.
Interfaces	CLI: Full-featured with 30+ subcommands, interactive TUI (Ink-based), headless mode (letta -p "prompt"), JSON output. Desktop app: Tauri, shares CLI backend. Web: chat.letta.com (App Server). Mobile: chat.letta.com responsive. Messaging: Telegram, Slack, Discord, WhatsApp, Signal, Custom. All share same agent runtime and memory. Core runtime (agent/, tools/, backend/) has no UI dependencies.
APIs	Programmatic: letta.js bundle exports: app-server-client, memory-confinement, mcp-client, agent-presets, schedules, channels, gateway-core. TypeScript definitions in dist/types/. Subpath exports in package.json. WebSocket listener for real-time. App Server — OpenAI-compatible API. Letta Cloud API via @letta-ai/letta-client. Channel webhooks.
Scheduling	Cron scheduler in src/cron/scheduler.ts with lease mechanism (prevents multi-process). Standard cron expressions + timezone support. /schedule slash command. Tasks stored in crons.json (editable). One-shot (scheduled_for) and recurring (cron) tasks. Jitter support for distributed execution. /crons lists tasks with status. runCronTaskNow() for manual execution. Pause/resume via status field. GC removes completed one-shots after 7 days.
Background Processing	Cron scheduler with lease mechanism. Queue runtime per conversation (queue-runtime.ts). Background task tool (Task with background: true). Reminders system in src/reminders/. Single-process scheduler. Queue pump processes items asynchronously. Process services started per connection in listener. Cron tasks track last_run_at, last_run_outcome, failed_count, missed_count. Run logs persisted in cron-run-log.ts. Queue items have observable state. Failure handling: cron task failure tracking, queue retry logic with backoff, WebSocket reconnection with state recovery, approval recovery on reconnect.
Security	Multi-layer permission system: deny rules → CLI disallow → alwaysAsk → mode → CLI allow → working dir → session allow → settings allow → ask rules → default. Cross-agent memory guard (unbypassable). Workspace sandbox guard. Mod permission registry. Permission modes: standard, acceptEdits, unrestricted, strict. CLI flags: --allowedTools, --disallowedTools, --permission-mode, --yolo. Per-tool approval policies in TOOL_PERMISSIONS. Hooks can auto-allow/deny. BYOK system with encrypted storage (cloud) or plaintext (local with LOCAL_PROVIDER_NO_API_KEY). Secrets API in cloud (serverSecrets capability). secret-substitution.ts for shell command secrets. No hardcoded credentials. Execution isolation: node-pty process isolation. Worktree support via EnterWorktree/ExitWorktree for git isolation. No sandboxing (Docker, gVisor, firecracker).
Observability	telemetry module with events. debugLog/debugWarn with LETTA_DEBUG=1. Structured error tracking via trackBoundaryError. No structured log aggregation (stdout/stderr only). trackBoundaryError captures context. Error formatting in cli/helpers/error-formatter.ts. Channel lifecycle errors normalized via lifecycle-error.ts. No external error tracking (Sentry, etc.). LETTA_DEBUG=1 enables verbose output. Turn lifecycle states observable. Queue state inspectable. LSP integration for type checking (LETTA_ENABLE_LSP). Turn progress events via WebSocket. Channel turn progress hooks. /doctor command audits memory quality. /palace views memory.
4. HARDWARE FINDINGS
Aspect	Findings
Framework Feasibility	High — TypeScript/Bun framework. Runs on Windows/macOS/Linux (CI tests all three). Node 22.19+ for production bundle. Bun 1.3.10+ for dev. Nix flake for reproducible builds. Docker image available. Native deps: node-pty, sharp (pinned versions, platform-specific builds).
Local Large-Model Feasibility (i5-1235U, 8GB RAM, UHD, no VRAM)	Moderate — Local backend runs entirely in-process (no cloud required). Providers: Ollama, LM Studio, llama.cpp, OpenAI-compatible. LETTA_LOCAL_BACKEND_EXPERIMENTAL=1 enables local mode. Deterministic executor for testing (LETTA_LOCAL_BACKEND_EXECUTOR=deterministic). However: local backend flagged experimental. 8GB RAM may be tight for TypeScript runtime + local model + agent context. No GPU acceleration for integrated graphics. Native deps (node-pty) require compilation on Windows.
Storage	~189 GB free — ample for MemFS (git repos), LocalStore (JSON), models.
5. RISKS
Risk Category	Findings
Architectural	Local backend experimental. RAG/Voice/Browser/Sandbox require custom mod development. Bun/Node runtime split may cause behavior differences.
Maintenance	Some legacy code paths (v1/v2 permissions dual eval). Experimental features flagged with _EXPERIMENTAL env vars. Baseline file size tracking (ratchet).
Dependency	Core deps: @letta-ai/letta-client, @earendil-works/pi-ai, ink, react. Native deps: node-pty, sharp (pinned). Some deps tied to Letta ecosystem. Regular updates via Dependabot.
Scalability	Single-process scheduler (lease prevents horizontal scaling). No distributed worker support. Queue runtime per conversation.
Hardware	node-pty native module compatibility on Windows (CI tests but vendor patches may be needed). 8GB RAM borderline for full stack + local model.
Lock-in	Low — Backend interface allows swapping storage/execution. Provider abstraction allows model switching. Tool registry allows toolset replacement. Mod system allows capability addition without core changes. Channel adapter interface allows transport replacement. Letta Cloud optional (secrets, remote environments only).
Security	No container/sandbox integration for code execution. Could integrate via mods but not built-in. Cross-agent memory guard is strong. Workspace sandbox guard.
6. REUSABILITY
Category	Components
Directly Reusable	Agent runtime (local + cloud), Model abstraction (multi-provider, local models, BYOK), Tool/function calling (80+ tools, dynamic registry, MCP), Agent orchestration (subagents, multi-agent, channels), Session memory (conversation history, resume), Persistent memory infrastructure (MemFS, git, blocks), Plugin/extension mechanisms (mods, skills, channels), File tooling (read/write/search with permissions), Code execution infrastructure (shell, subagents, worktrees), Background workers (cron, queue, reminders), Scheduling (cron expressions, timezone, jitter), APIs/interfaces (CLI, WS, HTTP, channels), Configuration (hierarchical, env, secrets), Security/permissions (multi-layer, guards, hooks), Testing (unit, integration, reproducible), Deployment (local, container, npm, Nix)
Reusable with Modification	Execution sandbox (add gVisor/Firecracker/Docker via mod), Resource awareness (add CPU/RAM/GPU monitoring), Structured logging (add OpenTelemetry/Loki)
Better Replaced	None — core infrastructure is production-grade
Missing Components	RAG/Vector Search (new mod/skill needed), Voice Infrastructure (STT/TTS/streaming — significant new infrastructure), Browser Automation (Playwright mod + tools), Execution Sandbox (gVisor/Firecracker integration), Observability Upgrade (OpenTelemetry + structured logs), Distributed Scheduler (if horizontal scaling needed)
7. EVIDENCE CONFIDENCE
Finding Area	Confidence	Basis
Architecture (modular, loose coupling, separation, extension points, replaceability, understandable, dependency mgmt)	Very High	Layer map in AGENTS.md, check-layer-boundaries.js, check:cycles (madge), 1000-line limit enforced, @/ imports, Backend interface, provider registry, replaceRegistry, Mod system, ChannelAdapter interface
Agent Runtime (execution, lifecycle, tool calling, function calling, multi-step, state, orchestration, error recovery)	Very High	createAgent, loadTools, startConnectedListenerRuntime, turn-lifecycle.ts, tools/manager.ts, turn-completion.ts, turn-recovery-policy.ts, subagents/manager.ts
Model Abstraction (abstraction, provider switching, local support, streaming, config, stable interface)	Very High	Backend interface, LocalPiModelsRuntime, pi-provider-registry.ts, /model command, resolveModel, byok-providers.ts, PiStreamAdapter, model-catalog.ts
Tool Framework (registration, custom tools, discovery, validation, error handling, permissions)	Very High	TOOL_DEFINITIONS, loadTools, registerExternalTools, getAvailableModToolsRegistry, toolFilter, tool-registry.ts, validation.ts, permission system
Memory (session, persistent, replaceable backend, retrieval, vector DB)	Very High	LocalStore, MemFS, memory-git.ts, local-message-projection.ts, Backend interface, transcript-search.ts — vector DB explicitly missing
RAG/Retrieval	Very High	Explicitly stated: "RAG is explicitly outside core scope per README", "No native vector store integration", "No embedding generation"
Plugin/Extensions (Mods, Skills, Channels, MCP)	Very High	src/mods/, src/skills/builtin/, src/channels/, ModContext, ModToolDefinition, ChannelAdapter interface, file watcher for skills
Interfaces/APIs (CLI, Desktop, Web, Messaging, Programmatic)	Very High	letta.js bundle exports, dist/types/, src/index.ts (CLI), src/websocket/, Tauri desktop, 6 channel adapters
Scheduling/Background (cron, queue, reminders, failure handling)	Very High	src/cron/scheduler.ts, crons.json, queue-runtime.ts, reminders/, lease mechanism, jitter, GC
Security (permissions, secrets, tool permissions, execution isolation, local execution)	Very High	Multi-layer permission system detailed, cross-agent-memory-guard, workspace-sandbox.ts, BYOK, secret-substitution.ts, node-pty isolation
Observability (logging, error reporting, debugging, visibility)	High	telemetry module, trackBoundaryError, LETTA_DEBUG, WebSocket progress events, /doctor, /palace — but no structured log aggregation
Testing (automated, unit, integration, reproducible)	Very High	200+ test files, bun test, CI matrix (macOS, Linux x64/arm64, Windows), check:test-mock-isolation, headless scenario tests
Documentation (installation, architecture, extension, config, examples, source)	High	README install methods, AGENTS.md ownership maps, layer map, skills as examples, package.json subpath exports — but no formal architecture diagrams
Hardware/Local Model Feasibility	Medium	Local backend works with Ollama/LM Studio/llama.cpp but flagged experimental; no benchmarks on target hardware; Bun/Node split
Experimental Local Backend	High	Explicitly flagged: LETTA_LOCAL_BACKEND_EXPERIMENTAL=1
# Todos
[✓] Extract OpenJarvis profile
[✓] Extract OpenHands profile
[✓] Extract Open Interpreter profile
[✓] Extract Letta-ai profile
[•] Extract crewAI profile
+ Thought: 88ms
CANDIDATE PROFILE: crewAI
1. REPORTED FINAL RESULT
Aspect	Finding
Reported Overall Score	No single aggregate. Classification Overview: Native 61% (91), High 29% (43), Partial/Critical 10% (15), Unknown 0%, Missing 0%. All significant capabilities verified with source code evidence.
Reported Recommendation	"crewAI is a strong technical foundation for a personal AI-agent system" / "suitable as a technical foundation and would save substantial engineering effort"
Reported Strengths	1. Mature, modular architecture with clear separation of concerns<br>2. Extensive documentation and practical examples<br>3. Strong test coverage across all major components<br>4. Active maintenance with recent commits and CI/CD<br>5. Replaceable components — model providers, memory backends, tools all configurable via LiteLLM abstraction<br>6. Local execution support — fully runnable offline with local models/tools via API keys in .env<br>7. Permission and security model — tool policies, usage limits, fingerprinting<br>8. Event-driven architecture — full observability through event bus
Reported Weaknesses	1. No native HTTP API — web/REST interface requires CrewAI AMP Suite or external integration (API-003 not native)<br>2. Scheduling limitations — max_rpm is rate-based, not cron-based; no built-in task scheduler (SCHED-001/002)<br>3. No execution sandboxing — deprecated code_execution_mode; external sandbox services recommended (E2B, Modal)<br>4. No built-in migration mechanism for storage backends
2. REQUIREMENT COVERAGE (Original Terminology Preserved)
Classification	Requirements (per audit matrix)
Native	ARCH-001 through ARCH-007 (all 7)<br>AGENT-001 through AGENT-008 (all 8)<br>LLM-001 through LLM-006 (all 6)<br>TOOL-001 through TOOL-006 (all 6)<br>MEM-001 through MEM-005 (all 5)<br>RAG-001 through RAG-005 (all 5)<br>BG-001 through BG-004 (all 4)<br>SCHED-002 (Configurable Scheduling), SCHED-003 (Job Management)<br>EXT-001 through EXT-003 (3 of 4)<br>API-001, API-002, API-004, API-005 (4 of 5)<br>STORE-001 through STORE-003 (3 of 4)<br>CONFIG-001 through CONFIG-004 (all 4)<br>SEC-001 through SEC-003, SEC-005 (4 of 5)<br>OBS-001 through OBS-004 (all 4)<br>TEST-001 through TEST-004 (all 4)<br>DOC-001 through DOC-006 (all 6)
High	ARCH-007 (Dependency Management), AGENT-002, AGENT-005, AGENT-006, AGENT-007, AGENT-008, LLM-004, LLM-005, TOOL-003, TOOL-004, TOOL-005, TOOL-006, MEM-002, MEM-003, MEM-004, MEM-005, BG-002, BG-003, BG-004, SCHED-002, SCHED-003, EXT-003, API-001, API-002, API-004, API-005, STORE-001, STORE-002, STORE-003, CONFIG-001, CONFIG-002, CONFIG-003, CONFIG-004, SEC-001, SEC-002, SEC-003, OBS-001, OBS-002, OBS-003, OBS-004, TEST-002, TEST-003, DOC-002, DOC-003, DOC-004, DOC-005, DOC-006
Partial/Critical (⚓)	SCHED-001 (Scheduled Tasks), API-003 (HTTP/API Interface), STORE-004 (Migration Support), EXT-004 (Extension Lifecycle), SEC-004 (Execution Isolation)
Missing	None explicitly — audit states "No capability was marked as 'Missing'"
Incompatible / Unknown	None
Note: crewAI audit uses ✅ Native / ✅ High / ⚓ Partial / ❌ Missing terminology. "High" appears to mean "well-implemented but not necessarily the highest maturity" — maps to Native/Integratable in our framework. "Partial" = Modifiable.
3. ARCHITECTURE FINDINGS
Area	Findings (Source: Phase 2-4 Audit)
Agent Runtime	Agent class with execute_task() / aexecute_task() sync/async. Two executors: CrewAgentExecutor (deprecated, sync) and AgentExecutor (experimental, preferred) via executor_class field. ReAct loop (_invoke_loop_react) with text-based Action/Action Input. Native function calling (_invoke_loop_native_tools) when LLM supports it. Max iteration limits (max_iter default 25). Max execution time (max_execution_time). RPM limiting (max_rpm). Error recovery with retry logic (_handle_execution_error, max_retry_limit=2). Checkpoint/resume via CheckpointConfig. _times_executed, _last_messages, _mcp_resolver tracked.
Model Abstraction	LLM class (1300+ lines) wraps LiteLLM. SUPPORTED_NATIVE_PROVIDERS: 15+ (OpenAI, Anthropic, Azure, Google/Gemini, Bedrock, AWS, OpenRouter, DeepSeek, Ollama, hosted_vllm, Cerebras, Dashscope, Snowflake). Factory __new__ routes by model name or explicit provider parameter. Native provider dispatch + LiteLLM fallback. Configuration: model, provider, temperature, top_p, max_tokens, response_format, seed, api_base, streaming, timeout, reasoning_effort, thinking, context_window_size. Stable abstraction: application code uses LLM/BaseLLM, not provider details. _validate_llm_ref handles string/model instance/dict refs.
Tool Architecture	BaseTool (abstract): run()/arun(), Pydantic args_schema validation, result_schema, cache_function, max_usage_count, tool_failure_policy. @tool decorator creates Tool instances from functions with auto-generated schemas. CrewStructuredTool: enhanced with format_output_for_agent(), ainvoke(), usage tracking. Tool registration: passed to agents via tools field; parsed via parse_tools(); converted to OpenAI schema for native calling (convert_tools_to_openai_schema()). Discovery: get_tool_names(), render_text_description_and_args() generates LLM-facing description. tools_handler manages tool registry, caching, usage tracking. Pre-built tools: crewai-tools package (SerperDevTool, CalculatorTool, DreamTool, CSVLoaderTool, etc.).
Memory	UnifiedMemory in unified_memory.py. Session memory: _retrieve_memory_context() injects memory into task prompts. Persistent: LanceDB-backed vector store with LLM-enhanced encoding/recall. recall(query, limit, depth) with "shallow" (direct vector) and "deep" (LLM-driven RecallFlow) modes. remember() / remember_many() for storage. Encoding flow: LLM analyzes content, extracts entities, assigns importance, creates embeddings. Recall flow: LLM distills query, selects scopes, parallel search, confidence-based routing. Scope/slice views: memory.scope(path), memory.slice(scopes). Storage backends: LanceDB (default), Qdrant Edge, custom via MemoryConfig. Embedder configurable (OpenAI, Google, etc.) via build_embedder() factory.
RAG/Retrieval	Knowledge sources: BaseKnowledgeSource implementations with add_sources(). Document ingestion via memory.remember() stores content. Document processing: EncodingFlow with LLM analysis, chunking, embedding, importance scoring. Embeddings: configurable embedder via Memory.embedder. Retrieval: recall() with vector search. MemoryQueryCompletedEvent results. Replaceable: storage backend pluggable via Memory.storage config (LanceDB, Qdrant Edge, custom).
Storage	Through memory system with pluggable backends. Default: LanceDB (local file-based vector store). Alternatives: Qdrant Edge, custom storage implementations. MemoryConfig controls: recency/semantic/importance weights, consolidation, confidence thresholds. No direct application-level database — storage encapsulated within Memory. Migration: no built-in mechanism; storage backend switch requires config change.
Plugin/Extensions	Skills system: lib/crewai/skills/ scaffolds projects, configures agents/tasks, queries docs. Knowledge sources: knowledge_sources agent field with BaseKnowledgeSource implementations. Custom tools: developers add via tools/ directory or @tool decorator; no core modification. Flow decorators: @start, @listen, @router, or_, and_ for workflow extension. Experimental AgentExecutor: pluggable executor type via executor_class field. __init_subclass__ registry for BaseTool. LiteLLM lazy-loading with pluggable providers.
Interfaces	Programmatic: Agent(), Crew(), Flow() classes with full execution methods. Task() for task definition. CLI: crewai create crew <name> (project scaffolding), crewai install, crewai run. HTTP/API: not natively provided (enterprise AMP suite provides separately). Interface independence: core runtime not tied to one UI; CLI, programmatic, Flow APIs all use same underlying agent executor. Multiple front ends: same runtime used by CLI, programmatic, Flow; different interfaces possible.
APIs	See Interfaces above. No native HTTP service API.
Scheduling	No native cron/scheduler. max_rpm controller: RPMController enforces requests per minute; configurable via agent field. Checkpoint/resume via CheckpointConfig. list_scopes(), list_records() for job inspection.
Background Processing	ThreadPoolExecutor for memory saves (drain_writes()). Crew checkpoint/resume. Background save pool in Memory: remember_many() non-blocking, close()/drain_writes(). Memory record state visible via list_records(), info(), tree(). Memory save failures emitted as MemorySaveFailedEvent; non-blocking background saves.
Security	Permission model: tool_failure_policy (ignore/warn/raise), max_usage_count, allow_delegation, security_config fingerprinting. Secret management: load_dotenv() + .env files; API keys from environment; never hard-coded. Tool permissions: tool_failure_policy, max_usage_count, cache_function, agent-level restrictability. Execution isolation: No built-in sandboxing; deprecated code_execution_mode; external sandbox services recommended (E2B, Modal). Local execution: full local execution capability; all components run locally; model API keys optional via .env.
Observability	Logger class; event bus emissions provide structured data; verbose mode on agents. Detailed error events: AgentExecutionErrorEvent, ToolUsageErrorEvent; exception propagation. Checkpoint/resume; event emissions at every step; verbose mode; message_content_text() utility. Agent execution visibility: event bus: AgentExecutionStarted/Completed, ToolUsageStarted/Finished, memory query events.
4. HARDWARE FINDINGS
Aspect	Findings
Framework Feasibility	High — Pure Python framework. UV-based dependency management with exact pins. Runs on Windows/macOS/Linux. No heavy native dependencies. Reproducible installs via uv.lock. --block-network flag in pytest for isolated testing.
Local Large-Model Feasibility (i5-1235U, 8GB RAM, UHD, no VRAM)	Moderate — Local model support via Ollama, hosted_vllm, and other open-source runtimes through LiteLLM. Any provider in SUPPORTED_NATIVE_PROVIDERS supporting local models can be used (e.g., ollama/llama3, hosted_vllm/llama3). Model name with prefix routes to local runtime. However: LiteLLM adds abstraction overhead. 8GB RAM limits model size. No GPU acceleration path for integrated graphics. No hardware auto-detection/config optimization.
Storage	~189 GB free — ample for LanceDB/Qdrant local files, model caches, checkpoints.
5. RISKS
Risk Category	Findings
Architectural	agent/core.py ~1400 lines, llm.py ~1300 lines — large central modules. Two executor types (deprecated + experimental) indicates transition. agent/core.py handles many concerns (execution, memory, tools, skills, planning).
Maintenance	Transition from CrewAgentExecutor to AgentExecutor (experimental). Some deprecated paths (code_execution_mode). Large central classes may accumulate technical debt.
Dependency	80+ dependencies pinned with exact versions in pyproject.toml override-dependencies. Security fixes monitored via exclude-newer-package timestamps. onnxruntime<1.24 pinned for Python version compatibility. UV lockfile ensures reproducibility.
Scalability	No distributed worker support. Single-process execution. RPMController for rate limiting. Checkpoint/resume for long-running tasks. Memory background saves use ThreadPoolExecutor.
Hardware	No hardware-specific optimization. Local model performance depends entirely on LiteLLM provider integration. 8GB RAM may limit concurrent agent + model execution.
Lock-in	Low — LiteLLM abstraction enables provider switching. Memory backend configurable (LanceDB/Qdrant/custom). Tools via registration. Flow executor swappable. But: HTTP API requires enterprise suite (potential lock-in for web deployment).
Security	No execution sandboxing — major gap for untrusted code execution. External services (E2B, Modal) recommended but add dependency. Permission model exists but file-system/network isolation not built-in.
6. REUSABILITY
Category	Components
Directly Reusable	Agent runtime (ReAct + native tool calling, checkpoint/resume), Model abstraction (LiteLLM wrapper, 15+ providers, local support), Tool framework (BaseTool, @tool decorator, CrewStructuredTool, validation, caching), Memory infrastructure (UnifiedMemory, LanceDB/Qdrant, encoding/recall flows, scopes), RAG infrastructure (KnowledgeSource, EncodingFlow, embedder factory, recall), Background processing (ThreadPoolExecutor for memory, non-blocking saves), Scheduling (RPM rate limiting, checkpoint/resume), Extensions (Skills, Knowledge Sources, Custom Tools, Flow DSL, Executor swappability), Programmatic API (Agent, Crew, Flow, Task), CLI (project scaffolding, run), Configuration (Settings class, .env, pyproject.toml), Security (permission model, secret management, tool policies), Observability (event bus, structured logging, error events, checkpoint), Testing (pytest suites, mock tests, integration, reproducible), Documentation (comprehensive README, versioned docs, examples), Deployment (UV, local execution, Docker via external)
Reusable with Modification	HTTP API (needs external integration or custom server), Scheduling (needs cron scheduler for time-based tasks), Execution isolation (needs sandbox integration), Storage migration (needs migration framework)
Better Replaced	None — core components well-designed
Missing Components	Native HTTP API server, Cron/task scheduler, Execution sandbox/container integration, Database migration framework
7. EVIDENCE CONFIDENCE
Finding Area	Confidence	Basis
Architecture (modular, loose coupling, separation, extension points, replaceability, understandable, dependency mgmt)	High	Workspace structure: lib/crewai-core/, lib/crewai/, lib/crewai-tools/, lib/cli/, lib/flow/. pyproject.toml workspace config. Event bus, hooks, LiteLLM abstraction, contextvars.
Agent Runtime (execution, lifecycle, tool calling, function calling, multi-step, state, orchestration, error recovery)	High	agent/core.py:856-996 (execute_task), agents/crew_agent_executor.py:230 (invoke), _invoke_loop_react, _invoke_loop_native_tools, max_iter, max_execution_time, _handle_execution_error, CheckpointConfig
Model Abstraction (abstraction, provider switching, local support, streaming, config, stable interface)	High	llm.py:371-394 (LLM class fields), __new__ factory (lines 409-515), SUPPORTED_NATIVE_PROVIDERS (330-348), _prepare_completion_params, _validate_llm_ref, _serialize_llm_ref
Tool Framework (registration, custom tools, discovery, validation, error handling, permissions)	High	tools/base_tool.py:103-518 (BaseTool), @tool decorator, CrewStructuredTool, parse_tools, convert_tools_to_openai_schema, tool_failure_policy, max_usage_count
Memory/RAG (session, persistent, replaceable backend, retrieval, vector DB, embeddings, document ingestion)	High	unified_memory.py (recall, remember, encoding/recall flows), memory/types.py (MemoryConfig), LanceDB/Qdrant storage resolution, BaseKnowledgeSource, build_embedder
Plugin/Extensions (skills, knowledge sources, custom tools, flows, executor)	High	lib/crewai/skills/, knowledge_sources field, @tool decorator, Flow DSL decorators, executor_class field mapping
Interfaces/APIs (programmatic, CLI, HTTP, interface independence, multiple front ends)	High	Agent(), Crew(), Flow(), Task() classes; crewai CLI commands; explicit "HTTP/API not natively provided"
Scheduling/Background (scheduled tasks, configurable, job management, background jobs, workers, failure handling)	High	RPMController, max_rpm, CheckpointConfig, ThreadPoolExecutor for memory, drain_writes, MemorySaveFailedEvent — but no cron scheduler
Security (permissions, secrets, tool permissions, execution isolation, local execution)	High	tool_failure_policy, max_usage_count, allow_delegation, security_config, load_dotenv(), .env, explicit "no built-in sandboxing"
Observability (logging, error reporting, debugging, visibility)	High	Logger class, crewai_event_bus events (AgentExecutionStarted, ToolUsageStarted, etc.), checkpoint/resume, verbose mode
Testing (automated, unit, integration, reproducible)	High	pytest suites in lib/crewai/tests/, lib/crewai-tools/tests/, lib/crewai-core/tests/, --block-network, --timeout, --dist=loadfile
Documentation (installation, architecture, extension, config, examples, source)	High	Comprehensive README.md, docs/edge/en/, versioned docs/v1.X.Y/, crewai-examples repo, quick tutorials
Hardware/Local Model Feasibility	Medium	Inferred from LiteLLM provider list; no benchmarks on target hardware; no hardware detection
No HTTP API / No Sandboxing / No Cron Scheduler	Very High	Explicitly stated in audit with evidence references

PHASE 3 — CLASSIFICATION NORMALIZATION
1. NORMALIZATION METHODOLOGY
Principle: Map each candidate's reported findings to the authoritative 6-class system using implementation evidence from the audit reports, not terminology alone.
Process per Requirement per Candidate:
1. Locate the audit's evidence description for the requirement
2. Assess the evidence against authoritative definitions:
- Native: Source code shows direct, usable implementation in the repository
- Integratable: Clean integration path documented (MCP, protocol, config) but not built-in
- Modifiable: Partial code exists but requires meaningful engineering changes to meet requirement
- Missing: No evidence of implementation or integration path
- Incompatible: Implementation exists but architecture prevents meeting our requirement
- Unknown: Evidence contradictory, absent, or insufficient to determine
3. Record original terminology, normalized status, reasoning, and evidence confidence
Key Distinctions Applied:
- "Supported via MCP/external package" → Integratable (if clean integration documented)
- "Possible via custom code/mods" → Modifiable (requires engineering effort)
- "Quality score 3/5 (Basic/Usable)" → Modifiable if evidence shows partial implementation
- "Quality score 4-5/5 (Mature)" + source evidence → Native
- "Not in core, but skills/mods can add" → Modifiable (requires custom development)
- "Delegated to server/external" → Integratable if protocol exists, Modifiable if custom integration needed
- Audit says "Native" but evidence shows gaps → downgrade to Modifiable or Integratable
- Audit says "High" (crewAI) → map based on evidence detail, not label
2. TERMINOLOGY MAPPING TABLE
Candidate	Original Terminology	Maps to Authoritative	Mapping Rule
OpenJarvis	Native	Native	Direct match — audit uses authoritative terms
 	Unknown	Unknown	Direct match
 	(Quality 3/5)	Modifiable	"Basic implementation" or "Improvements needed" = partial
OpenHands	5/5 Mature	Native	Full implementation with source evidence
 	4/5 Mature	Native	Strong implementation, minor gaps
 	3/5 Basic	Modifiable	"Basic implementation" = partial, needs work
 	(No explicit Missing)	—	All requirements have some implementation
Open Interpreter	Native	Native	Direct match — uses authoritative terms
 	Integratable	Integratable	Direct match
 	Modifiable	Modifiable	Direct match
 	Missing	Missing	Direct match
Letta-ai	Native	Native	Direct match — uses authoritative terms
 	Integratable	Integratable	Direct match
 	Modifiable	Modifiable	Direct match
 	Missing	Missing	Direct match
crewAI	✅ Native	Native	Direct match for "Native" label
 	✅ High	Native/Modifiable	Map by evidence: if source shows full impl → Native; if partial → Modifiable
 	⚓ Partial/Critical	Modifiable	"Partial" = meaningful changes required
 	❌ Missing	Missing	Direct match (though audit claims none missing)
3. CANDIDATE NORMALIZATION TABLES
3.1 OpenJarvis Normalization
Requirement	Original Finding	Original Term	Normalized Status	Reason	Evidence Confidence
ARCH-001 to ARCH-007	Native (Quality 4-5)	Native	Native	Source code shows registry pattern, clear modules, loose coupling	High
AGENT-001 to AGENT-008	Native (Quality 3-4)	Native	Native	AgentExecutor, BaseAgent, 15+ agents with full lifecycle	High
LLM-001 to LLM-006	Native (Quality 3-5)	Native	Native	12+ engine backends, InferenceEngine ABC, hardware-aware config	High
TOOL-001 to TOOL-006	Native (Quality 3-5)	Native	Native	50+ tools, ToolExecutor, MCP adapter, registry-based	High
MEM-001 to MEM-005	Native (Quality 4)	Native	Native	MemoryService, FactStore, 6 backends, SQLite persistence	High
RAG-001 to RAG-005	Native (Quality 3-4)	Native	Native	7 RAG backends (hybrid, FAISS, ColBERT, BM25, etc.), ingestion pipeline	High
EXT-001 to EXT-004	Native (Quality 3-4)	Native	Native	18 registries, skills (Hermes/OpenClaw), MCP, A2A	High
API-001 to API-005	Native (Quality 3-4)	Native	Native	CLI, FastAPI, WebSocket, SDK, 15 channels, Desktop GUI	High
STORE-001 to STORE-004	Native (Quality 3-4)	Native	Native	SQLite, vector stores, migration support in store.py	High
CONFIG-001 to CONFIG-004	Native (Quality 3-4)	Native	Native	Central config, env vars, secret management, env-specific	High
SEC-001 to SEC-005	Native (Quality 3-5)	Native	Native	Capability policy, taint tracking, injection scanner, Docker sandbox, local exec	High
TEST-001 to TEST-004	Native (Quality 3-4)	Native	Native	668 test files, CI with coverage gates, Windows+Linux	High
DOC-001 to DOC-006	Native (Quality 3-4)	Native	Native	Installation, architecture, API, config, examples, source understandability	High
DEP-001 to DEP-004	Native (Quality 4-5)	Native	Native	Docker, systemd, launchd, desktop installers, Render	High
HEALTH-001, 003, 005	Native (Quality 4)	Native	Native	Recent commits, git tags, clear roadmap	High
HEALTH-002, 004	Unknown	Unknown	Unknown	Explicitly "Not checked"	High
LIC-001 to LIC-003	Native (Quality 5)	Native	Native	Apache 2.0, permissive, commercial compatible	High
LIC-004	Unknown	Unknown	Unknown	Explicitly "Not checked"	High
Notes: OpenJarvis audit already uses authoritative terminology. Quality 3 items (e.g., AGENT-002, TOOL-004, SEC-001) still classified as Native because evidence shows usable implementation with minor improvements needed — meets "Native" threshold per requirements doc (Score 3 = Usable implementation).
3.2 OpenHands Normalization
Requirement	Original Finding	Original Term	Normalized Status	Reason	Evidence Confidence
ARCH-001	4/5 Mature	Mature	Native	Clear module boundaries: api/, hooks/, components/, stores/, utils/	High
ARCH-002	4/5 Mature	Mature	Native	Backend registry, typescript-client, centralized client options	High
ARCH-003	5/5 Mature	Mature	Native	Clear separation: agent execution, model interaction, tools, memory, storage, interfaces, config, background	High
ARCH-004	4/5 Mature	Mature	Native	Skills via @openhands/extensions SKILLS_CATALOG, buildBundledSkills()	High
ARCH-005	4/5 Mature	Mature	Native	Model providers via LLM profiles, memory backend via registry, tools via enable_sub_agents	High
ARCH-006	4/5 Mature	Mature	Native	Execution starts at root.tsx → App; communication patterns clear	High
ARCH-007	3/5 Basic	Basic	Modifiable	Exact pins but 70+ deps, 50+ overrides for vulns — needs dependency upgrades	High
AGENT-001	5/5 Mature	Mature	Native	buildStartConversationRequest(), ServerClient provides full API	High
AGENT-002	4/5 Mature	Mature	Native	Full lifecycle: init → execution → tool interaction → completion → error handling → shutdown	High
AGENT-003	5/5 Mature	Mature	Native	getAgentTools() registers default tools; client_tools payload	High
AGENT-004	5/5 Mature	Mature	Native	Native function calling via agent-server SDK; llm.stream=true	High
AGENT-005	5/5 Mature	Mature	Native	Core loop: LLM → tool → observation → next turn; max_iterations=500	High
AGENT-006	4/5 Mature	Mature	Native	execution_status, conversation_metadata (localStorage), event store, Zustand stores	High
AGENT-007	4/5 Mature	Mature	Native	Automation backend, prebuilt automations, skills, parent-child conversations	High
AGENT-008	4/5 Mature	Mature	Native	Confirmation policies, security analyzer, stuck detection, backend health probes	High
LLM-001	4/5 Mature	Mature	Native	DEFAULT_SETTINGS.llm_model; llm config with model/provider/api_key/base_url	High
LLM-002	4/5 Mature	Mature	Native	LLM profiles, ACP provider switching, resolveEffectiveAcpModel()	High
LLM-003	4/5 Mature	Mature	Native	llm.base_url for local endpoints; litellm integration; ACP local support	High
LLM-004	5/5 Mature	Mature	Native	llm.stream=true always; agent-server streams via WebSocket	High
LLM-005	5/5 Mature	Mature	Native	Comprehensive: model, provider, api_key, base_url, stream, auth_type, subscription_vendor	High
LLM-006	4/5 Mature	Mature	Native	toAppConversation() maps wire→app; ConversationClient hides provider details	High
TOOL-001	5/5 Mature	Mature	Native	agent_settings.tools + client_tools; getAgentTools() iterates DEFAULT + configured	High
TOOL-002	4/5 Mature	Mature	Native	Skills via @openhands/extensions SKILLS_CATALOG; agent_settings.tools config	High
TOOL-003	4/5 Mature	Mature	Native	shouldIncludeTool() filters; use-has-permission.ts; skill trigger keywords	High
TOOL-004	3/5 Basic	Basic	Modifiable	Basic gating only; parameter schema validation delegated to agent-server SDK	High
TOOL-005	4/5 Mature	Mature	Native	isAgentServerToolAvailable(); confirmation policies; backend health probes	High
TOOL-006	4/5 Mature	Mature	Native	enable_sub_agents gate; shouldIncludeTool() checks browser enablement	High
MEM-001	5/5 Mature	Mature	Native	useConversationHistory fetches 50 events; WebSocket resend_mode='since'	High
MEM-002	4/5 Mature	Mature	Native	localStorage metadata; active_profile tracks LLM profile across sessions	High
MEM-003	4/5 Mature	Mature	Native	Backend registry pattern; getEffectiveLocalBackend() fallback	High
MEM-004	5/5 Mature	Mature	Native	EventService.searchEvents(); paginated loading; WebSocket resend_mode='since'	High
MEM-005	4/5 Mature	Mature	Integratable	Embedding provider configs exist; skills may use vectors; but no native vector DB in core	High
BG-001	4/5 Mature	Mature	Native	Automation backend for scheduled execution; prebuilt webhook automations	High
BG-002	4/5 Mature	Mature	Native	Automation backend workers; goal store; task polling	High
BG-003	3/5 Basic	Basic	Modifiable	execution_status tracks progress; limited UI exposure for job state	High
BG-004	4/5 Mature	Mature	Native	MAX_CONSECUTIVE_FAILURES cap; recordBackendFailure/Success; reprobe	High
SCHED-001	4/5 Mature	Mature	Native	Automation backend with webhook/cron; prebuilt automations	High
SCHED-002	4/5 Mature	Mature	Native	Scheduling config via UI; no code modification; env vars for version	High
SCHED-003	3/5 Basic	Basic	Modifiable	Limited inspection/management UI; use-automation-detail.ts exists but basic	High
EXT-001	4/5 Mature	Mature	Native	@openhands/extensions SKILLS_CATALOG; buildBundledSkills(); build-time loading	High
EXT-002	4/5 Mature	Mature	Native	Skills addable via catalog; user/project skills separate	High
EXT-003	4/5 Mature	Mature	Native	Skills have defined triggers/content/description; bounded context	High
EXT-004	3/5 Basic	Basic	Modifiable	Build-time loading only; no runtime load/shutdown mechanism documented	High
API-001	5/5 Mature	Mature	Native	@openhands/typescript-client with ConversationClient, FileClient, ServerClient	High
API-002	4/5 Mature	Mature	Native	bin/agent-canvas.mjs; npm run dev; agent-canvas CLI command	High
API-003	5/5 Mature	Mature	Native	Full REST API via agent-server: /api/settings, /conversations, /events, /git/, /automation/	High
API-004	5/5 Critical	Critical	Native	Frontend decoupled from backend via typescript-client; multiple backends switchable	High
API-005	4/5 Mature	Mature	Native	Vite dev, static build, Electron, Docker; same backend via typescript-client	High
STORE-001	4/5 Mature	Mature	Native	Conversation metadata in localStorage; agent-server persists in DB	High
STORE-002	4/5 Mature	Mature	Native	Backend registry abstracts local/cloud/Docker; getEffectiveLocalBackend() fallback	High
STORE-003	5/5 Mature	Mature	Native	localStorage for metadata, skills, telemetry, workspaces	High
STORE-004	3/5 Basic	Basic	Modifiable	Some migration (v2→flat→misc_settings); frontend transformApiResponse; docs needed	High
CONFIG-001	5/5 Mature	Mature	Native	config/defaults.json single source; read by scripts, Docker, CI	High
CONFIG-002	5/5 Mature	Mature	Native	.env.sample; VITE_SESSION_API_KEY, VITE_ENABLE_BROWSER_TOOLS, etc.	High
CONFIG-003	4/5 Mature	Mature	Native	SecretsService via agent-server API; Fernet-encrypted; never hard-coded	High
CONFIG-004	4/5 Mature	Mature	Native	config/defaults.json has dev/prod distinctions; npm run dev vs build	High
SEC-001	4/5 Mature	Mature	Native	shouldIncludeTool() gating; enable_sub_agents; use-has-permission.ts; backend health	High
SEC-002	4/5 Mature	Mature	Native	SecretsService with retry; Fernet-encrypted; cloud vs local path	High
SEC-003	4/5 Mature	Mature	Native	Same as TOOL-006; enable_sub_agents gate; shouldIncludeTool() checks	High
SEC-004	4/5 Mature	Mature	Integratable	VITE_ENABLE_BROWSER_TOOLS gating; sandbox mode; runtime_services block — but no native OS sandbox	High
SEC-005	5/5 Critical	Critical	Native	Default local mode; OH_SECRET_KEY; session key auto-gen; ~/.openhands/ persistence	High
OBS-001	4/5 Mature	Mature	Native	winston + daily-rotate-file; error-handler.ts; event-logger.ts	High
OBS-002	4/5 Mature	Mature	Native	user-facing-error.ts; isCorsOrNetworkError; controlled error messages	High
OBS-003	4/5 Mature	Mature	Native	use-backends-health.ts; conversation-local-storage.ts; getStoredConversationMetadata()	High
OBS-004	3/5 Basic	Basic	Modifiable	execution_status, sandbox_status visible; limited runtime services block	High
TEST-001 to 004	5/5 Mature	Mature	Native	200+ test files; Vitest, MSW, Playwright; mock-LLM E2E; reproducible	High
DOC-001 to 006	4-5/5 Mature	Mature	Native	README (3 install options), SELF_HOSTING.md, architecture.md, AGENTS.md, examples/	High
DEP-001 to 004	4-5/5 Mature	Mature	Native	Source, Docker, Electron, static; config/defaults.json documents versions	High
PERF-001	3/5 Basic	Basic	Modifiable	uvx install; first-run ~50MB pull; 30-90s cold start	High
PERF-002	5/5 Mature	Mature	Native	Full async/await; WebSocket events; React Query; non-blocking UI	High
PERF-003	5/5 Mature	Mature	Native	llm.stream=true; token-level streaming via WebSocket; telemetry capture	High
PERF-004	4/5 Mature	Mature	Native	config/defaults.json documents versions, port allocations	High
MAINT-001	5/5 Critical	Critical	Native	src/ structure: api/, hooks/, components/, stores/, utils/, services/	High
MAINT-002	4/5 High	High	Native	Modules focused: single purpose per hook (verified by file names)	High
MAINT-003	4/5 Critical	Critical	Native	typescript-client clients; query keys; AgentServerUIRoot scoping	High
MAINT-004	4/5 Critical	Critical	Native	Known overrides for vulns; check-translation-completeness; make-i18n regenerates	High
MAINT-005	4/5 High	High	Native	Exact pins; overrides for transitive vulns; version from defaults.json	High
HEALTH-001	5/5 Critical	Critical	Native	Weekly CI runs; frequent commits; mock-llm-e2e.yml; docker.yml	High
HEALTH-002	4/5 High	High	Native	GitHub Projects; labeled issues; PR template; pr-artifacts.yml; release-please	High
HEALTH-003	5/5 High	High	Native	CHANGELOG.md; 1.16.0 latest; release-please auto-tags; semver	High
HEALTH-004	4/5 Medium	Medium	Native	Slack community; contributing guide; many contributors	High
HEALTH-005	5/5 Critical	Critical	Native	Clear multi-repo boundary (AGENTS.md); focused scope	High
LIC-001	5/5 Critical	Critical	Native	MIT License in LICENSE file	High
LIC-002	5/5 Critical	Critical	Native	MIT permits modification, merging, publishing, distributing, sublicensing	High
LIC-003	4/5 High	High	Native	MIT commercially compatible; dependency licenses checked via overrides	High
LIC-004	4/5 High	High	Native	70+ deps exact-pinned; overrides for transitive vulns; audit cycle	High
Notes: OpenHands uses 1-5 scoring. Mapped 5/5 and 4/5 with "Mature" evidence → Native. 3/5 "Basic" → Modifiable (partial implementation needing work). MEM-005 and SEC-004 mapped to Integratable because integration path exists (embedding configs, sandbox mode) but not native implementation.
3.3 Open Interpreter (codex-rs) Normalization
Requirement	Original Finding	Original Term	Normalized Status	Reason	Evidence Confidence
ARCH-001 to ARCH-007	Native (4-5)	Native	Native	80+ crates, trait boundaries, extension contributor pattern, Cargo workspace	Very High
AGENT-001 to AGENT-008	Native (4-5)	Native	Native	ThreadManager→CodexThread→Session→Agent; full lifecycle, tool calling, function calling, multi-step, state, orchestration (sub-agents), error recovery	Very High
LLM-001	Native (5)	Native	Native	ModelProvider trait, ModelClient session-scoped	Very High
LLM-002	Native (5)	Native	Native	/model command, config profile, factory function, 4+ providers	Very High
LLM-003	Integratable (3)	Integratable	Integratable	Ollama via OpenAI-compat only; no native llama.cpp/vllm — requires external server	Very High
LLM-004	Native (5)	Native	Native	SSE + WebSocket, prewarm, realtime audio streaming	Very High
LLM-005	Native (5)	Native	Native	Config.toml with model, provider, reasoning, temperature, verbosity, profiles	Very High
LLM-006	Native (5)	Native	Native	ModelClient/ModelClientSession/ResponseEvent — never provider-specific types	Very High
TOOL-001 to TOOL-006	Native (4-5)	Native	Native	ToolRegistry, ToolExecutor, DynamicTool, JSON Schema validation, PermissionProfile	Very High
MEM-001	Native (5)	Native	Native	Session holds conversation, RolloutRecorder, InitialHistory, 10K token cap	Very High
MEM-002	Native (4)	Native	Native	JSONL.zst + SQLite index, fork/resume, compaction	Very High
MEM-003	Modifiable (2)	Modifiable	Modifiable	ThreadStore trait exists but rollout storage hardcoded to file+SQLite	Very High
MEM-004	Native (3)	Native	Native	RolloutRecorder::read_session, ThreadStore::read_thread — chronological/fork retrieval	High
MEM-005	Missing (0)	Missing	Missing	No vector DB integration, no embeddings in core	Very High
RAG-001	Missing (0)	Missing	Missing	No document ingestion pipeline	Very High
RAG-002	Missing (0)	Missing	Missing	No chunking/preprocessing	Very High
RAG-003	Integratable (2)	Integratable	Integratable	Server-side only via OpenAI memories/trace_summarize (requires cloud)	Very High
RAG-004	Modifiable (1)	Modifiable	Modifiable	Keyword search only (BM25); would need Retriever trait + vector store	Very High
RAG-005	Missing (0)	Missing	Missing	No retrieval abstraction exists	Very High
VOICE-001	Integratable (2)	Integratable	Integratable	OpenAI Realtime API only; no local STT	Very High
VOICE-002	Missing (0)	Missing	Missing	No TTS implementation	Very High
VOICE-003	Integratable (3)	Integratable	Integratable	WebSocket realtime streaming works with OpenAI Realtime	Very High
VOICE-004	Modifiable (2)	Modifiable	Modifiable	Tightly coupled to OpenAI Realtime wire format; no VoiceProvider trait	Very High
VOICE-005	Missing (0)	Missing	Missing	No wake-word/VAD/push-to-talk	Very High
WEB-001	Integratable (3)	Integratable	Integratable	ext/web-search extension calls search API; skills reference agent-browser	Very High
WEB-002	Integratable (2)	Integratable	Integratable	Referenced in README; browser_use config exists; no native Playwright/Puppeteer	Very High
WEB-003	Integratable (3)	Integratable	Integratable	WebSearch tool via ext/web-search; provider search APIs; shell can curl	Very High
WEB-004	Native (4)	Native	Native	Tools extensible; DynamicTool allows runtime definition; MCP can expose browser tools	Very High
FILE-001 to FILE-004	Native (4-5)	Native	Native	file-system crate (read/write), apply-patch, shell, file-search (BM25), PermissionProfile with readable/writable roots, sandbox enforcement	Very High
CODE-001 to CODE-003	Native (5)	Native	Native	shell-command, unified_exec with PTY, apply-patch, exec-server; Seatbelt/Landlock/Windows sandboxes; AskForApproval, NetworkApproval, ExecPolicy	Very High
BG-001	Native (4)	Native	Native	ext/queue QueuedItemService; ThreadLifecycleContributor::on_thread_idle; AgentRunner spawn_subagent	Very High
BG-002	Native (4)	Native	Native	QueuedItemService dispatch_lock; AgentRunner; app-server in_process worker	Very High
BG-003	Modifiable (2)	Modifiable	Modifiable	Queue items persisted but no generic job state machine with inspection API	Very High
BG-004	Native (3)	Native	Native	Queue discards invalid; sub-agent errors logged; TurnAbortedEvent; analytics retry queue	High
SCHED-001	Missing (0)	Missing	Missing	No cron/scheduler in core; KimiCron internal only	Very High
SCHED-002	Missing (0)	Missing	Missing	N/A — no scheduling	Very High
SCHED-003	Missing (0)	Missing	Missing	N/A — no scheduling	Very High
EXT-001 to EXT-004	Native (4-5)	Native	Native	ExtensionRegistry + 7 contributor traits; 9 extensions; ExtensionDataInit; lifecycle hooks	Very High
API-001 to API-005	Native (5)	Native	Native	ThreadManager/CodexThread/Session public API; JSON-RPC v2 WS/HTTP; ACP; MCP; TUI; Exec SDK — all share core	Very High
STORE-001	Native (5)	Native	Native	SQLite + JSONL.zst rollouts, thread-store	Very High
STORE-002	Modifiable (3)	Modifiable	Modifiable	ThreadStore trait but RolloutRecorder concrete file-based	Very High
STORE-003	Native (5)	Native	Native	~/.openinterpreter default; fully local	Very High
STORE-004	Native (4)	Native	Native	SQLx migrations embedded; rollout_tracing; RolloutMigration	Very High
CONFIG-001 to CONFIG-004	Native (4-5)	Native	Native	Layered TOML, profiles, env vars, keyring secrets, cloud config layers	Very High
SEC-001 to SEC-005	Native (5)	Native	Native	PermissionProfile, keyring, ApprovalsReviewer, best-in-class sandboxing, fully local by default	Very High
OBS-001 to OBS-004	Native (4)	Native	Native	tracing + OpenTelemetry; CodexErr; codex-diagnostics; event stream; rollout JSONL	Very High
TEST-001 to TEST-004	Native (4-5)	Native	Native	core_test_support, TestCodexBuilder, ResponseMock, core/tests/suite/, cargo-insta, just test	Very High
DOC-001	Native (5)	Native	Native	install.md, quickstart.md, README one-liner	Very High
DOC-002	Modifiable (2)	Modifiable	Modifiable	AGENTS.md has conventions but no architecture overview; docs/architecture.md missing	Very High
DOC-003	Native (4)	Native	Native	plugins.md, skills.md, mcp.md, acp.md, sdk.md, generated TS schemas	Very High
DOC-004	Native (5)	Native	Native	config.md, config-reference.md (generated), just write-config-schema	Very High
DOC-005	Native (4)	Native	Native	use-cases.md, workflows.md, getting-started.md, skills examples	Very High
DOC-006	Native (4)	Native	Native	Well-structured, typed, doc comments, AGENTS.md conventions, clippy/rustfmt	Very High
DEP-001 to DEP-004	Native (4-5)	Native	Native	One-liner install, Cargo/Rust, Bazel, .devcontainer, single binary, local deployment, platform docs	Very High
PERF-001	Native (4)	Native	Native	Single binary; TUI ~100-200ms; model prewarm optional	Very High
PERF-002	Native (5)	Native	Native	Fully async Tokio; streaming throughout	Very High
PERF-003	Native (5)	Native	Native	Model streaming, tool output streaming, realtime audio, rollout streaming	Very High
PERF-004	Modifiable (3)	Modifiable	Modifiable	No explicit CPU/RAM/GPU limits; sandbox can limit; token budgets 10K cap	Very High
MAINT-001	Native (4)	Native	Native	80+ crates; core large but AGENTS.md acknowledges; new features go to ext/	Very High
MAINT-002	Native (4)	Native	Native	Most crates single-purpose; core violates (acknowledged)	Very High
MAINT-003	Native (5)	Native	Native	Traits for all major boundaries: ModelProvider, ThreadStore, ToolExecutor, etc.	Very High
MAINT-004	Modifiable (3)	Modifiable	Modifiable	codex-core bloat acknowledged; legacy code; active refactoring but large core remains risk	Very High
MAINT-005	Native (5)	Native	Native	Workspace version pinning; cargo-deny; MODULE.bazel.lock; regular updates	Very High
HEALTH-001 to HEALTH-005	Native (4-5)	Native	Native	20+ GH Actions; daily commits; regular releases; Discord; clear roadmap	Very High
LIC-001 to LIC-004	Native (4-5)	Native	Native	Apache-2.0; permits modification; commercial-friendly; cargo-deny checks licenses	Very High
Notes: Open Interpreter already uses authoritative terminology. Normalization confirms their classifications with evidence confidence ratings.
3.4 Letta-ai Normalization
Requirement	Original Finding	Original Term	Normalized Status	Reason	Evidence Confidence
ARCH-001 to ARCH-007	Native (4-5)	Native	Native	Layer map enforced, 0 cycles, 1000-line limit, Backend/provider/tool abstractions	Very High
AGENT-001 to AGENT-008	Native (4-5)	Native	Native	Full lifecycle, local+cloud backend, subagents, turn lifecycle, recovery policies	Very High
LLM-001 to LLM-006	Native (5)	Native	Native	Backend interface, provider catalog, local models (Ollama/LM Studio/llama.cpp), BYOK, streaming, stable interface	Very High
TOOL-001 to TOOL-006	Native (4-5)	Native	Native	80+ tools, dynamic registry, MCP, mod tools, multi-layer permissions, validation	Very High
MEM-001 to MEM-004	Native (4-5)	Native	Native	Conversation messages, MemFS (git-backed), LocalStore, transcript search, Backend abstraction	Very High
MEM-005	Missing (0)	Missing	Missing	No native vector store integration	Very High
RAG-001 to RAG-005	Missing (0)	Missing	Missing	Explicitly outside core scope; no ingestion, chunking, embeddings, retrieval, abstraction	Very High
VOICE-001	Integratable (2)	Integratable	Integratable	Channel transcription via OpenAI only; no local STT	Very High
VOICE-002 to VOICE-005	Missing (0)	Missing	Missing	No TTS, no streaming audio, no provider abstraction, no activation	Very High
WEB-001 to WEB-004	Missing (0)	Missing	Missing	No HTTP client tools, no Playwright/Puppeteer, no web search/fetch, no browser extensibility in core	Very High
FILE-001 to FILE-004	Native (5)	Native	Native	Read/Write/Edit/MultiEdit/Glob/Grep/ListDir tools; permissions restrict to working dir + additionalDirectories; cross-agent memory guard; workspace sandbox guard	Very High
CODE-001	Native (5)	Native	Native	Bash/shell/exec_command/RunShellCommand; Task/Agent tool spawns subagents; node-pty	Very High
CODE-002	Modifiable (3)	Modifiable	Modifiable	node-pty process isolation only; no sandboxing (Docker, gVisor, firecracker); worktree support	Very High
CODE-003	Native (5)	Native	Native	Permission modes (standard/acceptEdits/unrestricted/strict); CLI flags; per-tool policies; hooks	Very High
BG-001 to BG-004	Native (4-5)	Native	Native	Cron scheduler with lease; queue runtime; background Task tool; reminders; failure tracking; retry logic	Very High
SCHED-001 to SCHED-003	Native (4-5)	Native	Native	Cron with expressions/timezone/jitter; /schedule command; crons.json; manual run; pause/resume; GC	Very High
EXT-001 to EXT-004	Native (4-5)	Native	Native	Mods (tools/skills/hooks/permissions), Skills (installable), Channels (6 adapters), MCP; file watcher; start/stop lifecycle	Very High
API-001 to API-005	Native (5)	Native	Native	letta.js bundle exports; CLI (TUI/headless); WS listener; App Server (OpenAI-compat); 6 channels; desktop Tauri; all share backend	Very High
STORE-001 to STORE-004	Native (4-5)	Native	Native	LocalStore (JSON), Cloud PostgreSQL, MemFS (git); Backend interface; transcript migration; no formal migration framework	Very High
CONFIG-001 to CONFIG-004	Native (4-5)	Native	Native	settingsManager, ~/.letta/, .agents/, MemFS agent-scoped; env vars; BYOK encrypted; project/global separation	Very High
SEC-001 to SEC-003	Native (5)	Native	Native	Multi-layer permissions, cross-agent guard, workspace sandbox, BYOK, secret substitution, per-tool policies	Very High
SEC-004	Modifiable (3)	Modifiable	Modifiable	node-pty isolation; worktree isolation; no container/sandbox integration	Very High
SEC-005	Native (5)	Native	Native	Full local backend with local models; no cloud required for core features	Very High
OBS-001 to OBS-004	Native (4)	Native	Native	telemetry events, debugLog, trackBoundaryError, LETTA_DEBUG, turn progress events, /doctor, /palace — but no structured log aggregation	High
TEST-001 to TEST-004	Native (4-5)	Native	Native	200+ tests, CI matrix (macOS/Linux/Windows), mock isolation, headless scenario tests, Docker integration	Very High
DOC-001 to DOC-006	Native (4-5)	Native	Native	README install, AGENTS.md ownership maps, layer map, skills as examples, strict TS conventions	High
DEP-001 to DEP-004	Native (4-5)	Native	Native	bun install --frozen-lockfile, bun run build → standalone letta.js, npm pack, Nix flake, Docker, local backend, CI platforms	Very High
PERF-001	Native (4)	Native	Native	Lazy loads React/Ink; parallel init; tool loading gated; milestone timing	High
PERF-002	Native (5)	Native	Native	Fully async Bun/Node; queue runtime; streaming; non-blocking tools	Very High
PERF-003	Native (5)	Native	Native	Model streaming, tool output streaming, channel draft streaming, WS progress	Very High
PERF-004	Modifiable (3)	Modifiable	Modifiable	Context window management, token estimation; no explicit CPU/RAM/GPU monitoring; bundle size ~50MB	High
MAINT-001 to MAINT-005	Native (4-5)	Native	Native	Layered arch, 1000-line limit, named exports, clear interfaces (Backend, ChannelAdapter, ToolDefinition), baseline ratchet, experimental flags, stable deps	Very High
HEALTH-001 to HEALTH-005	Native (4-5)	Native	Native	CI on every PR, recent releases, release cascade, issue/PR templates, Discord, ClawHub/Hermes, clear roadmap	Very High
LIC-001 to LIC-004	Native (4-5)	Native	Native	Apache-2.0, modification rights, commercial compatibility, deps MIT/Apache/BSD (pi-ai needs verify)	High
Notes: Letta-ai already uses authoritative terminology. Normalization confirms classifications. MEM-005, RAG all, VOICE-002-005, WEB all correctly classified as Missing/Integratable per evidence.
3.5 crewAI Normalization
Requirement	Original Finding	Original Term	Normalized Status	Reason	Evidence Confidence
ARCH-001 to ARCH-007	Native / High	Native/High	Native	Workspace modules (crewai-core, crewai, crewai-tools, cli, flow); event bus, hooks, LiteLLM abstraction, contextvars; UV dependency mgmt with exact pins	High
AGENT-001	Native	Native	Native	Agent.execute_task/aexecute_task; CrewAgentExecutor + AgentExecutor; full execution runtime	High
AGENT-002	High	High	Native	post_init_setup (LLM, executor, skills, planning); execute_task; error handling; retry; cleanup; checkpoint	High
AGENT-003	Native	Native	Native	Native function calling + ReAct fallback; parse_tools; convert_tools_to_openai_schema	High
AGENT-004	High	High	Native	Structured function calling via LiteLLM tools parameter; OpenAI schema generation	High
AGENT-005	High	High	Native	ReAct loop with max_iter=25; timeout; retry limit; RPM limiting; state via messages list	High
AGENT-006	High	High	Native	_times_executed, _last_messages, _mcp_resolver, iterations, RPM state, execution UUID, checkpoint	High
AGENT-007	Native	Native	Native	Crew.kickoff() (sequential/hierarchical); Flow.kickoff() (event-driven); experimental AgentExecutor	High
AGENT-008	High	High	Native	_check_execution_error; _handle_execution_error (sync/async); OutputParserError handling; event emissions	High
LLM-001	Native	Native	Native	LLM class wraps LiteLLM; all config via LLM fields; factory routes to native providers	High
LLM-002	Native	Native	Native	15+ providers in SUPPORTED_NATIVE_PROVIDERS; model name prefix or explicit provider param	High
LLM-003	Native	Native	Native	Ollama, hosted_vllm via LiteLLM; model prefix routes to local runtime	High
LLM-004	High	High	Native	_handle_streaming_response; stream param; LLMStreamChunkEvent via event bus	High
LLM-005	High	High	Native	All common params: temperature, top_p, max_tokens, response_format, seed, api_base, stream, reasoning_effort, context_window	High
LLM-006	Native	Native	Native	LLM/BaseLLM stable abstraction; _validate_llm_ref handles string/instance/dict refs	High
TOOL-001	Native	Native	Native	Agent.tools field; parse_tools; BaseTool.init_subclass registry	High
TOOL-002	Native	Native	Native	@tool decorator; Tool.from_langchain; CrewStructuredTool.from_function; no core modification	High
TOOL-003	High	High	Native	get_tool_names(); render_text_description_and_args(); tools_handler manages registry	High
TOOL-004	High	High	Native	Pydantic args_schema validation via _validate_kwargs(); Tool._validate_kwargs()	High
TOOL-005	High	High	Native	tool_failure_policy (ignore/warn/raise); ToolFailure class; tool_failure_collector	High
TOOL-006	High	High	Native	max_usage_count; cache_function; tool_failure_policy; agent-level permission control	High
MEM-001	Native	Native	Native	_retrieve_memory_context() injects memory; recall()/remember()	High
MEM-002	High	High	Native	LanceDB-backed vector store; remember() persists; recall() retrieves across sessions	High
MEM-003	High	High	Native	storage field configures LanceDB/Qdrant Edge/custom; MemoryConfig pluggable	High
MEM-004	High	High	Native	recall(query, limit, depth) shallow/deep; MemoryQueryStarted/Completed events	High
MEM-005	High	High	Native	LanceDB default; Qdrant Edge alternative; embedder configurable (OpenAI, Google, etc.)	High
RAG-001	High	High	Native	Knowledge sources (BaseKnowledgeSource) with add_sources(); memory remember() stores content	High
RAG-002	Medium	Medium	Modifiable	EncodingFlow with LLM analysis, chunking, embedding, importance scoring — but "Medium" quality	High
RAG-003	High	High	Native	Configurable embedder via Memory.embedder; build_embedder() factory	High
RAG-004	High	High	Native	recall() with vector search; MemoryQueryCompletedEvent results	High
RAG-005	High	High	Native	Storage backend pluggable via Memory.storage config	High
BG-001	Native	Native	Native	ThreadPoolExecutor for memory saves; drain_writes(); crew checkpoint/resume	High
BG-002	High	High	Native	Background save pool in Memory; remember_many() non-blocking; close()/drain_writes()	High
BG-003	Medium	Medium	Modifiable	Memory record state via list_records(), info(), tree() — "Medium" quality	High
BG-004	High	High	Native	MemorySaveFailedEvent; non-blocking background saves	High
SCHED-001	Partial	Partial	Missing	"No native cron/scheduler; max_rpm is rate-based, not cron-based" — no time-based scheduling	High
SCHED-002	High	High	Modifiable	max_rpm configurable via agent field; but not cron scheduling — rate limiting only	High
SCHED-003	Medium	Medium	Modifiable	Checkpoint/resume via CheckpointConfig; list_scopes()/list_records() for inspection	High
EXT-001	Native	Native	Native	Skills system; knowledge sources; custom tools; Flow decorators; executor_class swappability	High
EXT-002	Native	Native	Native	@tool decorator; new BaseTool subclasses; BaseKnowledgeSource; no core modification	High
EXT-003	High	High	Native	Defined interfaces: BaseTool, BaseKnowledgeSource, BaseKnowledgeStorage; event bus contracts	High
EXT-004	Medium	Medium	Modifiable	Skills have disclosure levels; memory close()/drain; checkpoint save/restore — "Medium" quality	High
API-001	High	High	Native	Agent(), Crew(), Flow(), Task() classes with full execution methods	High
API-002	Medium	Medium	Native	crewai create crew, install, run fully documented	High
API-003	Missing	Missing	Missing	"No native HTTP service API (enterprise AMP suite provides this separately)"	High
API-004	Native	Native	Native	Core runtime not tied to one UI; CLI, programmatic, Flow APIs all work	High
API-005	High	High	Native	Same runtime used by CLI, programmatic, Flow; different interfaces possible	High
STORE-001	High	High	Native	Through memory system with LanceDB; data persists across runs	High
STORE-002	High	High	Native	Memory.storage field configures backend; pluggable storage interface	High
STORE-003	High	High	Native	LanceDB local file-based storage by default; path parameter controls location	High
STORE-004	Medium	Medium	Modifiable	No built-in migration mechanism; storage backend switch requires config change	High
CONFIG-001	High	High	Native	Settings class in crewai_core; pyproject.toml; .env file support	High
CONFIG-002	High	High	Native	.env file supported; env vars for API keys, model config	High
CONFIG-003	Native	Native	Native	API keys via .env/environment variables; never hard-coded; load_dotenv() in llm.py	High
CONFIG-004	Medium	Medium	Native	.env files differentiate dev/prod; uv workspace configuration	High
SEC-001	Native	Native	Native	tool_failure_policy; max_usage_count; allow_delegation; security_config fingerprinting	High
SEC-002	Native	Native	Native	load_dotenv() + .env files; API keys from environment; never hard-coded	High
SEC-003	High	High	Native	tool_failure_policy; max_usage_count; cache_function; agent-level restrictability	High
SEC-004	High	High	Missing	"No built-in sandboxing; deprecated code_execution_mode; external sandbox services recommended (E2B, Modal)" — no isolation mechanism in framework	High
SEC-005	Native	Native	Native	Full local execution capability; all components run locally; model API keys optional via .env	High
OBS-001	High	High	Native	Logger class; event bus emissions provide structured data; verbose mode	High
OBS-002	High	High	Native	Detailed error events (AgentExecutionErrorEvent, ToolUsageErrorEvent); exception propagation	High
OBS-003	High	High	Native	Checkpoint/resume; event emissions at every step; verbose mode; message_content_text()	High
OBS-004	Medium	Medium	Native	Event bus: AgentExecutionStarted/Completed; ToolUsageStarted/Finished; memory query events	High
TEST-001 to TEST-004	Native/High	Native/High	Native	pytest suites in lib/crewai/tests/, lib/crewai-tools/tests/, lib/crewai-core/tests/; --block-network; --timeout; --dist=loadfile	High
DOC-001 to DOC-006	Native/High	Native/High	Native	Comprehensive README; docs/edge/en/ versioned; crewai-examples repo; quick tutorials; clear module boundaries	High
DEP-001	(Not explicitly scored)	—	Native	UV-based dependency management; reproducible installs; pyproject.toml workspace	High
DEP-002	(Not explicitly scored)	—	Integratable	No native container support documented; would need custom Dockerfile	Medium
DEP-003	(Implied by SEC-005)	—	Native	Full local execution capability	High
DEP-004	(Not explicitly scored)	—	Native	UV/pyproject.toml; runs on Windows/macOS/Linux	High
PERF-001 to PERF-004	(Not explicitly scored)	—	Unknown	No explicit performance audit in report	Low
MAINT-001 to MAINT-005	(Not explicitly scored)	—	Unknown	No explicit maintainability audit in report	Low
HEALTH-001 to HEALTH-005	(Not explicitly scored)	—	Unknown	No explicit health audit in report (though "active maintenance with recent commits and CI/CD" noted)	Low
LIC-001 to LIC-004	(Not explicitly scored)	—	Unknown	No explicit licensing audit in report	Low
Notes: crewAI audit uses different terminology (Native/High/Partial/Missing). Mapped "Native" and "High" with strong evidence → Native. "Medium" → Modifiable (partial implementation). "Partial" for SCHED-001 → Missing because evidence explicitly states "no native cron/scheduler." SEC-004 "High" but evidence says "no built-in sandboxing" → Missing. PERF, MAINT, HEALTH, LIC not explicitly audited → Unknown.
4. REQUIREMENTS THAT COULD NOT BE NORMALIZED RELIABLY
Requirement	Candidates Affected	Issue
PERF-001 to PERF-004	crewAI	No performance audit in crewAI report — cannot normalize
MAINT-001 to MAINT-005	crewAI	No maintainability audit in crewAI report — cannot normalize
HEALTH-001 to HEALTH-005	crewAI	No repository health audit in crewAI report — only brief mention
LIC-001 to LIC-004	crewAI	No licensing audit in crewAI report — cannot normalize
DEP-002	crewAI	No container support documentation — inferred as Integratable (Medium confidence)
VOICE-001 to VOICE-005	OpenHands, crewAI	OpenHands: voice inferred from frontend only; crewAI: not audited
WEB-001 to WEB-004	crewAI	Not explicitly audited — likely Missing but not confirmed
RAG-001 to RAG-005	OpenHands	Covered under Memory vector DB but not as separate RAG category — inferred
5. REQUIREMENTS REQUIRING FURTHER EVIDENCE
Requirement	Candidates Needing Clarification	Specific Question
MEM-005 (Vector DB Integration)	OpenHands	Is vector DB integration native (built-in) or only via embedding provider configs + skills?
SEC-004 (Execution Isolation)	OpenHands, Letta-ai, crewAI	OpenHands: "sandbox mode" — what does it isolate? Letta-ai: only node-pty? crewAI: explicitly missing?
RAG-001 to RAG-005	OpenHands	Does the skill system provide actual RAG infrastructure or just tool access?
SCHED-001 to SCHED-003	Open Interpreter	Confirmed Missing — but could extension system add it? (EXT-001 is Native)
VOICE-001 to VOICE-005	OpenHands, Letta-ai	OpenHands: only STT via channels? Letta-ai: only OpenAI STT in channels?
BG-003 (Job State)	OpenHands, Open Interpreter	Both Modifiable — what specific job state inspection is missing?
STORE-004 (Migration Support)	OpenHands, Letta-ai, crewAI	All Modifiable — what migration framework is needed vs. config change?
PERF-004 (Resource Awareness)	All	OpenJarvis: hardware detection; OpenInterpreter: token budgets; Letta: context window; others unknown