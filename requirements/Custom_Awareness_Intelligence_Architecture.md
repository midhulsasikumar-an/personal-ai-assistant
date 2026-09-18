# Custom Awareness Intelligence Architecture

## Section 1 — Architectural Goals
- **Correctness**: Accurate event detection, change detection, and relevance assessment.
- **Low notification noise**: Suppress duplicates and low-significance updates.
- **Explainability**: Every notification/digest item includes clear rationale.
- **Source provenance**: Preserve origin, timestamp, and retrieval metadata.
- **Temporal awareness**: Maintain timelines and historical state.
- **Event continuity**: Group related information into coherent events over time.
- **Personal relevance**: Match developments to user interests.
- **Modularity**: Separate concerns to allow independent evolution.
- **Testability**: Components can be unit-tested with deterministic fixtures.
- **Extensibility**: Add new sources, models, and analytics without major refactor.
- **Fault isolation**: Failures in one component do not cascade.
- **Security**: Treat external input as untrusted; defend against prompt injection.
- **Observability**: Log decisions, latency, and confidence for debugging.
- **Maintainability**: Clear boundaries and well-documented interfaces.
- **Model agnosticism**: Ability to swap LLMs or use deterministic alternatives.
- **Source flexibility**: Plug in new connectors with minimal changes.

## Section 2 — System Boundaries

### Layer A — OpenJarvis Foundation
- **Responsibilities**: Agent lifecycle, model abstraction, tool framework (web access, scrapers, APIs), RAG infrastructure, memory infrastructure (caching, persistence), storage, scheduling/background workers, security sandboxing, observability (metrics, logging, tracing), extension points/APIs.
- **Owns**: Runtime, model interfaces, tool execution, basic memory KV store, HTTP client, job scheduler, secret management, health checks.
- **Does NOT own**: Event semantics, change detection, relevance/importance scoring, user interest modeling, notification/digest formatting, awareness-specific memory schemas.
- **Interfaces**: 
  - *Up*: Receives config, schedules jobs, provides tool access to custom layer.
  - *Down*: Exposes APIs for custom layer to store/retrieve awareness memory, trigger model inference, access web, use RAG.

### Layer B — Custom Extension / Integration Layer
- **Responsibilities**: Adapts OpenJarvis foundation to awareness needs: wraps tool calls, transforms data into awareness contracts, routes output to custom components, handles foundation-provided events (e.g., model completion, tool result), implements shared interfaces defined by OpenJarvis extension points.
- **Owns**: Adapter code, schema translation, middleware for provenance injection, lightweight orchestration that calls foundation services.
- **Does NOT own**: Core awareness algorithms (event detection, change detection, etc.), long-term awareness memory semantics, user-facing notification/digest generation.
- **Interfaces**: 
  - *Up*: Calls foundation tools (web fetch, LLM invoke, RAG query, memory put/get).
  - *Down*: Passes normalized, provenance-tagged InformationItems to custom intelligence components.

### Layer C — Custom Awareness Intelligence
- **Responsibilities**: All awareness-specific logic: ingestion pipelines, normalization, deduplication, event/entity understanding, change detection, historical context, relevance, importance, impact, confidence/trust, notification decision, explanation generation, feedback processing, awareness memory management.
- **Owns**: Event, entity, topic schemas; pipelines; decision engines; memory stores; feedback loops; observability hooks for awareness metrics.
- **Does NOT own**: Low-level HTTP, model inference primitives, foundation tool execution, foundation scheduling (though may schedule jobs via foundation).
- **Interfaces**: 
  - *Up*: Consumes foundation services via extension layer.
  - *Down*: Emits notifications/digest items to foundation delivery mechanisms (e.g., via a custom tool or foundation messaging).

## Section 3 — High-Level Architecture (ASCII)

```
                    INFORMATION SOURCES (RSS, APIs, Web, etc.)
                           |
                           v
                  +-------------------+
                  | Source Connectors | (Ext Layer: wraps foundation web/tools)
                  +-------------------+
                           |
                           v
                  +-------------------+
                  | Ingestion Layer   | (Custom: raw -> InformationItem + provenance)
                  +-------------------+
                           |
                           v
                  +-------------------+
                  | Normalizer        | (Custom: schema enforcement, language)
                  +-------------------+
                           |
                           v
                  +-------------------+
                  | Provenance Manager| (Custom: dedup input, source trust init)
                  +-------------------+
                           |
                           v
                  +-------------------+
                  | Deduplication     | (Custom: exact & near-dup suppression)
                  +-------------------+
                           |
                           v
              +--------------------------+
              | Event / Entity Analysis  | (Custom: NER, linking, typing)
              +--------------------------+
                    |              |
                    v              v
              +----------+   +----------+
              | Event    |   | Entity   |
              | Store    |   | Store    |   (Custom: awareness memory)
              +----------+   +----------+
                    |              |
                    +-------+------+
                            |
                            v
                    +------------------+
                    | Change Detector  | (Custom: compare state delta)
                    +------------------+
                            |
                            v
                    +------------------+
                    | Historical Context| (Custom: fetch prior state/events)
                    +------------------+
                            |
                            v
                    +------------------+
                    | Relevance Engine | (Custom: match to user interests)
                    +------------------+
                            |
                            v
                    +------------------+
                    | Importance Engine| (Custom: intrinsic significance)
                    +------------------+
                            |
                            v
                    +------------------+
                    | Impact Analyzer  | (Custom: direct/indirect, pos/neg)
                    +------------------+
                            |
                            v
              +------------------------------+
              | Confidence & Source Trust   | (Custom: uncertainty propagation)
              +------------------------------+
                            |
                            v
                    +------------------+
                    | Notification Decision| (Custom: priority, suppression)
                    +------------------+
                       /               \
                      /                 \
                     v                   v
          +------------------+   +------------------+
          | Notification     |   | Digest Engine    |
          | Formatter        |   | (Custom: periodic)
          +------------------+   +------------------+
                     |                   |
                     v                   v
              +------------------+   +------------------+
              | User Feedback    |   | Delivery via    |
              | Processor        |   | Foundation tools|
              +------------------+   +------------------+
                     |                   |
                     +--------+----------+
                              |
                              v
                    +------------------+
                    | Awareness Memory | (Custom: persist events, entities, user model, feedback)
                    +------------------+
                              |
                              v
                    +------------------+
                    | Observability & Audit Logs (Custom metrics + foundation logging) 
                    +------------------+
```

## Section 4 — Component Decomposition

### 1. Source Connectors (Extension Layer)
- **Purpose**: Fetch raw data from configured sources using foundation web/tools.
- **Responsibilities**: Authenticate, rate-limit, handle pagination/continuation, record raw fetch metadata.
- **Inputs**: Source config (URL, auth, schedule).
- **Outputs**: RawBlob + fetch metadata (timestamp, source ID, HTTP status).
- **State**: None (stateless per fetch).
- **Depends**: Foundation HTTP tool, secret manager.
- **Sync/Async**: Async (scheduled or push).
- **Failure**: Retry with backoff, emit error metric.
- **Security**: Validate URLs, sandbox requests.
- **Observability**: Fetch latency, success rate.

### 2. Ingestion Pipeline (Custom)
- **Purpose**: Transform RawBlob into InformationItem with provenance.
- **Responsibilities**: Decode (if needed), extract text, assign UUID, attach source ID, fetch time, content hash.
- **Inputs**: RawBlob, fetch metadata.
- **Outputs**: InformationItem.
- **State**: None.
- **Depends**: None (pure function).
- **Sync/Async**: Synchronous per item.
- **Failure**: Skip item, log error.
- **Observability**: Items processed/sec.

### 3. Normalizer (Custom)
- **Purpose**: Enforce common schema, language detection, basic cleaning.
- **Responsibilities**: Strip HTML, normalize whitespace, detect language, truncate to max length.
- **Inputs**: InformationItem.
- **Outputs**: NormalizedInformationItem (same ID, normalized content).
- **State**: None.
- **Depends**: None.
- **Sync/Async**: Synchronous.
- **Failure**: Pass-through with warning.
- **Observability**: Normalization error rate.

### 4. Provenance Manager (Custom)
- **Purpose**: Initialize source trust, prepare for dedup.
- **Responsibilities**: Look up source reliability score, attach initial confidence based on source type.
- **Inputs**: NormalizedInformationItem.
- **Outputs**: ProvenancedInformationItem (adds sourceTrust, initialConfidence).
- **State**: Source trust table (loaded from memory).
- **Depends**: Source Trust Engine (read-only).
- **Sync/Async**: Synchronous.
- **Failure**: Use default low trust.
- **Observability**: Source usage stats.

### 5. Deduplication Engine (Custom)
- **Purpose**: Suppress exact and near-duplicate items.
- **Responsibilities**: Compute content hash, similarity (MinHash/Jaccard) against recent window, drop if duplicate.
- **Inputs**: ProvenancedInformationItem.
- **Outputs**: UniqueInformationItem or None (if duplicate).
- **State**: Sliding window of hashes/signatures (in-memory, TTL-based).
- **Depends**: None.
- **Sync/Async**: Synchronous.
- **Failure**: Fail open (let through) on error.
- **Observability**: Duplicate rate.

### 6. Event / Entity Analysis (Custom)
- **Purpose**: Extract entities, topics, facts; perform typing and linking.
- **Responsibilities**: 
  - Run foundation NER tool (or lightweight regex/gazetteer).
  - Link entities to canonical KB (e.g., Wikidata via foundation RAG or local map).
  - Extract tentative facts (subject-verb-object patterns).
  - Assign topical tags via keyword or zero-shot classifier.
- **Inputs**: UniqueInformationItem.
- **Outputs**: AnalyzedItem (entities[], topics[], facts[], sourceLinks).
- **State**: Entity alias map, topic hierarchy (loaded from memory).
- **Depends**: Foundation NER tool, optional LLM for disambiguation.
- **Sync/Async**: Synchronous (foundation tool call).
- **Failure**: Return partial extraction, flag low confidence.
- **Observability**: Entity/link success rate.

### 7. Event Store & Entity Store (Custom - Awareness Memory)
- **Purpose**: Persist canonical events and entities with timelines.
- **Responsibilities**: 
  - Event Store: Create/update events, attach items, maintain state summary.
  - Entity Store: Maintain canonical entity record, aliases, attributes, related entities.
- **Inputs**: AnalyzedItem (for event creation/update).
- **Outputs**: Stored references (eventId, entityIds).
- **State**: Persistent (foundation storage via extension layer).
- **Depends**: Foundation storage interface (KV or document).
- **Sync/Async**: Asynchronous write (fire-and-forget with retry).
- **Failure**: Queue for retry, log error.
- **Observability**: Store latency, error rate.

### 8. Change Detector (Custom)
- **Purpose**: Determine what is new relative to prior known state.
- **Responsibilities**: 
  - Retrieve prior state for affected entities/event from stores.
  - Compute delta (new/changed/removed facts, entity attribute changes).
  - Classify change type (new event, update, correction, escalation, etc.).
- **Inputs**: AnalyzedItem, related eventId/entityIds.
- **Outputs**: ChangeSet (delta description, changeType, confidence).
- **State**: None (reads from stores).
- **Depends**: Event Store, Entity Store.
- **Sync/Async**: Synchronous (reads then compute).
- **Failure**: Treat as unknown change, low confidence.
- **Observability**: Change detection latency, distribution of change types.

### 9. Historical Context Engine (Custom)
- **Purpose**: Provide relevant prior events/developments for interpretation.
- **Responsibilities**: 
  - Query event/store for prior events on same entities/topics within time window.
  - Summarize trend, previous milestones, recurring patterns.
- **Inputs**: EventId, entityIds, timestamp.
- **Outputs**: ContextBundle (prior events, timeline snippets, trend notes).
- **State**: None (read-only query).
- **Depends**: Event Store, Entity Store.
- **Sync/Async**: Synchronous (bounded query).
- **Failure**: Return empty context.
- **Observability**: Context fetch latency.

### 10. Relevance Engine (Custom)
- **Purpose**: Score personal relevance of change/event.
- **Responsibilities**: 
  - Match entities/topics against user interest model (weights, explicit/temporary/muted).
  - Factor in historical relevance (user previously engaged).
  - Combine into relevance score (0-1) with explanation factors.
- **Inputs**: ChangeSet, user profile, historical relevance flags.
- **Outputs**: RelevanceAssessment (score, factors[], confidence).
- **State**: Reads user interest model.
- **Depends**: User Interest Model (read).
- **Sync/Async**: Synchronous.
- **Failure**: Default low relevance.
- **Observability**: Relevance score distribution.

### 11. Importance Engine (Custom)
- **Purpose**: Score intrinsic importance independent of user.
- **Responsibilities**: 
  - Evaluate dimensions: scale, scope, urgency, novelty, persistence, credibility, future significance.
  - Combine into importance score (0-1) with dimension breakdown.
- **Inputs**: ChangeSet, source trust, corroboration count.
- **Outputs**: ImportanceAssessment (score, dimensions[], confidence).
- **State**: None.
- **Depends**: Source Trust Engine (read).
- **Sync/Async**: Synchronous.
- **Failure**: Default medium importance.
- **Observability**: Importance score distribution.

### 12. Impact Analyzer (Custom)
- **Purpose**: Reason about consequences.
- **Responsibilities**: 
  - Identify direct effects (on primary entities/topics).
  - Infer indirect effects (ripple, secondary).
  - Label valence (positive/negative/mixed/uncertain).
  - Distinguish fact vs inference vs speculation, attach evidence.
- **Inputs**: ChangeSet, historical context, source facts.
- **Outputs**: ImpactAssessment (direct[], indirect[], timeHorizon, valence, confidence, evidenceTags[]).
- **State**: None.
- **Depends**: None (rule-based or lightweight LLM with grounding).
- **Sync/Async**: Synchronous.
- **Failure**: Return unknown impact, low confidence.
- **Observability**: Impact valence distribution.

### 13. Confidence & Source Trust Engine (Custom)
- **Purpose**: Aggregate uncertainty, assess source reliability.
- **Responsibilities**: 
  - Compute event confidence from source agreement, source trust, extraction confidence.
  - Update source trust based on corroboration vs. retraction (slow update).
  - Provide confidence levels (high/medium/low) with justification.
- **Inputs**: SourceTrust scores per source, extraction confidences, corroboration flag.
- **Outputs**: ConfidenceAssessment (level, justification[], sourceAgreement).
- **State**: Source trust table (persisted, slowly updated).
- **Depends**: Source trust memory.
- **Sync/Async**: Synchronous for assessment; async background trust updates.
- **Failure**: Default low confidence.
- **Observability**: Confidence levels, source trust updates.

### 14. Notification Decision Engine (Custom)
- **Purpose**: Decide user-facing disposition.
- **Responsibilities**: 
  - Compute priority = f(relevance, importance, confidence, urgency, novelty).
  - Apply suppression rules (cooldown, duplicate event, already notified).
  - Map priority to outcome: immediate, digest, monitor, suppress, ignore, request clarification.
- **Inputs**: RelevanceAssessment, ImportanceAssessment, ConfidenceAssessment, ChangeSet, user notification history, noise metrics.
- **Outputs**: NotificationDecision (outcome, priorityScore, rationale[]).
- **State**: Recent notification log (TTL) for suppression.
- **Depends**: None.
- **Sync/Async**: Synchronous.
- **Failure**: Default to digest or suppress.
- **Observability**: Decision outcomes histogram.

### 15. Notification Formatter & Digest Engine (Custom)
- **Purpose**: Produce user-visible output.
- **Responsibilities**: 
  - Notification Formatter: Turn NotificationDecision + ChangeSet + explanations into a user message (template + optional LLM polish).
  - Digest Engine: Periodically collect decisions marked for digest, deduplicate, rank, generate summary.
- **Inputs**: NotificationDecision, ChangeSet, Relevance/Importance/Impact/Confidence assessments, user preferences (explanation depth).
- **Outputs**: Notification (rendered text) or DigestItem.
- **State**: Digest buffer (time-based).
- **Depends**: Foundation templating/tools, optional LLM for polishing.
- **Sync/Async**: Notification: sync per decision; Digest: scheduled job.
- **Failure**: Fallback to plain text.
- **Observability**: Notification/digest latency, user feedback on clarity.

### 16. User Feedback Processor (Custom)
- **Purpose**: Incorporate explicit/implicit signals.
- **Responsibilities**: 
  - Map feedback types (open, dismiss, mark important, mute, follow, correction) to model updates.
  - Adjust user interest weights, mute lists, temporary interests.
  - Log feedback for audit.
- **Inputs**: FeedbackEvent (userId, targetId, type, comment, timestamp).
- **Outputs**: Updated user interest model (persisted).
- **State**: User interest model (persisted).
- **Depends**: None.
- **Sync/Async**: Async (fire-and-forget).
- **Failure**: Log error, retry.
- **Observability**: Feedback processing latency.

### 17. Awareness Memory Manager (Custom)
- **Purpose**: Centralize persistence of awareness-specific data.
- **Responsibilities**: 
  - Store/retrieve events, entities, topics, user interests, source trust, feedback, notification logs.
  - Handle TTL-based purging of transient state (dedup windows).
  - Provide query interfaces for components.
- **Inputs**: Various CRUD requests from components.
- **Outputs**: Stored data or query results.
- **State**: Persistent via foundation storage.
- **Depends**: Foundation storage abstraction (KV/doc).
- **Sync/Async**: Mix (reads sync, writes async with retry).
- **Failure**: Queue writes, degrade to caching if store unavailable.
- **Observability**: Store operation latency, error rates.

### 18. Observability & Audit Layer (Custom + Foundation)
- **Purpose**: Emit metrics, logs, traces for debugging and compliance.
- **Responsibilities**: 
  - Instrument component boundaries (latency, error rates).
  - Log key decisions (why notified/not notified) with provenance.
  - Emit audit trail for GDPR/user request.
- **Inputs**: Events from components.
- **Outputs**: Metrics to foundation observability, logs to foundation logging.
- **State**: None.
- **Depends**: Foundation metrics/logging/tracing.
- **Sync/Async**: Sync for logs, async for metrics batching.
- **Failure**: Drop logs if foundation unavailable, buffer briefly.
- **Observability**: Self-monitoring.

## Section 5 — Data Flow (Example Item)

1. **Source**: RSS feed item fetched by Source Connector → RawBlob + metadata.
2. **Ingestion**: RawBlob → InformationItem (UUID, source ID, fetch time, raw content hash).
3. **Normalization**: Strip HTML, detect language → NormalizedInformationItem.
4. **Provenance**: Attach source trust, fetch time → ProvenancedInformationItem.
5. **Deduplication**: Hash/similarity check → UniqueInformationItem or drop.
6. **Event/Entity Analysis**: NER, linking, fact extraction → AnalyzedItem (entities, topics, facts).
7. **Event Store**: Lookup/create event; attach item; update state summary.
8. **Entity Store**: Update entity record, aliases, attributes.
9. **Change Detector**: Compare AnalyzedItem to prior event/entity state → ChangeSet (delta, type).
10. **Historical Context**: Query prior events on same entities → ContextBundle.
11. **Relevance Engine**: Match to user interests → RelevanceAssessment.
12. **Importance Engine**: Score dimensions → ImportanceAssessment.
13. **Impact Analyzer**: Derive effects → ImpactAssessment.
14. **Confidence Engine**: Aggregate uncertainty → ConfidenceAssessment.
15. **Notification Decision**: Compute priority, apply suppression → NotificationDecision.
16. **If notify**: Notification Formatter → user message via foundation tool.
    **If digest**: Add to buffer; Digest Engine periodically → DigestItem.
17. **Feedback**: User actions → FeedbackProcessor → update interest model.
18. **Memory**: All relevant stores updated asynchronously.
19. **Observability**: Metrics/logs emitted at each stage.

## Section 6 — Control Flow

- **Scheduled Polling**: Source Connectors run per source schedule (foundation scheduler).
- **Event-Driven**: When a UniqueInformationItem emerges, it triggers synchronous pipeline through analysis to decision.
- **Background Jobs**: Digest generation runs on fixed interval; source trust updates run low-frequency batch.
- **User Requests**: Explicit fetch of digest or interest update triggers synchronous read path.
- **Feedback Handling**: Async processing of user actions.
- **Retries**: Foundation tool calls retry per foundation policy; awareness writes retry with exponential backoff.
- **Idempotency**: All storage operations use UUIDs; writes are idempotent (update by ID).

## Section 7 — Event Lifecycle

- **DISCOVERED**: First UniqueInformationItem creates Event record.
- **EXTRACTED**: Entities/facts/topics pulled.
- **RESOLVED**: Entities linked to canonical IDs.
- **CLASSIFIED**: Event type assigned (announcement, release, etc.).
- **CORROBORATED**: Multiple independent sources increase confidence.
- **ACTIVE**: Event is open for updates; change detection runs on new items.
- **UPDATED**: New ChangeSet applied; event state summary advanced; timeline entry added.
- **RESOLVED** (terminal): No significant updates for timeout period; event moved to archive.
- **CORRECTION**: If new item contradicts prior facts, change type = correction; confidence adjusted.
- **Contradictory Information**: Stored as conflicting evidence; confidence lowered; may trigger clarification request.

## Section 8 — Entity Lifecycle

- **DISCOVERED**: Entity string first seen.
- **CANDIDATE**: Appears in analyzed items, not yet linked.
- **RESOLVED**: Linked to canonical KB entry or assigned new canonical ID.
- **TRACKED**: Stored in Entity Store with attributes, aliases, related entities.
- **UPDATED**: Attributes changed via ChangeSet.
- **ARCHIVED**: No activity for long period; moved to cold storage.

## Section 9 — Change Detection Architecture

- **Inputs**: Current AnalyzedItem, prior Event state (summary + timeline), prior Entity states.
- **Processing**: 
  - Align facts by subject/predicate; detect new/changed/removed.
  - Compare entity attribute values.
  - Determine change type based on patterns (e.g., new causal fact → escalation).
- **Outputs**: ChangeSet with delta natural language, change type enum, confidence.
- **Duplicate Prevention**: Only consider changes that modify the canonical state; identical re-publications yield empty delta → treated as duplicate by dedup earlier.
- **Confidence**: Based on source trust of new item and extent of corroboration.

## Section 10 — Relevance Architecture

- **Inputs**: Event/entities/topics, user interest model (weights, explicit flags, temporal scopes, muted lists), historical relevance (user opened/dismissed similar).
- **Processing**: 
  - Score = Σ (weight * match strength) for matched interests.
  - Apply decay for temporary interests.
  - Zero if muted.
  - Factors list explains contributions.
- **Output**: RelevanceAssessment (score 0-1, factors, confidence).
- **Contract**: Allows swapping scoring function without changing callers.

## Section 11 — Importance Architecture

- **Inputs**: Event scale (entity count, geographic spread), scope (sector/countries), urgency (time-sensitive actions), novelty (unprecedented), persistence (effect duration), source credibility, potential future significance (expert signals).
- **Processing**: Weighted combination or rule-based scoring per dimension.
- **Output**: ImportanceAssessment (score 0-1, dimension breakdown, confidence).
- **Contract**: Enables independent tuning.

## Section 12 — Impact Architecture

- **Inputs**: ChangeSet, historical context, source facts.
- **Processing**: 
  - Direct: Extract affected entities/actions from change.
  - Indirect: Use simple causal chains (e.g., product release → competitor stock impact) via knowledge graphs or LLM with grounding.
  - Valence: Sentiment of facts + explicit positive/negative cues.
  - Label each claim as fact (direct observation), inference (logical deduction), speculation (explicit uncertainty).
- **Output**: ImpactAssessment with arrays of direct/indirect effects, time horizon, valence, evidence tags, confidence.
- **Contract**: Keeps fact/inference/speculation separate.

## Section 13 — Source Trust + Confidence Architecture

- **Source Trust**: 
  - Identity: source ID, type (news, blog, govt, social), reliability score (0-1), last success.
  - Updated slowly: +delta on corroboration, -delta on retraction/failure.
- **Event Confidence**: 
  - Based on: source trust average, number of independent sources, extraction confidence, corroboration.
  - Levels: high (≥0.8), medium (0.5-0.8), low (<0.5) with justification text.
- **Model Confidence**: 
  - From foundation NER/LLM outputs (e.g., logits) mapped to 0-1.
- **Architecture**: Keeps three concepts separate; combines only for final notification decision.

## Section 14 — Memory Architecture

| Memory Type          | Purpose                           | Lifetime       | Ownership       | Update Mechanism                     | Persistent? |
|----------------------|-----------------------------------|----------------|-----------------|--------------------------------------|-------------|
| User Preference      | Topics, weights, mute, temporary  | Long-term      | Custom          | Feedback processor, explicit sets    | Yes (foundation storage) |
| Entity Memory        | Canonical entities, aliases, attrs| Long-term      | Custom          | Entity store updates                 | Yes |
| Event Memory         | Events, state summary, timeline   | Long-term      | Custom          | Event store updates                  | Yes |
| Timeline Memory      | Ordered entries per event         | Long-term (part of Event) | Custom | Append on update | Yes |
| Fact Memory          | Extracted facts with confidence   | Medium-term (tied to event) | Custom | In event store | Yes |
| Notification History | Recent notifications/digests      | Short-term (TTL 7d) | Custom | Append on send | Yes (with TTL) |
| Feedback History     | User feedback events              | Long-term      | Custom          | Append on feedback                   | Yes |
| Audit/System         | Decision logs, metrics            | Long-term      | Custom/Foundation | Emit per decision | Yes (via foundation logging) |

## Section 15 — Temporal Architecture

- **Publication Time**: When source published item (from metadata).
- **Event Time**: When the real-world occurrence happened (extracted or inferred).
- **Update Time**: When system processed the item.
- **Observation Time**: Wall-clock time of ingestion.
- **Validity Period**: How long a fact is considered current (configurable per fact type).
- **Historical State**: Prior values stored in event/entity timelines.
- **Answering “What was known at time T?”**: Query event/entity state as of T via timeline replay.
- **Answering “What changed since previous observation?”**: Compare current state to state at last notification/digest timestamp for that user.

## Section 16 — Notification Decision Architecture

- **Pipeline**: 
  1. Base score = relevance * importance.
  2. Adjust by confidence multiplier (high=1.0, medium=0.7, low=0.4).
  3. Apply urgency boost if time-sensitive (from impact).
  4. Apply novelty penalty if user already notified recently for same event.
  5. Apply global noise dampener if system-wide notification rate exceeds threshold.
- **Outputs**: NotificationDecision enum {IMMEDIATE, DIGEST, MONITOR, SUPPRESS, IGNORE, REQUEST_CLARIFICATION}.
- **Contract**: Structured with score, thresholds used, rationale list.
- **Prevents**: Presentation layer from overriding decision.

## Section 17 — Notification / Digest Architecture

- **Notification Object**: 
  - eventId, timestamp, channel, priority, explanation (structured: what, changed, why matters, why to me, confidence, sources, outlook), acknowledged flag.
- **Digest Item**: 
  - eventId, position, explanation (shorter), link to full notification if expanded.
- **Explanation**: Always includes provenance (source IDs/links) and confidence level; separates fact/inference/speculation.
- **Decision vs Presentation**: Notification Decision only chooses outcome and priority; formatter decides exact wording (may use LLM for polish but must preserve factual constraints).

## Section 18 — Feedback Architecture

- **Explicit Feedback**: Open, dismiss, mark important, request more/less detail, follow/unfollow, mute, correction.
- **Implicit Feedback**: Dwell time, click-through (if available).
- **Processor**: 
  - Open/Dismiss → adjust relevance weight for matched interests (small delta).
  - Mark Important → boost importance weight temporarily.
  - Follow → add explicit interest with weight.
  - Mute → add to mute list or decay weight to zero.
  - Correction → flag item for review, possibly lower source trust.
- **Safety**: Updates are bounded; no autonomous model retraining without explicit approval; changes logged.

## Section 19 — Pipelines and Orchestration

- **Stages**: Ingestion → Normalization → Deduplication → Analysis → Change Detection → Relevance/Importance/Impact/Confidence → Decision → Formatting.
- **Execution Model**: 
  - Each unique item flows synchronously through the stages (non-blocking per stage but overall latency matters).
  - Foundation provides async tool calls; custom layer chains them via futures/promises.
  - Background jobs: Digest generation, source trust updates, memory maintenance.
- **Deterministic vs LLM**: 
  - Deterministic preferred for: normalization, dedup, entity linking (gazetteer), change detection (rule-based), scoring (if linear).
  - LLM-assisted for: fact extraction from noisy text, impact reasoning, explanation polishing (with grounding).
  - Avoid LLM for pure routing decisions.

## Section 20 — LLM vs Deterministic Processing

| Subsystem                | Processing Preference          | Rationale |
|--------------------------|--------------------------------|-----------|
| Normalization            | Deterministic                  | Well-defined rules |
| Deduplication            | Deterministic                  | Hash/similarity thresholds |
| Entity Resolution        | LLM-assisted (fallback to deterministic) | Ambiguous names need context |
| Event Extraction         | LLM-assisted                   | Need to understand relations |
| Classification (Topic/Event) | Deterministic (keyword) or LLM-assisted (zero-shot) | Depending on available taxonomy |
| Change Detection         | Deterministic                  | State diff is rule-based |
| Relevance                | Deterministic (weighted sum)   | Transparent, auditable |
| Importance               | Deterministic (rule-based)     | Explainable dimensions |
| Impact                   | LLM-assisted (with grounding)  | Requires causal reasoning |
| Confidence/Trust         | Deterministic (bayesian-ish)   | Simple aggregation |
| Notification Decision    | Deterministic                  | Clear thresholds |
| Formatting/Explanation   | LLM-assisted (template base)   | Natural language generation |

## Section 21 — Data Contracts (Conceptual)

**Source**: { id, name, type, reliabilityScore, lastFetch, metadata }
**InformationItem**: { id, contentRaw, contentNormalized, sourceId, fetchedAt, hash, modality, language }
**ProvenancedInformationItem**: extends InformationItem { sourceTrust, initialConfidence }
**AnalyzedItem**: extends ProvenancedInformationItem { entities[{id, name, type, confidence}], topics[], facts[{statement, confidence, evidence[]}], sourceLinks[] }
**Event**: { id, type, firstSeen, lastUpdated, stateSummary{}, entities[], topics[], timeline[{timestamp, description, sourceIds, confidence}], status }
**Entity**: { id, canonicalName, aliases[], entityType, attributes{}, relatedEntities[], firstSeen, lastSeen }
**ChangeSet**: { eventId, timestamp, deltaDescription, changeType, confidence, affectedEntities[], affectedTopics[] }
**RelevanceAssessment**: { itemId, userId, score (0-1), factors[{name, weight, contribution}], confidence }
**ImportanceAssessment**: { itemId, score (0-1), dimensions[{name, weight, contribution}], confidence }
**ImpactAssessment**: { itemId, directEffects[], indirectEffects[], timeHorizon, valence, confidence, evidenceTags[] }
**ConfidenceAssessment**: { itemId, level (high/medium/low), justification[], sourceAgreement, extractionConfidenceAvg }
**NotificationDecision**: { itemId, userId, outcome, priorityScore, rationale[], suppressedReason? }
**Notification**: { id, eventId, userId, timestamp, channel, explanationStructured, acknowledged, feedbackRequests[] }
**DigestItem**: { digestId, eventOrUpdateId, position, explanationShort }
**Digest**: { id, userId, startTime, endTime, items[], generatedAt }
**UserFeedback**: { id, userId, targetId, type, comment, timestamp }
**AwarenessMemoryRecord**: generic KV for persistence.

## Section 22 — Contract Examples

```
{
  "event_id": "evt_9f3a2b",
  "type": "product_release",
  "first_observed_at": "2026-09-12T08:00:00Z",
  "last_updated_at": "2026-09-14T14:30:00Z",
  "state_summary": {
    "product": "QuantumChip X1",
    "version": "1.0",
    "release_date": "2026-09-14"
  },
  "entities": [
    {"id": "ent_acme", "name": "Acme Corp", "type": "Company"},
    {"id": "ent_qc", "name": "QuantumChip X1", "type": "Product"}
  ],
  "topics": ["quantum_computing", "semiconductors"],
  "timeline": [
    {"timestamp": "2026-09-12T08:00:00Z", "description": "Initial leak", "sourceIds": ["src_techblog"], "confidence": 0.6},
    {"timestamp": "2026-09-14T14:30:00Z", "description": "Official release", "sourceIds": ["src_acme_pr", "src_reuters"], "confidence": 0.9}
  ],
  "confidence": 0.85,
  "change_summary": "Official release confirmed with specs"
}
```

## Section 23 — Interface Contracts

### Ingestion → Normalization
- **Input**: InformationItem
- **Output**: NormalizedInformationItem
- **Errors**: None (pass-through on failure)
- **Sync**: Yes

### Normalization → Provenance
- **Input**: NormalizedInformationItem
- **Output**: ProvenancedInformationItem
- **Errors**: None

### Provenance → Deduplication
- **Input**: ProvenancedInformationItem
- **Output**: UniqueInformationItem or None
- **Errors**: Log, treat as unique

### Deduplication → Event/Entity Analysis
- **Input**: UniqueInformationItem
- **Output**: AnalyzedItem
- **Errors**: Return minimal extraction, flag low confidence

### Analysis → Change Detection
- **Input**: AnalyzedItem, related eventId/entityIds
- **Output**: ChangeSet
- **Errors**: Return empty ChangeSet, low confidence

### Change Detection → Relevance
- **Input**: ChangeSet, user interest model
- **Output**: RelevanceAssessment
- **Errors**: Default low relevance

### Relevance → Importance
- **Input**: ChangeSet (shared context)
- **Output**: ImportanceAssessment
- **Errors**: Default medium importance

### Importance → Impact
- **Input**: ChangeSet, historical context
- **Output**: ImpactAssessment
- **Errors**: Unknown impact

### Impact → Confidence
- **Input**: ImpactAssessment, source trust, corroboration
- **Output**: ConfidenceAssessment
- **Errors**: Low confidence

### Confidence → Notification Decision
- **Input**: RelevanceAssessment, ImportanceAssessment, ConfidenceAssessment, ChangeSet
- **Output**: NotificationDecision
- **Errors**: Default to SUPPRESS

### Notification Decision → Formatter
- **Input**: NotificationDecision + all assessments
- **Output**: Notification (or DigestItem)
- **Errors**: Fallback plain text

### Feedback → User Interest Model
- **Input**: FeedbackEvent
- **Output**: Updated interest model (persisted)
- **Errors**: Log, retry

## Section 24 — OpenJarvis Integration Boundary

### OpenJarvis Provides
- **Agent Runtime**: Job scheduling, lifecycle, supervision.
- **Model Abstraction**: Unified LLM invoke interface (text, JSON mode).
- **Tool Framework**: Web fetch, scraper, API caller, RAG query, memory put/get.
- **Memory Infrastructure**: KV store, blob storage, basic query.
- **Storage**: Persistent volumes, backup.
- **Security**: Sandboxing, secret management, request signing.
- **Observability**: Metrics (Prometheus), logging (structured), tracing.
- **Extension Points**: Registries for tools, memory adapters, schedulers.

### Custom Layer Provides
- **Awareness-Specific Connectors**: Wraps foundation tools to emit ProvenancedInformationItem.
- **Awareness Memory Schemas**: Event, entity, change, user interest models stored via foundation KV.
- **Custom Algorithms**: All pipelines and engines above.
- **Decision Logic**: Notification/digest outcome.
- **Feedback Handling**: Updates to user model.
- **Observability Hooks**: Awareness-specific metrics (latency per stage, decision distribution).

### Shared Interface
- **Memory Contract**: Custom layer reads/writes JSON blobs via foundation KV using defined keys (e.g., `awareness:event:{id}`).
- **Tool Contract**: Custom layer invokes foundation `web_fetch`, `llm_invoke`, `rag_query` as needed.
- **Scheduler Contract**: Custom layer registers jobs (ingestion per source, digest generation, trust update) via foundation scheduler.
- **Observability Contract**: Custom layer emits counters/histograms to foundation metrics (e.g., `awareness_ingestion_latency`).

### What We Do NOT Redesign
- Foundation’s internal LLM prompting, tool execution sandbox, core KV API.

### What We Assume (Pending Verification)
- Foundation provides a reliable at-least-once delivery for tool calls.
- Foundation memory supports TTL or we implement our own expiry layer.
- Foundation scheduler supports cron-like and interval jobs.

## Section 25 — Security Architecture

- **Trust Boundary**: External source → [Untrusted Data] → Sanitization → [Trust Boundary] → Internal Processing.
- **Untrusted Data Handling**: 
  - All fetched content treated as raw bytes; no automatic execution.
  - HTML stripped before NER; script tags removed.
  - URLs validated against allow/deny lists (configurable).
- **Prompt Injection**: 
  - LLM prompts built with strict templating; user-controlled content never placed in instruction slot.
  - Use foundation’s tool-use constrained generation if available.
  - Sanitize outputs: strip JSON/code fences unless expected.
- **Source Manipulation**: 
  - Source trust decay on low-quality or conflicting reports.
  - Rate-limit per source to prevent flooding.
- **Data Contamination**: 
  - Validation schemas on stored objects; quarantine malformed entries.
- **Privacy**: 
  - No storage of raw PII unless explicitly user-provided interest.
  - Audit log excludes full content, stores hashes.
- **Access Control**: 
  - Memory keys namespaced per user; foundation enforces separation if multi-user.
- **Audit Logging**: 
  - Every notification/digest decision logged with rationale, scores, source IDs.
  - Immutable append-only log via foundation.

## Section 26 — Observability Architecture

- **Metrics** (exported via foundation):
  - `awareness_ingestion_total`, `awareness_ingestion_latency`
  - `awareness_dedup_duplicates_total`
  - `awareness_event_created_total`, `awareness_event_updated_total`
  - `awareness_change_detected_total` (by changeType)
  - `awareness_relevance_score` (histogram)
  - `awareness_importance_score` (histogram)
  - `awareness_confidence_level` (counts by high/med/low)
  - `awareness_notification_outcome_total` (by outcome)
  - `awareness_digest_generated_total`
  - `awareness_feedback_processed_total`
  - `awareness_component_errors_total`
- **Logs**:
  - Structured log per pipeline stage with trace ID.
  - Decision log: `{eventId, userId, decision, priority, rationale, sources, confidence}`.
- **Tracing**:
  - Foundation trace ID propagated through custom async chains.
- **Debug Queries**:
  - API to replay a historical item stream and see decisions.
  - Endpoint to query why a particular event was/not notified.

## Section 27 — Failure and Recovery Architecture

- **Source Unavailable**: 
  - Retry with backoff (foundation). After max retries, skip, increment error metric, continue; event may be delayed.
- **Malformed Input**: 
  - Log error, skip item, emit metric; pipeline continues.
- **Model Unavailable**: 
  - Fallback to deterministic heuristics (if exists) or skip LLM-assisted stage with low confidence; alert if persistent.
- **Timeout**: 
  - Foundation timeout propagation; treat as error, retry or degrade.
- **Duplicate Uncertainty**: 
  - If dedup uncertain (low similarity), let through; change detection will likely yield no change → suppressed later.
- **Entity Resolution Uncertainty**: 
  - Keep provisional entity ID, flag for review, low confidence propagation.
- **Conflicting Sources**: 
  - Lower confidence, record conflicting evidence in event timeline, may trigger clarification request.
- **Database Failure**: 
  - Queue writes in memory with periodic retry; if persistent, switch to read-only mode and alert.
- **Notification Failure**: 
  - Retry with exponential backoff; after limit, store for next digest and notify user of delivery issue via alternative channel if possible.
- **Degraded Mode**: 
  - If core pipeline fails, system continues to ingest and store raw items; decision engine defaults to suppress/digest until recovery.

## Section 28 — MVP Architecture

- **Components Required**: 
  - Source Connectors (RSS/HTTP wrappers)
  - Ingestion, Normalization, Provenance (simple passthrough)
  - Deduplication (hash + title similarity)
  - Event/Entity Analysis (foundation NER + gazetteer linking)
  - Event & Entity Stores (foundation KV)
  - Change Detector (state diff of extracted facts)
  - Relevance Engine (weighted keyword match to user interests)
  - Importance Engine (rule-based: mention count * source trust)
  - Confidence Engine (average source trust)
  - Notification Decision (thresholds on product of relevance, importance, confidence)
  - Notification Formatter (template-based)
  - Feedback Processor (adjust weights)
  - Awareness Memory Manager (foundation KV wrappers)
  - Observability (basic metrics)
- **Simplifiable/Deferable**: 
  - Historical Context Engine (skip for MVP)
  - Impact Analyzer (return neutral)
  - Positive/Negative Analysis (skip)
  - Source Trust dynamic updates (use static config)
  - Digest Engine (optional; can start with immediate only)
  - Complex event typing (use generic)
- **OpenJarvis Reused**: 
  - Runtime, model abstraction (LLM for NER if needed), tools (web, KV store), scheduler, security, observability.
- **Custom Code**: 
  - All pipeline glue, schemas, decision logic, memory adapters, feedback.

## Section 29 — First Vertical Slice

- **Goal**: End-to-end path from one trusted RSS feed to user notification with explanation.
- **Contents**: 
  - Single RSS connector (foundation web tool).
  - Ingestion → Normalization → Provenance (source trust from config).
  - Deduplication (hash).
  - Analysis: foundation NER + manual gazetteer for companies/tech.
  - Event creation: store simple event with {product, version, timestamp}.
  - Change Detection: compare version string or fact presence.
  - Relevance: if entity in user interest list → score 1.0 else 0.
  - Importance: log(mention count) * source trust.
  - Confidence: source trust.
  - Decision: if (relevance * importance * confidence) > threshold → immediate.
  - Formatter: template: “[Entity] released [product]. Why it matters: [importance reason]. Why to you: [interest match]. Confidence: [level]. Sources: [list].”
  - Persistence: store notification, update entity state.
  - Feedback: log open/dismiss.
- **Success Criteria**: 
  - System runs continuously.
  - Generates non-spam notifications for true developments.
  - Explanations are clear and grounded.

## Section 30 — Scaling Path

- **MVP → Early Prototype**: 
  - Add multiple source types (news APIs, web scrapers).
  - Improve entity linking (foundation RAG + Wikidata).
  - Add basic historical context (previous version).
- → **Multi-source System**: 
  - Tune dedup window, introduce near-dup (MinHash).
  - Add event clustering (shared entities + time).
- → **Richer Event History**: 
  - Build timelines, enable trend detection (sliding window counts).
- → **Advanced Relevance**: 
  - Learn weights from feedback (bounded linear model).
  - Add temporal interest decay.
- → **Larger Volume**: 
  - Introduce message queue (foundation if available) between ingestion and analysis to smooth bursts.
  - Shard event store by entity hash if needed.
- → **Future**: 
  - Causal impact graphs, multi-hop reasoning, proactive source discovery (guarded).

## Section 31 — Testability

- **Unit Boundaries**: Each engine (relevance, importance, change detector) receives pure inputs and returns outputs; can be tested with fixtures.
- **Contract Tests**: Mock foundation tools; verify input/output schemas.
- **Integration Tests**: Spin up foundation with in-memory KV and HTTP mock; feed synthetic RSS items; assert store updates and notifications.
- **End-to-End**: Playback recorded historical feeds; compare notification sequence to expected (based on ground truth annotations).
- **Deterministic Fixtures**: Use fixed timestamps, seeded randomness for any probabilistic steps.
- **Replay Testing**: Foundation scheduler can be overridden to run jobs instantly; we can feed a batch of items and verify state.

## Section 32 — Evaluation Architecture

- **Dimensions to Measure**:
  - Event Detection: Precision/recall against human-annotated corpus.
  - Duplicate Suppression: Ratio of unique events to raw items.
  - Change Detection: Accuracy of detecting true state changes vs. noise.
  - Relevance: Precision@k and recall for user-interest items (via user studies).
  - Importance: Correlation with expert significance scores.
  - Impact: Correctness of direct effect extraction; calibration of confidence.
  - Confidence: Calibration (observed accuracy vs. confidence level).
  - Notification Precision: Fraction of notifications deemed relevant/important by user.
  - Notification Noise: Notifications per relevant event (goal <1.2).
  - Missed Important Events: Rate of high-importance, high-relevance items not surfaced.
  - User Satisfaction: Survey on usefulness and explanation quality.
  - Explanation Quality: Human rating of clarity, grounding, and actionability.
- **Evaluation Harness**: 
  - Benchmark suite of feeds with known ground truth.
  - Can be run nightly against main branch.

## Section 33 — Architectural Decisions

| Decision | Reason | Alternatives Considered | Trade-offs | Status |
|----------|--------|-------------------------|------------|--------|
| Pipeline-style synchronous flow per item | Simplicity, ease of reasoning, low latency for urgent items | Event-driven actors, message queues | Queues add complexity; sync is fine for moderate throughput | DECIDED |
| Deterministic scoring for relevance/importance | Transparency, auditability, easy tuning | Learned neural models | Less adaptive but controllable; can add lightweight learner later | DECIDED |
| Separate source trust and model confidence | Avoid conflating data quality with model uncertainty | Single combined score | Clearer explanations; slight extra computation | DECIDED |
| Notification decision precedes formatting | Guarantees low-noise property; presentation cannot override | Let formatter decide importance | Clean separation; formatter focuses on clarity | DECIDED |
| Use foundation tools for web/LLM/KV | Leverage tested, secure primitives | Reimplement in custom layer | Reduces attack surface; relies on foundation quality | DECIDED |
| Store awareness data in foundation KV with namespaces | Avoid custom storage layer; benefit from foundation backup/security | Separate DB | Simpler ops; dependent on foundation storage performance | DECIDED |
| Digest generation as scheduled background job | Predictable resource usage; avoids per-item overhead | Real-time per-user digests | Slightly stale but acceptable for awareness | DECIDED |
| Feedback updates bounded, no online retraining | Safety, explainability, compliance | Continuous online learning | Prevents drift; may need periodic manual tuning | DECIDED |
| Change detection based on state delta (facts/entities) | Captures meaningful novelty; suppresses rehash | Treat every new item as potential event | Reduces false positives; requires good state model | DECIDED |

## Section 34 — Open Questions

- **Exact Source Mix**: Which source types (news, blogs, govt, social, academic) to include initially? Depends on user surveys and availability.
- **Notification Channels**: Desktop toast, email, mobile push, chat? Determined by deployment environment and user preference.
- **Numerical Thresholds**: Priority cutoffs for immediate vs digest, noise suppression levels, cooldown durations. To be tuned during MVP evaluation with user feedback.
- **Entity Resolution Mechanism**: Foundation NER + gazetteer vs. full LLM disambiguation vs. external KB (Wikidata). Will prototype both and measure latency/accuracy.
- **Model Selection for LLM-Assisted Tasks**: Which foundation LLMs to use for fact extraction, impact reasoning? Will benchmark against open‑source alternatives.
- **Ingestion Frequency**: How often to poll each source? Balanced by freshness needs vs. load; to be derived from source SLA.
- **Significant Change Thresholds**: What delta in state warrants a notification? Will define via user studies on scenarios.
- **Temporal Interest Decay Formula**: How fast do temporary interests fade? Await data on user behavior.
- **Deployment Model**: Single-tenant vs multi-tenant? Affects memory namespacing and scaling.

## Section 35 — Implementation Readiness

| Area | Status | Notes |
|------|--------|-------|
| Architecture | READY | High-level design complete, grounded in requirements. |
| Component Boundaries | READY | Clear separation of concerns defined. |
| Data Contracts | READY | Conceptual schemas provided; can be mapped to implementation. |
| OpenJarvis Integration Boundaries | PROVISIONAL | Based on supplied documents; Phase 7 must verify actual foundation APIs (tool names, memory KV interface, scheduler registries). |
| MVP | READY | Minimal set identified; can be built incrementally. |
| First Vertical Slice | READY | End-to-end path scoped. |
| Testing Strategy | READY | Unit, contract, integration, e2e approaches outlined. |
| Security Boundaries | READY | Trust boundary and mitigations defined. |
| Observability | READY | Metrics and logging plan specified. |
| Deployment Assumptions | NOT READY | Requires Phase 7 to confirm foundation’s deployment model (container, K8s, bare metal) and resource limits. |

## Section 36 — Phase 7 Handoff

Phase 7 must verify the following assumptions against the actual OpenJarvis codebase:

1. **Tool Registry**: Names and signatures for `web_fetch`, `llm_invoke`, `rag_query`, `memory_put`, `memory_get`, `memory_delete`, `memory_list`.
2. **Memory Abstraction**: Whether foundation provides a KV store with TTL or binary blobs; key naming constraints.
3. **Scheduler**: How to register cron/interval jobs; ability to pass payload.
4. **Observability**: Metrics (counter/histogram) and logging APIs (structured JSON).
5. **Security Sandbox**: Constraints on outbound network, file system, subprocess spawning.
6. **Extension Points**: Registries for custom tools, memory adapters, or middleware that the custom layer can hook into.
7. **Model Abstraction**: Supported modalities (text, JSON) and any built-in prompt templating utilities.
8. **Error Handling**: Foundation tool retry policies, timeout behavior, error propagation.
9. **Configuration**: How custom layer provides its own configs (JSON/YAML) and accesses secrets.
10. **Concurrency Guarantees**: Whether foundation tools are safe to call concurrently from multiple fibers.

**PHASE 7 MUST VERIFY THESE ARCHITECTURAL ASSUMPTIONS AGAINST THE ACTUAL OPENJARVIS CODEBASE.**

--- 
*End of Custom Awareness Intelligence Architecture Document*