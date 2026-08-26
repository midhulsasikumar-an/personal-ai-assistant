# Open-Source AI Foundation Requirements

**Document Type:** Open-Source Foundation Evaluation Specification  
**Purpose:** Define what we expect an existing open-source AI agent/framework from GitHub to provide before considering it as a technical foundation.

---

## 1. Purpose

This document defines the technical, architectural, engineering, and project-health capabilities expected from an existing open-source AI agent or AI-agent framework.

Its purpose is to let us evaluate GitHub projects consistently before choosing a foundation.

The goal is to reuse mature, generic AI-agent infrastructure wherever practical rather than unnecessarily rebuilding it.

This document is **strictly limited to the open-source foundation**.

---

## 2. Evaluation Principle

We are **not** looking for an open-source project that already implements the final system.

We are looking for a strong technical foundation that already solves as much generic infrastructure as possible and can be understood, extended, modified, and maintained by us.

The preferred foundation should:

- provide mature reusable infrastructure;
- have a modular and understandable architecture;
- allow components to be extended or replaced;
- avoid unnecessary vendor lock-in;
- support local execution;
- have sufficient documentation and tests;
- have an active and healthy repository;
- allow us to retain control over the resulting system.

The number of features alone is not enough. Architecture quality and extensibility are equally important.

---

# 3. Strict Scope Boundary

## 3.1 This document DOES evaluate

- Agent runtime
- LLM/model abstraction
- Local model support
- Tool/function calling
- Agent orchestration
- Session and persistent memory infrastructure
- RAG/retrieval infrastructure
- Embeddings/vector-store integrations
- Data/document ingestion
- Voice infrastructure
- Browser/web interaction
- File-system interaction
- Code execution
- Background processing
- Scheduling
- Plugin/extension mechanisms
- APIs and interfaces
- Storage
- Configuration
- Security and permissions
- Logging and observability
- Testing
- Documentation
- Deployment
- Performance
- Maintainability
- Repository health
- Licensing

## 3.2 This document DOES NOT evaluate

The following are deliberately excluded:

- World-awareness intelligence
- Current-affairs intelligence
- News analysis
- Business intelligence
- Startup intelligence
- Opportunity detection
- Risk analysis
- Learning recommendations
- Trend prediction
- Personal relevance analysis
- Custom intelligence/briefing logic
- Domain-specific reasoning
- The final JARVIS intelligence layer

A repository must **not** be rejected because it does not provide those capabilities. They are outside the scope of this foundation evaluation.

---

# 4. Requirement Classification

Every capability found in a repository should be classified as:

| Classification | Meaning |
|---|---|
| **Native** | Implemented directly and usable in the repository |
| **Integratable** | Available through a clean, supported integration |
| **Modifiable** | Partially present but requires meaningful changes |
| **Missing** | Not provided |
| **Incompatible** | Exists but is unsuitable because of architectural or implementation constraints |
| **Unknown** | Cannot be reliably verified |

---

# 5. Evidence Standard

A capability should not be marked as present solely because it appears in a README or marketing description.

Preferred evidence, in order of confidence:

1. Source code
2. Official documentation
3. Tests
4. Working examples/configuration
5. Official integrations
6. Release history

For significant requirements, record the evidence used.

If a capability cannot be verified, mark it **Unknown** rather than assuming it exists.

---

# 6. Architecture Requirements

## ARCH-001 — Modular Architecture

The project should have clear, logically separated modules.

Expected characteristics:

- clear module boundaries;
- separation of responsibilities;
- limited unnecessary coupling;
- independently understandable components.

**Priority:** Critical

## ARCH-002 — Loose Coupling

Major components should not depend unnecessarily on implementation details of other components.

Important components should be replaceable without rewriting unrelated parts.

**Priority:** Critical

## ARCH-003 — Separation of Concerns

The architecture should clearly separate areas such as:

- agent execution;
- model interaction;
- tools;
- memory;
- storage;
- interfaces;
- configuration;
- background processing.

**Priority:** Critical

## ARCH-004 — Extension Points

The architecture should provide clear mechanisms for adding functionality.

**Priority:** Critical

## ARCH-005 — Replaceable Components

Important infrastructure should be replaceable where practical.

Examples:

- model provider;
- memory backend;
- storage;
- voice provider;
- tools;
- UI/interface.

**Priority:** Critical

## ARCH-006 — Understandable Architecture

An engineer should be able to determine:

- where execution starts;
- what the major components do;
- how components communicate;
- where extensions belong;
- how a request flows through the system.

**Priority:** Critical

## ARCH-007 — Dependency Management

Dependencies should be explicit, reproducible, and reasonably controlled.

**Priority:** High

---

# 7. Agent Runtime Requirements

## AGENT-001 — Agent Execution Runtime

The foundation should provide a reusable mechanism for creating and executing AI agents.

**Priority:** Critical

## AGENT-002 — Agent Lifecycle

The runtime should have a clear lifecycle for:

- initialization;
- execution;
- tool interaction;
- completion;
- error handling;
- shutdown.

**Priority:** High

## AGENT-003 — Tool Calling

Agents should be capable of invoking registered tools.

**Priority:** Critical

## AGENT-004 — Function Calling

Structured function/tool invocation should be supported where the underlying model allows it.

**Priority:** High

## AGENT-005 — Multi-Step Execution

The runtime should support tasks involving multiple model/tool steps.

**Priority:** High

## AGENT-006 — Agent State

The runtime should provide a mechanism for maintaining execution state.

**Priority:** High

## AGENT-007 — Agent Orchestration

The framework should support coordinating multiple tools, tasks, workflows, or agents.

**Priority:** Medium

## AGENT-008 — Error Recovery

The runtime should handle model failures, tool failures, and recoverable execution errors in a controlled way.

**Priority:** High

---

# 8. LLM and Model Abstraction Requirements

## LLM-001 — Model Abstraction

Application logic should not be tightly coupled to a single model provider.

**Priority:** Critical

## LLM-002 — Provider Switching

It should be reasonably easy to change the underlying model/provider.

The exact provider list is less important than having a clean abstraction.

**Priority:** Critical

## LLM-003 — Local Model Support

Support for local inference should be strongly preferred.

The foundation should be capable of integrating local model runtimes.

**Priority:** Critical

## LLM-004 — Streaming

Streaming model responses should be supported where the provider permits it.

**Priority:** High

## LLM-005 — Model Configuration

The model layer should support configuration of applicable settings such as:

- model;
- provider;
- temperature;
- context configuration;
- token/output limits.

**Priority:** High

## LLM-006 — Stable Model Interface

Application code should communicate with a stable model abstraction rather than provider-specific implementation details.

**Priority:** Critical

---

# 9. Tool Framework Requirements

## TOOL-001 — Tool Registration

There should be a clear mechanism for registering tools.

**Priority:** Critical

## TOOL-002 — Custom Tools

Developers should be able to create and add tools without modifying unrelated core components.

**Priority:** Critical

## TOOL-003 — Tool Discovery

Agents should have structured access to the tools made available to them.

**Priority:** High

## TOOL-004 — Tool Input Validation

Tool parameters should be validated before execution where appropriate.

**Priority:** High

## TOOL-005 — Tool Error Handling

Tool failures should be returned through controlled error mechanisms.

**Priority:** High

## TOOL-006 — Tool Permissions

The architecture should support restricting which tools can be used.

**Priority:** High

---

# 10. Memory Infrastructure Requirements

This section evaluates **generic memory infrastructure only**.

## MEM-001 — Session Memory

The foundation should support retaining context within a conversation/session.

**Priority:** Critical

## MEM-002 — Persistent Memory

The foundation should provide or cleanly support persistent memory.

**Priority:** High

## MEM-003 — Replaceable Memory Backend

Memory should not be permanently tied to one storage implementation.

**Priority:** High

## MEM-004 — Memory Retrieval

The framework should provide mechanisms for retrieving stored context.

**Priority:** High

## MEM-005 — Vector Database Integration

Vector storage/retrieval should be supported directly or through clean integrations.

**Priority:** High

---

# 11. Retrieval and RAG Requirements

## RAG-001 — Document Ingestion

The foundation should provide mechanisms for ingesting documents/data into retrieval systems.

**Priority:** High

## RAG-002 — Document Processing

The system should support reasonable document preprocessing/chunking.

**Priority:** Medium

## RAG-003 — Embeddings

Embedding generation should be supported through configurable providers or integrations.

**Priority:** High

## RAG-004 — Retrieval

Semantic or equivalent retrieval capabilities should be available.

**Priority:** High

## RAG-005 — Replaceable Retrieval Backend

The retrieval implementation should be replaceable.

**Priority:** High

---

# 12. Voice Infrastructure Requirements

Voice is evaluated only as a generic interface capability.

## VOICE-001 — Speech-to-Text

The foundation should support speech input directly or through a clean integration.

**Priority:** Medium

## VOICE-002 — Text-to-Speech

The foundation should support speech output directly or through a clean integration.

**Priority:** Medium

## VOICE-003 — Streaming Voice

Streaming audio should be supported where practical.

**Priority:** Medium

## VOICE-004 — Voice Provider Abstraction

Voice providers should be replaceable.

**Priority:** Medium

## VOICE-005 — Activation Mechanism

A mechanism for initiating a voice interaction is desirable.

**Priority:** Low

---

# 13. Browser and Web Interaction Requirements

## WEB-001 — Web Interaction

The foundation should support controlled interaction with web resources directly or through integrations.

**Priority:** Medium

## WEB-002 — Browser Automation

Browser automation support is desirable.

**Priority:** Medium

## WEB-003 — Web Retrieval

The system should provide mechanisms for retrieving web content where appropriate.

**Priority:** Medium

## WEB-004 — Browser Tool Extensibility

Browser capabilities should be exposed through an extensible tool mechanism.

**Priority:** Medium

---

# 14. File-System Requirements

## FILE-001 — File Reading

The agent should be able to access permitted files through tools/integrations.

**Priority:** High

## FILE-002 — File Writing

Controlled file creation/modification should be supported.

**Priority:** High

## FILE-003 — File Search

The foundation should support searching accessible files.

**Priority:** Medium

## FILE-004 — Workspace Isolation

File-system access should be restrictable to permitted locations.

**Priority:** High

---

# 15. Code Execution Requirements

## CODE-001 — Program Execution

The foundation should support controlled execution of code or external commands where required.

**Priority:** Medium

## CODE-002 — Execution Isolation

Code execution should preferably support sandboxing or isolation.

**Priority:** High

## CODE-003 — Execution Permissions

Execution permissions should be controllable.

**Priority:** High

---

# 16. Background Processing Requirements

## BG-001 — Background Jobs

The architecture should support work occurring independently of an active conversation.

**Priority:** Critical

## BG-002 — Worker Architecture

Background workers or an equivalent mechanism should be supported.

**Priority:** High

## BG-003 — Job State

Long-running/background jobs should have observable state.

**Priority:** Medium

## BG-004 — Failure Handling

Background failures should be logged and recoverable where practical.

**Priority:** High

---

# 17. Scheduling Requirements

## SCHED-001 — Scheduled Tasks

The foundation should support scheduled execution.

**Priority:** High

## SCHED-002 — Configurable Scheduling

Scheduling should not require modifying core source code.

**Priority:** High

## SCHED-003 — Job Management

There should be a way to inspect/manage scheduled or background jobs where applicable.

**Priority:** Medium

---

# 18. Plugin and Extension Requirements

## EXT-001 — Plugin Architecture

The foundation should provide a clear extension mechanism.

**Priority:** Critical

## EXT-002 — Custom Extensions

New capabilities should be addable without modifying unrelated core modules.

**Priority:** Critical

## EXT-003 — Extension Isolation

Extensions should have defined interfaces and boundaries.

**Priority:** High

## EXT-004 — Extension Lifecycle

Where applicable, extensions should have mechanisms for loading, configuration, and shutdown.

**Priority:** Medium

---

# 19. API and Interface Requirements

## API-001 — Programmatic API

The foundation should expose a usable programmatic interface.

**Priority:** High

## API-002 — CLI

A command-line interface is strongly preferred.

**Priority:** Medium

## API-003 — HTTP/API Interface

An HTTP or equivalent service API is desirable.

**Priority:** Medium

## API-004 — Interface Independence

The core runtime should not be tightly coupled to one UI.

**Priority:** Critical

## API-005 — Multiple Front Ends

Different interfaces should be able to use the same underlying runtime.

Examples:

- CLI;
- web;
- desktop;
- voice.

**Priority:** High

---

# 20. Storage Requirements

## STORE-001 — Persistent Storage

The foundation should support persistent application data.

**Priority:** High

## STORE-002 — Storage Abstraction

Application logic should not be unnecessarily tied to one database.

**Priority:** High

## STORE-003 — Local Storage

Local persistence should be supported.

**Priority:** High

## STORE-004 — Migration Support

Database/schema changes should have a controlled migration mechanism where applicable.

**Priority:** Medium

---

# 21. Configuration Requirements

## CONFIG-001 — Central Configuration

The project should provide a clear configuration system.

**Priority:** High

## CONFIG-002 — Environment Configuration

Environment variables should be supported where appropriate.

**Priority:** High

## CONFIG-003 — Secret Separation

Credentials must not need to be hard-coded into source code.

**Priority:** Critical

## CONFIG-004 — Environment-Specific Configuration

Development and production configuration should be separable.

**Priority:** Medium

---

# 22. Security and Permission Requirements

## SEC-001 — Permission Model

Potentially dangerous operations should be controllable.

**Priority:** Critical

## SEC-002 — Secret Management

API keys, tokens, and credentials should be handled securely.

**Priority:** Critical

## SEC-003 — Tool Permissions

Sensitive tools should be restrictable.

**Priority:** Critical

## SEC-004 — Execution Isolation

Code, command, or browser execution should support isolation or clean integration with a sandbox.

**Priority:** High

## SEC-005 — Local Execution

The architecture should allow significant portions of the system to operate locally.

**Priority:** Critical

---

# 23. Logging and Observability Requirements

## OBS-001 — Structured Logging

The application should provide meaningful logs.

**Priority:** High

## OBS-002 — Error Reporting

Failures should provide useful diagnostic information.

**Priority:** High

## OBS-003 — Debugging Support

Developers should be able to inspect execution behavior.

**Priority:** High

## OBS-004 — Agent Execution Visibility

Where possible, agent/tool execution state should be inspectable.

**Priority:** Medium

---

# 24. Testing Requirements

## TEST-001 — Automated Tests

The repository should contain meaningful automated tests.

**Priority:** Critical

## TEST-002 — Unit Testing

Important components should have unit tests.

**Priority:** High

## TEST-003 — Integration Testing

Important integrations should have integration tests where appropriate.

**Priority:** High

## TEST-004 — Reproducible Testing

The project should provide a reasonable way to run its tests.

**Priority:** High

---

# 25. Documentation Requirements

## DOC-001 — Installation Documentation

Installation and startup should be clearly documented.

**Priority:** Critical

## DOC-002 — Architecture Documentation

Major architectural components should be documented.

**Priority:** Critical

## DOC-003 — Extension/API Documentation

Public APIs and extension points should be documented.

**Priority:** High

## DOC-004 — Configuration Documentation

Configuration options should be documented.

**Priority:** High

## DOC-005 — Practical Examples

The repository should provide usable examples.

**Priority:** High

## DOC-006 — Source Understandability

Important modules should be understandable without undocumented assumptions.

**Priority:** High

---

# 26. Deployment Requirements

## DEP-001 — Reproducible Installation

The project should have a reproducible installation process.

**Priority:** High

## DEP-002 — Container Support

Containerized deployment is strongly preferred.

**Priority:** Medium

## DEP-003 — Local Deployment

The foundation must be capable of local execution.

**Priority:** Critical

## DEP-004 — Platform Documentation

Supported operating systems/platforms should be documented.

**Priority:** Medium

---

# 27. Performance Requirements

## PERF-001 — Reasonable Startup

The foundation should avoid unnecessary startup overhead.

**Priority:** Medium

## PERF-002 — Asynchronous Operations

Asynchronous execution should be supported where appropriate.

**Priority:** High

## PERF-003 — Streaming

Long-running model/tool operations should support streaming where practical.

**Priority:** High

## PERF-004 — Resource Awareness

CPU, RAM, GPU, and storage requirements should be reasonably documented or managed.

**Priority:** Medium

---

# 28. Maintainability Requirements

## MAINT-001 — Clear Code Organization

The source tree should have understandable responsibilities.

**Priority:** Critical

## MAINT-002 — Single Responsibility

Modules, classes, and functions should avoid unnecessary responsibility aggregation.

**Priority:** High

## MAINT-003 — Clear Interfaces

Cross-module interfaces should be explicit.

**Priority:** Critical

## MAINT-004 — Manageable Technical Debt

The foundation should not require major architectural cleanup before meaningful extension.

**Priority:** Critical

## MAINT-005 — Dependency Stability

Core dependencies should be reasonably maintained and controlled.

**Priority:** High

---

# 29. Repository Health Requirements

## HEALTH-001 — Active Maintenance

Recent meaningful activity should indicate that the repository is maintained.

**Priority:** Critical

## HEALTH-002 — Issue/PR Activity

Issues and pull requests should provide evidence of project activity where applicable.

**Priority:** High

## HEALTH-003 — Release History

A reasonable release/versioning history is preferred.

**Priority:** High

## HEALTH-004 — Community

A healthy contributor/community ecosystem is preferred.

**Priority:** Medium

## HEALTH-005 — Project Direction

The project's current direction should be compatible with continued use as a foundation.

**Priority:** Critical

---

# 30. Licensing Requirements

## LIC-001 — Clear Open-Source License

The repository must have a clearly identified license.

**Priority:** Critical

## LIC-002 — Modification Rights

The license must permit the intended modification and extension.

**Priority:** Critical

## LIC-003 — Commercial Compatibility

If eventual commercialization is possible, license compatibility must be evaluated.

**Priority:** High

## LIC-004 — Dependency Licenses

Important dependency licenses should also be checked.

**Priority:** High

---

# 31. Open-Source Capabilities We Prefer to Reuse

When evaluating candidates, preference should be given to mature implementations of generic infrastructure such as:

- Agent runtime
- Model abstraction
- Local model integration
- Tool/function calling
- Agent orchestration
- Session memory
- Persistent memory infrastructure
- Retrieval/RAG
- Embeddings
- Vector-store integration
- Voice infrastructure
- Plugin/extension mechanisms
- Browser/web tooling
- File tooling
- Code execution infrastructure
- Background workers
- Scheduling
- APIs/interfaces
- Configuration
- Logging
- Security
- Testing
- Deployment

The purpose is to minimize unnecessary redevelopment of mature generic infrastructure.

---

# 32. Replaceability Requirement

A feature being present is not enough.

The foundation should also be evaluated on whether we can replace that feature later.

For example, determine whether it is possible to replace:

- LLM provider
- Local model runtime
- Memory backend
- Vector database
- Voice provider
- Storage system
- Tool implementations
- Browser automation
- UI/interface
- Background job system

without rewriting the entire application.

A foundation that provides many capabilities but creates strong architectural lock-in should receive a lower evaluation.

---

# 33. Red Flags

The following characteristics should significantly reduce suitability:

- Monolithic architecture
- Strong coupling to one model provider
- Cloud-only architecture
- Hard-coded credentials
- Poor security boundaries
- No meaningful tests
- Poor documentation
- Abandoned repository
- Unclear licensing
- Difficult installation
- No extension mechanism
- Excessive hacks in core functionality
- Undocumented critical behavior
- Difficult-to-replace components
- Poor separation of concerns
- Uncontrolled code execution
- No reasonable way to understand or modify the source

A red flag does not automatically mean rejection. Its severity and impact must be recorded.

---

# 34. Repository Evaluation Matrix

Every candidate should eventually be evaluated using a consistent matrix.

| Category | Requirement | Status | Evidence | Quality | Modification Needed | Notes |
|---|---|---|---|---|---|---|
| Architecture | Modular architecture | | | | | |
| Architecture | Loose coupling | | | | | |
| Agent | Agent runtime | | | | | |
| Agent | Tool calling | | | | | |
| LLM | Model abstraction | | | | | |
| LLM | Local model support | | | | | |
| Memory | Session memory | | | | | |
| Memory | Persistent memory | | | | | |
| RAG | Retrieval | | | | | |
| Voice | STT | | | | | |
| Voice | TTS | | | | | |
| Extensions | Plugin system | | | | | |
| Background | Workers | | | | | |
| Scheduling | Scheduled jobs | | | | | |
| Security | Permission system | | | | | |
| Testing | Automated tests | | | | | |
| Documentation | Architecture docs | | | | | |
| Health | Active maintenance | | | | | |
| License | Suitable license | | | | | |

---

# 35. Evaluation Scoring

A repository should not be selected solely by feature count.

Evaluation should consider:

1. Capability coverage
2. Implementation maturity
3. Architecture quality
4. Extensibility
5. Replaceability
6. Documentation
7. Testing
8. Security
9. Local execution
10. Maintenance/community health
11. Licensing
12. Engineering effort required to adapt it

A smaller but well-designed foundation may be preferable to a feature-heavy but tightly coupled project.

---

# 36. Requirement Scoring Scale

Each requirement can be scored:

| Score | Meaning |
|---:|---|
| 0 | Missing |
| 1 | Experimental/poor |
| 2 | Basic implementation |
| 3 | Usable implementation |
| 4 | Mature implementation |
| 5 | Mature, extensible, well documented |

Every significant score should have supporting evidence.

---

# 37. Repository Review Template

When evaluating a GitHub repository, record:

## Repository

- Name:
- GitHub URL:
- License:
- Primary language:
- Latest release:
- Last meaningful commit:
- Contributors:
- Stars:
- Forks:

## Architecture

- Overall architecture:
- Major modules:
- Entry points:
- Extension points:
- Coupling concerns:

## Agent Runtime

- Agent execution:
- Tool calling:
- Function calling:
- Multi-step execution:
- Orchestration:

## Model Layer

- Model abstraction:
- Local models:
- Provider support:
- Streaming:
- Switching difficulty:

## Memory/RAG

- Session memory:
- Persistent memory:
- Retrieval:
- Embeddings:
- Vector database:

## Interfaces

- CLI:
- API:
- Web:
- Voice:
- Interface independence:

## Infrastructure

- Background workers:
- Scheduling:
- Storage:
- Configuration:
- Logging:
- Security:

## Engineering Quality

- Tests:
- Documentation:
- CI/CD:
- Code organization:
- Maintainability:

## Repository Health

- Maintenance:
- Community:
- Releases:
- Issues:
- Roadmap:

## Final Assessment

- Major strengths:
- Major weaknesses:
- Major risks:
- Capabilities suitable for reuse:
- Components likely to require replacement:
- Overall suitability:
- Recommended next action:

---

# 38. Final Selection Questions

Before selecting a foundation, answer:

1. Can we understand its architecture?
2. Can we modify its core components?
3. Can we add substantial functionality without breaking the core?
4. Can we replace the model layer?
5. Can we run it locally?
6. Can we extend its tools?
7. Can we extend its memory infrastructure?
8. Can we add background processing?
9. Can we add or replace interfaces?
10. Can we control permissions?
11. Can we test our modifications?
12. Is the license suitable?
13. Is the repository actively maintained?
14. Is the documentation sufficient?
15. Would using it save substantial engineering effort?
16. Does adopting it create architectural lock-in?
17. Would we understand the code well enough to maintain it ourselves?

---

# 39. Final Evaluation Principle

Do not select a repository because:

- it has the most GitHub stars;
- it has the largest feature list;
- its README looks impressive;
- it is currently popular;
- it claims to support many capabilities.

The foundation should be selected based on:

> **Architecture + maturity + extensibility + maintainability + openness + engineering value.**

The central question is:

> **Does this project provide a strong, understandable, maintainable foundation that we can build upon without becoming permanently dependent on its architecture?**

---

# 40. Evaluation Workflow

The intended workflow is:

```text
Open-Source Foundation Requirements
                ↓
Identify Candidate GitHub Projects
                ↓
Inspect Repository
                ↓
Inspect Documentation
                ↓
Inspect Source Code
                ↓
Evaluate Requirements
                ↓
Record Evidence
                ↓
Score Capabilities
                ↓
Identify Strengths and Weaknesses
                ↓
Compare Candidates
                ↓
Select Foundation
                ↓
Then Define the Custom System Requirements
```

This document is the evaluation standard for the **open-source foundation stage**.

It deliberately stops before defining the custom intelligence or final product requirements.
