# Phase 7 OpenJarvis Codebase Integration Map

## Part 1 — Repository Inventory

The OpenJarvis repository at `C:\Ai_Agent_Research\candidate\Openjarvis` has the following structure:

```
openjarvis/
├── .github/
├── assets/
├── configs/
├── deploy/
├── desktop/
├── docs/
├── examples/
├── frontend/
├── rust/
├── scripts/
├── src/
│   └── openjarvis/
│       ├── a2a/
│       ├── agents/
│       ├── analytics/
│       ├── bench/
│       ├── channels/
│       ├── cli/
│       ├── connectors/
│       ├── core/
│       ├── daemon/
│       ├── engine/
│       ├── evals/
│       ├── intelligence/
│       ├── learning/
│       ├── mcp/
│       ├── memory/
│       ├── mining/
│       ├── operators/
│       ├── prompt/
│       ├── recipes/
│       ├── sandbox/
│       ├── scheduler/
│       ├── security/
│       ├── server/
│       ├── sessions/
│       ├── skills/
│       ├── speech/
│       ├── system/
│       ├── telemetry/
│       ├── templates/
│       ├── tools/
│       ├── traces/
│       ├── workflow/
│       ├── sdk.py
│       ├── _rust_bridge.py
│       └── __init__.py
├── tests/
├── .gitignore
├── .pre-commit-config.yaml
├── CHANGELOG.md
├── CODE_OF_CONDUCT.md
├── CONTRIBUTING.md
├── LICENSE
├── Makefile
├── mkdocs.yml
├── Open_Source_AI_Foundation_Requirements.md
├── pyproject.toml
├── README.md
├── render.yaml
├── REVIEW.md
└── uv.lock
```

**Key directories:**
- `src/openjarvis/core`: Core primitives (config, registry, events, paths, types)
- `src/openjarvis/engine`: Inference engine abstractions and implementations
- `src/openjarvis/memory`: Automatic long-term memory storage (facts)
- `src/openjarvis/connectors`: Data source connectors (RSS, email, calendars, etc.)
- `src/openjarvis/tools`: Tool implementations (web search, file I/O, shell execution, etc.)
- `src/openjarvis/agents`: Built-in agent implementations (morning_digest, deep_research, monitor_operative, etc.)
- `src/openjarvis/server`: FastAPI routes for external access
- `src/openjarvis/scheduler`: Background task scheduler
- `src/openjarvis/intelligence`: Model catalog and intelligence-related utilities

## Part 2 — Technology Stack

From the repository, we can determine:

- **Programming language**: Python (>=3.10) as indicated in README and pyproject.toml
- **Framework**: 
  - FastAPI for HTTP API (server directory)
  - Standard library for event bus, threading, etc.
  - Uses `httpx` for HTTP requests (seen in news_rss connector)
  - Uses `sqlite3` for persistent storage (digest store, telemetry, traces, etc.)
  - Uses `uv` for dependency management (as per install instructions)
- **Package manager**: `uv` (as seen in install instructions and uv.lock)
- **Dependency management**: `pyproject.toml` defines dependencies
- **Database**: SQLite for various stores (digest, telemetry, traces, sessions, agent manager, etc.)
- **Vector storage**: Not explicitly present in the core codebase; retrieval uses SQLite with full-text search or simple matching (as seen in memory store for facts)
- **Configuration mechanism**: TOML file (`config.toml`) with Pydantic-like dataclasses (see core/config.py)
- **Test framework**: `pytest` (as seen in CONTRIBUTING.md and pyproject.toml)
- **Async framework**: Uses `asyncio` (seen in engine stubs and server code)
- **HTTP framework**: FastAPI (server directory)
- **CLI framework**: Custom CLI built with standard library (see scripts and cli directory)
- **Plugin/registry mechanism**: Decorator-based registry system (see core/registry.py)
- **Model provider abstraction**: Engine interface in engine/_stubs.py with concrete implementations in engine/ (ollama.py, vllm.py, etc.)
- **Background job mechanism**: TaskScheduler in scheduler/scheduler.py with background polling thread
- **Scheduler**: Scheduler in scheduler/scheduler.py supporting cron, interval, and once schedules
- **Logging/telemetry**: 
  - Logging via standard library `logging` module
  - Telemetry via SQLite storage (see server/savings.py and telemetry config)
  - Structured event bus for inter-component communication (core/events.py)

## Part 3 — Actual OpenJarvis Architecture

We reconstruct the architecture from the source code:

### Application Entry Points
- **CLI Entry Point**: The `jarvis` command is installed via uv and points to the SDK (`sdk.py`) which provides the main entry point.
- **SDK Entry Point**: `openjarvis.sdk.Jarvis` context manager (see sdk.py) initializes the system.
- **Server Entry Point**: `openjarvis.server.app` creates the FastAPI application.

### Agent Runtime
- **Agent Lifecycle**: Managed by `openjarvis.agents.manager.AgentManager` (see agents/manager.py) which handles agent instantiation, execution, and lifecycle.
- **Execution Loop**: Agents inherit from `openjarvis.agents._stubs.BaseAgent` and implement the `run` method. The executor (`agents/executor.py`) handles the agent turn loop.
- **Model Abstraction**: Via `openjarvis.engine._stubs.InferenceEngine` interface, with concrete implementations in `engine/` directory.
- **Tool Execution**: Tools are registered via `openjarvis.tools._stubs.BaseTool` and resolved by `openjarvis.agents.tool_resolver.ToolResolver`.
- **Memory / Context**: Automatic memory service (`openjarvis.memory.service.MemoryService`) extracts facts and stores them via `openjarvis.memory.store.LocalFactStore`. Context injection is handled by agents via `context_from_memory` config.
- **Response Generation**: Agents use the engine to generate responses, optionally streaming.

### Web Access
- **HTTP Fetching**: Through `openjarvis.tools.http_request.HTTPRequestTool` and `openjarvis.tools.web_search.WebSearchTool`.
- **Browser Automation**: Via `openjarvis.tools.browser.BrowserTool` (Playwright-based).
- **RSS Feeds**: Via `openjarvis.connectors.news_rss.NewsRSSConnector` (see Part 11).

### Storage
- **Fact Storage**: JSONL-based `LocalFactStore` (memory/store.py) for automatic long-term memory facts.
- **Relational Storage**: SQLite for:
  - Digest artifacts (`agents/digest_store.py`)
  - Telemetry (`server/savings.py`)
  - Traces (`server/savings.py` - shares telemetry DB or separate)
  - Sessions (`server/session_store.py`)
  - Agent manager (`server/savings.py` or dedicated)
  - Scheduler (`scheduler/store.py`)
  - Configuration is stored in TOML file, not database.
- **Blob Storage**: Not explicitly present; attachments are stored via channels (e.g., email attachments).

### Background Processing
- **Workers**: The `TaskScheduler` runs a background daemon thread that polls for due tasks.
- **Queues**: Not used; the scheduler scans the store for due tasks each poll interval.
- **Async Execution**: Engine calls can be async (see `InferenceEngine.stream`), but most agent processing is synchronous per turn.
- **Task Lifecycle**: Tasks are created via `TaskScheduler.create_task`, executed in the poll loop, and logged to the store.
- **Retries**: Not built into the scheduler; individual tools or engines may implement retries.
- **Persistence**: Task state is persisted in SQLite via `SchedulerStore`.

### Scheduling
- **Scheduler Implementation**: `TaskScheduler` in `scheduler/scheduler.py`.
- **Scheduling API**: `create_task`, `list_tasks`, `pause_task`, `resume_task`, `cancel_task`.
- **Recurring Jobs**: Supported via cron and interval schedule types.
- **Persistence**: Task state stored in SQLite via `SchedulerStore`.
- **Startup Behavior**: Scheduler must be started explicitly via `start()`; not auto-started by default.
- **Timezone Handling**: Uses `datetime` with timezone awareness (see scheduler's `_now_iso` and cron computation).
- **Failure Handling**: Exceptions in task execution are caught and logged; the task is marked as failed in the run log but remains active for future runs unless cancelled.

### APIs
- **HTTP API**: FastAPI-based REST API under `/api/` prefix (see server/ directory).
  - Agent execution: `/api/agent` (see agent_manager_routes.py)
  - Digest: `/api/digest` (see digest_routes.py)
  - Connectors: `/api/connectors` (see connectors_router.py)
  - Models: `/api/models` (see models_routes.py)
  - Telemetry: `/api/telemetry` (see analytics_routes.py or savings.py)
  - Traces: `/api/traces` (see traces_routes.py)
  - etc.
- **WebSocket**: Not present in the core codebase.
- **CLI**: The `jarvis` command provides subcommands for configuration, agent execution, skill management, etc.
- **SDK**: Python SDK (`openjarvis.sdk`) for programmatic access.
- **Channels**: Messaging channels (email, Slack, Telegram, etc.) for sending/receiving messages.

### Security
- **Sandboxing**: 
  - SSRF protection in connectors (see news_rss connector's use of `openjarvis.security.ssrf`).
  - Tool execution sandboxing via `openjarvis.sandbox` (see sandbox directory).
  - Container sandboxing configured via `SandboxConfig`.
- **Permissions**: 
  - Tool confirmation enforced via `SecurityConfig.enforce_tool_confirmation`.
  - Capability-based access control via `CapabilitiesConfig` (not enabled by default).
- **Capability Policies**: Defined in `SecurityConfig.capabilities`.
- **Prompt-Injection Defenses**: 
  - Input scanning via `SecurityConfig.scan_input`.
  - Output scanning via `SecurityConfig.scan_output`.
  - Secret and PII scanning via `secret_scanner` and `pii_scanner`.
- **SSRF Protection**: Explicit in connectors (see news_rss connector) and configurable via `SecurityConfig.ssrf_protection`.
- **Authentication**: 
  - API server authentication via `auth_middleware.py` (supports API keys, etc.).
  - OAuth for connectors (see oauth.py).
- **Authorization**: Not fully implemented; relies on authentication and tool confirmation.
- **Audit Logs**: 
  - Security audit log via `SecurityConfig.audit_log_path` (see security directory).
  - Immutable audit trail via merkle trees if `SecurityConfig.merkle_audit` enabled.

### Observability
- **Logging**: Standard library `logging` used throughout; configured via `logging.config` in application startup.
- **Traces**: 
  - Distributed tracing not explicitly present; however, `Trace` and `TraceStep` types exist in core/types.py for agent traces.
  - Traces can be stored via `TracesConfig` and `Traces` table in SQLite.
- **Telemetry**: 
  - Inference telemetry stored via `TelemetryConfig` and `TelemetryRecord` in SQLite (see server/savings.py).
  - Metrics include latency, token usage, cost, energy, etc.
- **Metrics**: 
  - No explicit metrics export (e.g., Prometheus) in core; telemetry is stored locally.
  - However, the `MetricsConfig` in learning section suggests potential for learning metrics.
- **Events**: 
  - Event bus (`core/events.py`) for inter-component communication.
  - Events include inference start/end, tool calls, memory operations, agent turns, scheduler tasks, etc.
- **Error Reporting**: Exceptions are logged and sometimes returned to users (e.g., in API routes).

### Extension/Registry Mechanisms
- **Registries**: 
  - `ModelRegistry` (core/registry.py) for model specifications.
  - `EngineRegistry` for inference engine backends.
  - `MemoryRegistry` for memory/retrieval backends.
  - `FactStoreRegistry` for automatic memory fact stores.
  - `AgentRegistry` for agent implementations.
  - `ToolRegistry` for tool specifications.
  - `RouterPolicyRegistry` for router policies.
  - `BenchmarkRegistry` for benchmarks.
  - `ChannelRegistry` for channel implementations.
  - `LearningRegistry` for learning policies.
  - `SkillRegistry` for skill manifests.
  - `SpeechRegistry` for speech backends.
  - `CompressionRegistry` for context compression strategies.
  - `TTSRegistry` for text-to-speech backends.
  - `ConnectorRegistry` for data source connectors.
  - `MinerRegistry` for Pearl mining providers.
- **Registration**: 
  - Components are registered via decorators (e.g., `@ModelRegistry.register("model_id")`).
  - Imperative registration via `register_value` method.
  - Resolution via `get(key)` or `create(key, *args, **kwargs)`.
  - Discovery via `items()`, `keys()`, `contains(key)`.
- **Plugin Loading**: 
  - Plugins are loaded by importing modules that contain registrations.
  - Skills are loaded from the `skills` directory or external sources via `SkillsConfig`.
  - Connectors are loaded from the `connectors` directory.
  - Engines are loaded from the `engine` directory.
  - Agents are loaded from the `agents` directory.
  - Tools are loaded from the `tools` directory.

## Part 4 — OpenJarvis Registry / Extension System

The registry system is implemented in `src/openjarvis/core/registry.py`.

**Key components:**
- `RegistryBase[T]`: Generic base class for registries.
- Typed subclass registries for each component type:
  - `ModelRegistry`: For `ModelSpec` objects.
  - `EngineRegistry`: For `InferenceEngine` subclasses.
  - `MemoryRegistry`: For `MemoryBackend` subclasses.
  - `FactStoreRegistry`: For `FactStore` subclasses.
  - `AgentRegistry`: For `BaseAgent` subclasses.
  - `ToolRegistry`: For tool specifications.
  - `RouterPolicyRegistry`: For router policy implementations.
  - `BenchmarkRegistry`: For benchmark implementations.
  - `ChannelRegistry`: For channel implementations.
  - `LearningRegistry`: For learning policies.
  - `SkillRegistry`: For skill manifests.
  - `SpeechRegistry`: For speech backend implementations.
  - `CompressionRegistry`: For context compression strategies.
  - `TTSRegistry`: For text-to-speech backend implementations.
  - `ConnectorRegistry`: For data source connector implementations.
  - `MinerRegistry`: For Pearl mining provider implementations.

**How registration works:**
- Decorator: `@RegistrySubclass.register("key")` registers a class or instance under the given key.
- Imperative: `RegistrySubclass.register_value("key", value)` registers a value directly.
- Resolution: `RegistrySubclass.get("key")` retrieves the registered item.
- Instantiation: `RegistrySubclass.create("key", *args, **kwargs)` looks up the key and calls it with arguments.
- Introspection: `RegistrySubclass.items()` returns all (key, item) pairs; `contains(key)` checks existence.

**How a new component would be added:**
1. For a new model: Create a `ModelSpec` instance and register it with `ModelRegistry.register_value("model_id", spec)` or ensure it's picked up via auto-discovery.
2. For a new engine: Implement a subclass of `InferenceEngine`, then register it with `@EngineRegistry.register("engine_name")` on the class.
3. For a new tool: Implement a subclass of `BaseTool`, then register it with `@ToolRegistry.register("tool_name")` on the class.
4. For a new connector: Implement a subclass of `BaseConnector`, then register it with `@ConnectorRegistry.register("connector_name")` on the class.
5. For a new agent: Implement a subclass of `BaseAgent`, then register it with `@AgentRegistry.register("agent_name")` on the class.
6. For a new memory backend: Implement a subclass of `MemoryBackend`, then register it with `@MemoryRegistry.register("backend_name")` on the class.
7. For a new fact store: Implement a subclass of `FactStore`, then register it with `@FactStoreRegistry.register("store_name")` on the class.

## Part 5 — Agent Runtime

**Execution Path:**
1. **User/System Input**: 
   - CLI: User runs `jarvis ask "query"` or `jarvis` for interactive chat.
   - API: POST to `/api/agent` with `{ "prompt": "...", "agent": "orchestrator", ... }`.
   - Channel: Incoming message via a channel connector (e.g., email, Slack).
   - Schedule: Scheduler triggers a task with a prompt.
2. **Agent**: 
   - The `JarvisSystem` (or `Jarvis` context manager) resolves the agent type from config or input.
   - The agent is instantiated via `AgentRegistry.create(agent_name, ...)`.
   - The agent's `run` method is called (see `agents/manager.py` and `agents/executor.py`).
3. **Model Selection**: 
   - The agent may select a model based on routing (if learning enabled) or use default.
   - The model is specified as a string ID (e.g., "qwen3:8b").
4. **Tool Selection**: 
   - The agent determines which tools to use (based on reasoning, ReAct loop, etc.).
   - Tools are resolved via `ToolResolver` from `openjarvis.agents.tool_resolver`.
5. **Tool Execution**: 
   - Each tool's `execute` method is called with arguments.
   - Tools may perform web access, file operations, computations, etc.
   - Tool results are returned to the agent.
6. **Memory / Context**: 
   - If `context_from_memory` is enabled, the agent's memory service retrieves relevant facts.
   - Facts are injected into the prompt as context.
7. **Response**: 
   - The agent uses the inference engine to generate a response (via `engine.generate` or `engine.stream`).
   - The response is returned to the user/system.

**Background Awareness Pipeline Support:**
- The runtime **can** support a background awareness pipeline because:
  - The scheduler (`scheduler/scheduler.py`) can run periodic tasks without user interaction.
  - Tasks can execute arbitrary prompts via the `JarvisSystem.ask` method (as seen in scheduler's `_execute_task`).
  - The system is designed for long-running processes (see daemon directory).
  - However, the scheduler executes tasks as isolated agent turns; there is no persistent background agent state by default.
  - For a continuous awareness pipeline, one would need to either:
    - Use the `monitor_operative` or `operative` agent types which are designed for long-horizon monitoring with state management.
    - Or create a custom agent that runs in a loop and uses the scheduler for periodic wake-ups.

**Evidence:**
- `src/openjarvis/scheduler/scheduler.py`: Shows the background polling thread and task execution mechanism.
- `src/openjarvis/agents/monitor_operative.py`: Implements a continuous agent with state.
- `src/openjarvis/agents/operative.py`: Implements a persistent autonomous agent.
- `src/openjarvis/server/agent_manager_routes.py`: Shows how agents are invoked via API.
- `src/openjarvis/sdk.py`: Shows the `Jarvis` context manager for system access.

## Part 6 — Model / LLM Abstraction

**Model Interface:**
- Defined in `src/openjarvis/engine/_stubs.py` as `InferenceEngine` abstract base class.

**Provider Interface:**
- Concrete implementations inherit from `InferenceEngine`:
  - `engine/ollama.py`: `OllamaEngine`
  - `engine/vllm.py`: `VllmEngine`
  - `engine/sglang.py`: `SglangEngine`
  - `engine/mlx.py`: `MlxEngine`
  - `engine/litellm.py`: `LiteLiMEngine` (for OpenAI-compatible APIs)
  - `engine/cloud.py`: `CloudEngine` (for proprietary cloud APIs)
  - `engine/apple_fm.py`: `AppleFMEngine` (for Apple Foundation Models)
  - `engine/gemma_cpp.py`: `GemmaCppEngine`
  - `engine/nexa_shim.py`: `NexaEngine`
  - `engine/nim.py`: `NimEngine`
  - `engine/_apple_fm_support.py`: `AppleFMSupport` (support for AFM)

**Model Selection:**
- Model is specified by string ID (e.g., "qwen3:8b") in the `generate`/`stream` calls.
- The engine's `can_serve(model_id)` method determines if the engine can serve the model (default returns True).
- The `ModelRegistry` provides `ModelSpec` metadata for known models.

**Streaming:**
- Supported via `InferenceEngine.stream` (yields token strings) and `stream_full` (yields `StreamChunk` with tool calls, finish reason, etc.).

**Structured Output:**
- Supported via `ResponseFormat` dataclass (see `_stubs.py`) with `type` ("json_object" or "json_schema") and optional `schema`.

**Tool Calling:**
- Engines that support native tool calling should override `stream_full` to yield tool call chunks.
- The base `stream_full` wraps `stream` and yields a final chunk with `finish_reason="stop"`.

**Retries and Errors:**
- Retries are not built into the engine interface; individual implementations may add retries.
- Errors propagate as exceptions; specific error types like `EngineConnectionError` and `EngineContextLengthError` are defined in `engine/_base.py`.

**Configuration:**
- Engine-specific configuration is provided via the config file (see `core/config.py` `EngineConfig` and engine-specific sections like `engine.ollama`).

**Local/Cloud Models:**
- The `InferenceEngine` interface does not distinguish local vs cloud; the `is_cloud` flag on the engine instance indicates if it's a cloud engine.
- Cloud engines (like `CloudEngine`) require API keys and make remote API calls.
- Local engines run models locally (via Ollama, vLLM, etc.).

**Different Models for Different Tasks:**
- The architecture allows different models to be used for different tasks because:
  - Each `generate`/`stream` call specifies the model ID.
  - An agent could use one model for reasoning and another for extraction (though current agents typically use a single model per turn).
  - The routing system (if learning enabled) could select different models based on query characteristics.
  - However, there is no built-in mechanism to use multiple models within a single agent turn without custom agent logic.

**Evidence:**
- `src/openjarvis/engine/_stubs.py`: Defines `InferenceEngine` ABC.
- `src/openjarvis/engine/ollama.py`: Example concrete implementation.
- `src/openjarvis/core/types.py`: Defines `ModelSpec` for model metadata.
- `src/openjarvis/core/registry.py`: Shows `ModelRegistry` for model spec registration.
- `src/openjarvis/server/savings.py`: Shows telemetry recording of model usage.

## Part 7 — Tool Architecture

**Tool Interface:**
- Defined in `src/openjarvis/tools/_stubs.py` as `BaseTool` abstract base class (inherits from `BaseComponent`).

**Tool Registration:**
- Tools are registered via `ToolRegistry` (see core/registry.py).
- Decorator: `@ToolRegistry.register("tool_name")` on a tool class.
- Imperative: `ToolRegistry.register_value("tool_name", tool_instance)`.

**Tool Execution:**
- Tools are resolved by `openjarvis.agents.tool_resolver.ToolResolver`.
- The agent's executor calls `tool.execute(**kwargs)` with arguments provided by the agent.
- Tools return a string result (or raise an exception).

**Tool Permissions:**
- Tools do not have built-in permissions; permission is enforced by:
  - `SecurityConfig.enforce_tool_confirmation`: If true, the user must confirm tool execution.
  - The agent's reasoning loop may decide not to call a tool based on safety considerations.
  - Some tools may have internal checks (e.g., file tools checking paths).

**Tool Metadata:**
- Tools can have metadata via the `metadata` field inherited from `BaseComponent`.
- However, the `BaseTool` class in `_stubs.py` does not define specific metadata fields.

**Tool Observability:**
- Tool execution is observable via:
  - The event bus: `TOOL_CALL_START` and `TOOL_CALL_END` events are published (see core/events.py).
  - Telemetry: Tool usage, latency, and cost are recorded in teleonomy (see server/savings.py).
  - Logging: Tools may log internally.

**MCP (Model Context Protocol):**
- Not explicitly present in the tool system; however, the `mcp` directory exists and `MCPConfig` is in core/config.py.
- MCP appears to be for connecting to external MCP servers, not for tool implementation.

**External Tool Integration:**
- External tools can be integrated by implementing a `BaseTool` subclass and registering it with the `ToolRegistry`.
- Examples of external tool integration are not visible in the core codebase, but the mechanism is in place.

**Awareness Components as Tools/Services/etc.:**
Awareness components should **not** be forced into the tool abstraction because:
- Tools are designed for discrete, single-purpose operations with string inputs/outputs.
- Awareness pipelines involve continuous state, complex data models (events, entities), and multi-stage processing.
- Instead, awareness components should be implemented as:
  - **Services**: Long-lived objects that maintain state and provide methods for processing.
  - **Pipeline Components**: Stages in a processing pipeline that transform data incrementally.
  - **Agents**: If the awareness pipeline needs to reason and use tools, it could be implemented as a custom agent type.
  - **Background Workers**: For periodic polling and processing, using the scheduler or a custom daemon.

**Appropriate Architectural Boundary:**
Based on the OpenJarvis codebase, awareness components should be implemented as:
1. **Foundation Extension**: New registry types for awareness-specific components (e.g., `EventRegistry`, `EntityRegistry`, `AwarenessMemoryRegistry`) if they need to be pluggable.
2. **Custom Services**: Classes that encapsulate awareness logic (e.g., `AwarenessPipeline`, `EventDetector`, `RelevanceEngine`).
3. **Integration Layer**: Adapters that use existing foundation tools (web search, HTTP fetching, etc.) and services (memory, engine) via their public APIs.
4. **Not as Tools**: Unless a specific awareness function is a discrete operation (e.g., "extract events from text"), which could be a tool, but the overall pipeline is too complex for the tool abstraction.

**Evidence:**
- `src/openjarvis/tools/_stubs.py`: Defines `BaseTool` ABC.
- `src/openjarvis/agents/tool_resolver.py`: Shows how tools are resolved and used by agents.
- `src/openjarvis/server/connectors_router.py`: Shows how connectors are used via API (similar pattern could apply to awareness services).
- `src/openjarvis/core/registry.py`: Shows the registry pattern for pluggable components.

## Part 8 — Memory System

**Memory Interfaces:**
- Defined in `src/openjarvis/memory/store.py` as `FactStore` ABC for automatic long-term memory facts.
- The `MemoryService` in `src/openjarvis/memory/service.py` uses the fact store.

**Memory Services:**
- `MemoryService`: Background service that extracts facts from conversations and stores them via the fact store.
- `MemoryManager`: Not present as a separate service; memory is accessed directly via the store or through the memory service.

**Storage:**
- **Fact Storage**: `LocalFactStore` (JSONL file) is the only built-in implementation, registered with `FactStoreRegistry` under "local".
- **Other Storage**: 
  - SQLite is used for:
    - Digest artifacts (`agents/digest_store.py`)
    - Telemetry (`server/savings.py`)
    - Traces (`server/savings.py` or separate table)
    - Sessions (`server/session_store.py`)
    - Agent manager (likely in `server/savings.py` or dedicated)
    - Scheduler (`scheduler/store.py`)
    - Configuration is in TOML file.

**Fact Extraction:**
- Performed by the `MemoryService` using a configurable extraction model (see `MemoryConfig.extraction_model`).
- The extractor is in `src/openjarvis/memory/extractor.py`.

**Fact Storage:**
- Facts are stored as JSONL lines in `memory_facts.jsonl` (default location).
- Each fact has `text`, `source`, `created_at`, and `trust` fields.

**Retrieval:**
- Facts are retrieved via `FactStore.list()` which returns all facts (oldest first).
- The `MemoryService` provides `load_configured_facts` to get recallable facts for context injection.
- No built-in vector search or similarity retrieval; retrieval is sequential scan.

**Update:**
- Facts are added via `FactStore.add(text, source)`.
- Trust can be updated via `FactStore.set_trust(index, trust)`.
- Facts are deduplicated on addition (case-insensitive text match).
- Oldest facts are evicted when `max_facts` is exceeded.

**Deletion:**
- All facts can be cleared via `FactStore.clear()`.
- Individual fact deletion is not supported in the `FactStore` API.

**Provenance:**
- Each fact includes a `source` string (where the fact came from) and a `trust` field.
- Trust tiers: 
  - `TRUST_AUTO` = "auto" (auto-extracted and scanner-clean)
  - `TRUST_TRUSTED` = "trusted" (explicitly vouched for by the user)
  - `TRUST_UNTRUSTED` = "untrusted" (scanner flagged)
  - Empty string `""` = legacy (treated as recallable for backward compatibility)
- Only facts with trust in `RECALLABLE_TRUST_TIERS` (`{"", TRUST_AUTO, TRUST_TRUSTED}`) are recallable for model context.

**Metadata:**
- Minimal metadata: `source`, `created_at`, `trust`.
- No additional metadata fields in the `Fact` dataclass.

**Persistence:**
- Facts are persisted to disk immediately on addition (via atomic write: temp file + rename).
- The store is thread-safe and uses cross-process locking for concurrent access.

**Lifecycle:**
- Facts are added continuously by the background memory service.
- Facts are never modified except to update trust (via explicit review).
- Facts are evicted when the store exceeds `max_facts`.
- Facts can be cleared entirely via `clear()`.

**Limits:**
- `max_facts` defaults to 1000 (configurable via `MemoryConfig.max_facts`).
- No explicit size limit per fact, but very large facts may be impractical.

**Indexing:**
- No indexing; retrieval is a linear scan of all facts.
- The store is designed to be small (capped at 1000 facts) so linear scan is acceptable.

**Mapping Phase 6 Memory Architecture:**
- **User Preference Memory**: Can be built using the same `FactStore` mechanism or a custom SQLite table (since preferences are structured).
- **Entity Memory**: Requires a custom storage solution (not provided by OpenJarvis out-of-the-box).
- **Event Memory**: Requires a custom storage solution.
- **Timeline Memory**: Requires a custom storage solution.
- **Fact Memory**: Can reuse the existing `LocalFactStore` for awareness-specific facts if desired, but awareness may need more structured fact storage.
- **Notification History**: Can be stored in a custom SQLite table or reuse existing patterns (like digest store).
- **Feedback Memory**: Can be stored in a custom SQLite table.
- **Audit/System Memory**: Can reuse existing audit log mechanisms or custom storage.

**What can be:**
- **USE AS-IS**: 
  - The `FactStore` pattern for simple key-value or fact-like storage.
  - The SQLite storage pattern used by digest store, telemetry, etc., for structured data.
  - The event bus (`core/events.py`) for inter-component awareness communication.
- **EXTENDED**: 
  - The memory service could be extended to extract awareness-specific facts (e.g., event mentions).
  - The fact store could be extended with additional fields (would require modifying `FactStore` and `Fact` dataclass, which is not recommended; better to create a new store).
- **MODIFIED**: 
  - Not recommended to modify existing memory store; better to extend or build separately.
- **BUILT SEPARATELY**: 
  - Entity memory, event memory, timeline memory, user preference memory, notification history, feedback memory, and awareness-specific audit logs should be built as new storage components, possibly using the same SQLite pattern or a custom registry.

**Evidence:**
- `src/openjarvis/memory/store.py`: Defines `FactStore` ABC and `LocalFactStore` implementation.
- `src/openjarvis/memory/service.py`: Shows the memory service that extracts and stores facts.
- `src/openjarvis/agents/digest_store.py`: Shows SQLite storage pattern for digest artifacts.
- `src/openjarvis/server/savings.py`: Shows SQLite storage for telemetry.
- `src/openjarvis/server/session_store.py`: Shows SQLite storage for sessions.
- `src/openjarvis/scheduler/store.py`: Shows SQLite storage for scheduler tasks.
- `src/openjarvis/core/events.py`: Shows event bus for decoupled communication.

## Part 9 — Entity / History Capabilities

**What OpenJarvis ACTUALLY provides:**
- **Entities**: No built-in entity tracking, resolution, or memory.
- **Conversation History**: 
  - Provided via the `Conversation` type in `core/types.py`.
  - Used by agents to maintain the conversation window.
  - Persistence is not automatic; agents must manage conversation history themselves (though the `simple` agent does not persist history beyond the turn).
- **Event History**: 
  - No built-in event history storage.
  - The `Event` type in `core/events.py` is for the event bus, not for persisting world events.
  - However, the `Trace` and `TraceStep` types in `core/types.py` provide a way to record agent execution traces, which could be adapted for event history but are intended for agent introspection.
- **Temporal Memory**: 
  - No built-in temporal memory beyond timestamps in facts and traces.
  - No time-based querying or time-travel capabilities.
- **Persistent State**: 
  - Provided via SQLite stores for specific purposes (digest, telemetry, etc.).
  - No general-purpose key-value or object storage service.
- **Traces**: 
  - Agent execution traces are supported via the `Trace` type and can be stored if `TracesConfig.enabled` is true.
  - Traces are stored in SQLite (see `server/savings.py` or dedicated traces table).
- **Sessions**: 
  - Multi-turn conversation sessions across channels are supported via `SessionConfig` and `SessionStore` (see `server/session_store.py`).
  - Sessions store channel-specific state and can be linked across channels.

**What we need to build for awareness:**
- **Entity**: 
  - Entity storage with canonical IDs, aliases, attributes, relationships, associated events, and historical state.
  - Need entity resolution (linking mentions to canonical entities).
- **Entity identity**: 
  - Global unique identifier for each entity.
- **Aliases**: 
  - Alternative names or variations for the same entity.
- **Relationships**: 
  - Relationships between entities (e.g., subsidiary, part-of, location-of).
- **Event association**: 
  - Linking entities to the events they participate in.
- **Entity history**: 
  - Historical states of entities (e.g., CEO changes, product launches, policy updates).
- **Timeline**: 
  - Chronological sequence of events or state changes for an entity or topic.

**Evidence:**
- `src/openjarvis/core/types.py`: Defines `Conversation`, `Trace`, `TraceStep` for agent-level history.
- `src/openjarvis/server/session_store.py`: Shows how session state is stored.
- `src/openjarvis/agents/digest_store.py`: Shows how digest artifacts are stored.
- `src/openjarvis/server/savings.py`: Shows how telemetry is stored.
- No entity or event storage found in the codebase.

## Part 10 — RAG / Retrieval

**Retrieval Interface:**
- Not explicitly defined as a separate interface; retrieval is embedded in:
  - The memory service for fact retrieval (`MemoryService` uses `FactStore.list()`).
  - The `knowledge_search` tool (`tools/knowledge_search.py`) which appears to be for querying a knowledge base (possibly SQLite-based).
  - The `retrieval` tool (`tools/retrieval.py`) which is vague.
  - The `db_query` tool (`tools/db_query.py`) for executing SQL queries.

**Available Backends:**
- **Fact Retrieval**: 
  - Backend: `FactStore` (specifically `LocalFactStore` for JSONL).
  - Retrieval method: `list()` returns all facts (oldest first); no filtering or scoring.
  - No indexing, no vector search, no relevance scoring.
- **Knowledge Search Tool**: 
  - Appears to use a SQLite knowledge base (see `tools/knowledge_search.py`).
  - However, the implementation is not visible in the provided code snippets; we would need to examine it.
- **DB Query Tool**: 
  - Allows arbitrary SQL queries on the SQLite database (likely the same one used for telemetry, traces, etc.).
  - This could be used to implement custom retrieval if we store awareness data in SQLite.

**Indexing:**
- No built-in indexing for facts; linear scan is used.
- The SQLite databases used by other stores (digest, telemetry, etc.) likely benefit from SQLite's automatic indexing (primary keys, etc.) but no explicit indexing configuration is visible.

**Chunking:**
- Not applicable to fact retrieval; facts are short statements.
- For document retrieval (if we had a document store), chunking would be needed, but OpenJarvis does not have a built-in document store for RAG.

**Embeddings:**
- No built-in embedding generation or vector similarity search.
- The `knowledge_search` tool might use simple text matching or SQLite FTS (Full-Text Search), but we would need to check its implementation.

**Metadata:**
- Facts have minimal metadata (`source`, `created_at`, `trust`).
- SQLite stores for other data (digest, telemetry) have richer metadata.

**Filtering:**
- Fact retrieval has no filtering; returns all facts.
- For custom SQLite tables, we can add WHERE clauses for filtering.

**Ranking:**
- No built-in ranking algorithm for fact retrieval.
- For custom retrieval, we would need to implement scoring (e.g., TF-IDF, BM25) or use SQLite's FTS ranking.

**Persistence:**
- Fact storage is persistent via JSONL file.
- SQLite stores are persistent.

**Query Interface:**
- Fact store: `list()` returns all facts.
- SQLite stores: Can execute arbitrary SQL via `db_query` tool or direct SQLite API (though direct API is not exposed as a service; we would need to create a wrapper).

**How awareness information could be retrieved:**
- If we store awareness data (events, entities, etc.) in SQLite tables, we can use SQL queries for retrieval.
- If we want to use the existing fact store mechanism, we would be limited to sequential scan of all facts (which is acceptable for small datasets but not scalable).
- For scalable retrieval, we should build a custom SQLite-backed storage with appropriate indices.

**Limitations relevant to:**
- **Event history**: 
  - No built-in event storage; we would need to build it.
  - If using SQLite, we can index by time, entity ID, etc.
- **Entity history**: 
  - No built-in entity storage; we would need to build it.
  - If using SQLite, we can index by entity ID and time.
- **Source history**: 
  - Not explicitly required, but if we want to track source reliability over time, we would need to build source storage with time-indexed reliability scores.
- **Temporal retrieval**: 
  - No built-in temporal queries; we would need to implement time-based WHERE clauses in SQL or scan and filter in memory.

**Evidence:**
- `src/openjarvis/tools/knowledge_search.py`: Need to examine to see what knowledge base it uses.
- `src/openjarvis/tools/db_query.py`: Allows arbitrary SQL queries.
- `src/openjarvis/memory/store.py`: Shows fact storage limitations.
- `src/openjarvis/agents/digest_store.py`: Shows how to structure SQLite storage for complex data.
- `src/openjarvis/server/savings.py`: Shows SQLite schema for telemetry.

Let's examine the knowledge search tool:




<tool_call>
<function=bash>
<parameter=command>
type "C:\Ai_Agent_Research\candidate\Openjarvis\src\openjarvis\tools\knowledge_search.py"