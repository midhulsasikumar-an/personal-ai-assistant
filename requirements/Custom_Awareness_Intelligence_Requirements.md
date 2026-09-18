# Custom Awareness Intelligence Requirements

## Section 1 — System Mission
The awareness intelligence continuously monitors relevant information sources to detect world developments, assesses their personal relevance and importance, estimates potential impact, and notifies the user only when the development is new, significant, and relevant, providing clear explanations and preserving provenance. It distinguishes between knowing what happened, understanding what changed, why it matters, whether it matters to the user, and whether the user should be interrupted.

## Section 2 — Core Functional Requirements
1. **Source ingestion** – Collect data from configured feeds, APIs, web pages, and documents.
2. **Information normalization** – Convert diverse formats into a common internal representation.
3. **Source provenance** – Record origin, timestamp, and retrieval metadata for each item.
4. **Freshness detection** – Identify newly published or updated items.
5. **Duplicate detection** – Exact‑match suppression of previously seen items.
6. **Near‑duplicate detection** – Cluster semantically similar items representing the same underlying event.
7. **Event detection** – Recognize when a set of items describes a noteworthy occurrence.
8. **Event extraction** – Pull structured facts (who, what, when, where, why) from items.
9. **Entity extraction** – Identify named entities (persons, organizations, locations, etc.).
10. **Entity resolution** – Link extracted entities to canonical identifiers in a knowledge base.
11. **Topic classification** – Assign items/events to predefined or learned topics.
12. **Event classification** – Label events by type (announcement, release, policy change, etc.).
13. **Change detection** – Determine what is new compared to previously known state.
14. **Trend detection** – Identify rising or falling patterns over time.
15. **Fact extraction** – Isolate verifiable statements with confidence scores.
16. **Temporal information** – Capture timestamps, durations, and temporal relations.
17. **Historical context** – Retrieve prior events and states for the same entities/topics.
18. **User‑interest matching** – Compare extracted entities/topics against the user profile.
19. **Personal relevance analysis** – Estimate how pertinent an event is to the user.
20. **Importance estimation** – Assess the intrinsic significance of an event independent of the user.
21. **Impact analysis** – Reason about direct and indirect consequences.
22. **Positive/negative development analysis** – Determine valence and justify with evidence.
23. **Confidence estimation** – Quantify uncertainty in extraction, resolution, and assessment.
24. **Source trust assessment** – Evaluate reliability of each source based on historical accuracy.
25. **Uncertainty representation** – Propagate and display confidence levels.
26. **Priority calculation** – Combine relevance, importance, urgency, and confidence into a rank.
27. **Notification decision** – Choose immediate, delayed, digest, or suppress based on priority and noise controls.
28. **Digest generation** – Periodically compile a summary of notable developments since last digest.
29. **Explanation generation** – Produce human‑readable justification for each notification/digest item.
30. **User feedback** – Capture explicit signals (open, dismiss, mark important, etc.).
31. **Feedback‑based adaptation** – Adjust interest weights, thresholds, and source trust safely.
32. **Memory/persistence requirements** – Store short‑term state and long‑term awareness memory.
33. **Auditability** – Log decisions, sources, and rationale for reproducibility.
34. **Failure handling** – Degrade gracefully when sources, models, or services are unavailable.

## Section 3 — Event Understanding
An event is a coherent set of information items describing a change in the world that persists beyond a single publication. The system must distinguish:
- New event vs. continuation/update/correction/reversal/escalation/de‑escalation/milestone/announcement/release/discovery/policy change/market/tech/scientific/org change/emerging trend/positive/negative development.
Grouping criteria: temporal proximity, shared entities, overlapping factual claims, and semantic similarity.

## Section 4 — Change Detection
Change detection answers “What is actually new?” by comparing the extracted state of an event/topic/entity against its previously stored state. It must recognize:
- Previously unknown event
- New information about an existing event
- Shift in significance
- New entities involved
- New consequences/decisions/evidence
- Status change
- Corrections/reversals
- Escalation/resolution
It must suppress notifications for items that contain no novel factual change.

## Section 5 — User Interest Model
The user profile shall contain (minimal set):
- Explicit interests (topics, technologies, companies, organizations, people)
- Geographic focus
- Professional/educational domains
- Long‑term watchlists
- Temporary interests
- Muted topics/entities
- Notification sensitivity (frequency, urgency preference)
- Source preferences/trust levels
- Feedback history (relevance marks, importance marks)
The system stores only what is needed to improve relevance; no unnecessary personal data.

## Section 6 — Personal Relevance
Relevance is the degree to which an development affects the user’s goals, concerns, or interests. Factors:
- Direct match to explicit interests
- Indirect relevance via related topics/entities
- Historical relevance (user previously engaged)
- Potential future impact on user
- Uncertainty (when confidence low)
Relevance is separate from importance; an event can be globally important but personally irrelevant, or vice versa.

## Section 7 — Importance
Importance reflects the intrinsic magnitude of a development irrespective of the user. Dimensions:
- Scale (number of people/entities affected)
- Scope (geographic, institutional)
- Urgency (time‑sensitivity)
- Novelty (how unprecedented)
- Persistence (how long effects may last)
- Credibility of sources
- Potential future significance
- Rate of change
The system must be able to rank events by importance without relying on personal relevance.

## Section 8 — Impact Analysis
Impact analysis reasons about consequences:
- Direct impact (immediate effects on primary entities)
- Indirect impact (secondary effects, ripple effects)
- Short‑term vs. long‑term impact
- Positive, negative, or mixed impact
- Uncertain/speculative impact (clearly labeled)
- Affected entities/topics
- Possible relevance to user
Fact, inference, and speculation must be distinctly marked; speculation never presented as fact.

## Section 9 — Positive and Negative Developments
The system must detect and explain:
- Positive developments (improvements, opportunities)
- Negative developments (declines, risks)
- Mixed developments (both benefits and drawbacks)
- Risks, opportunities, unintended consequences
- Worsening/improving conditions
- Controversial developments
Evidence must be cited; sensationalism avoided.

## Section 10 — Source Trust and Confidence
Requirements:
- Record source identity and metadata
- Assess source reliability based on historical accuracy and editorial standards
- Distinguish primary vs. secondary sources
- Track corroboration across independent sources
- Represent confidence levels (high/medium/low) with justification
- Communicate uncertainty (e.g., “preliminary”, “disputed”)
- Detect and flag stale or corrected information
No exact scoring formula mandated; qualitative levels suffice.

## Section 11 — Temporal / Historical Understanding
The system shall maintain timelines for events and entities, enabling:
- Comparison of current state to prior state
- Identification of recurring patterns
- Provision of historical context when it materially changes interpretation
- Surfacing of relevant prior announcements, milestones, or decisions
Historical context is included when it alters the perceived significance or impact.

## Section 12 — Entity Understanding
Track canonical entities with:
- Official identifiers and aliases
- Relationships (ownership, subsidiary, partnership, etc.)
- Current state (status, leadership, etc.)
- Historical state changes
- Associated events and topics
Entity tracking is bounded to those appearing in relevance‑filtered streams; unbounded growth is not required.

## Section 13 — Notification Intelligence
Decision outcomes:
- Immediate high‑priority notification
- Normal notification
- Include in next digest
- Monitor silently (no user‑facing output)
- Ignore
- Request clarification or wait for corroboration
Factors:
- Priority score (relevance × importance × urgency × confidence)
- Recent notification history for same event/topic (suppression)
- User‑defined frequency caps
- Whether user already knows (via explicit acknowledgment)
- Time‑sensitivity of required awareness
Optimization goal: minimize noise while ensuring truly important developments surface.

## Section 14 — Notification Explanation
Each notification should answer, as appropriate:
1. What happened? (concise fact)
2. What changed? (delta from prior known state)
3. Why does it matter? (importance/impact)
4. Why does it matter to me? (relevance justification)
5. How confident are we? (confidence level + brief rationale)
6. What is the evidence/source? (citations)
7. What could happen next? (short‑term outlook, labeled speculation)
Lower‑priority digest items may omit some elements; immediate high‑priority items must include all.

## Section 15 — Digests
Digests shall:
- Aggregate developments since last digest
- Prioritize by priority score, novelty, and relevance
- Cluster updates to same event (show only latest delta)
- Highlight emerging trends and unresolved stories
- Include brief historical context when it changes interpretation
- Exclude items already notified individually unless significant change occurred
- Provide explanation for each item (as per Section 14)

## Section 16 — Memory Requirements
Short‑term state (volatile, refreshed each cycle):
- Recently seen information items (for dedup)
- Active event states
- Pending notifications
Long‑term awareness memory (persistent):
- Canonical entities with aliases and relationships
- Event timelines and state history
- Topic prevalence and trend data
- User interest model and feedback history
- Source trust metrics
- Notification and digest logs
Separate stores for fact memory, event memory, entity memory, user preference memory, and system/audit memory.

## Section 17 — Learning from Feedback
Safe adaptation mechanisms:
- Increase/decrease weight of matched topics/entities based on relevance feedback
- Adjust source trust up/down based on corroboration vs. retraction
- Temporarily boost interest for explicitly followed items
- Decay temporary interests over time
- Never alter core extraction/models without explicit validation
- Log all adaptations for auditability
Feedback types: open, dismiss, mark important, request more/less detail, follow/unfollow, mute, correction.

## Section 18 — Anti‑Noise Requirements
- Duplicate suppression (exact and near‑duplicate)
- Notification cooldown per event/entity
- Story clustering with update thresholds (only notify if delta exceeds significance threshold)
- Significplitude thresholds for notifications vs. digest inclusion
- Confidence thresholds (low‑confidence items default to digest or ignore)
- Fallback to digest when notification volume spikes
- Repeated‑event suppression (no re‑notify for same state without material change)
- User‑specific frequency caps
- Diminishing priority for unchanged information over time

## Section 19 — User Control
Users shall be able to:
- Add/remove topics, entities, geographic foci
- Set temporary interests with expiration
- Mute topics/entities indefinitely or for a duration
- Adjust notification sensitivity (high/medium/low)
- Choose digest frequency (real‑time, hourly, daily, weekly)
- Influence source preferences (whitelist/blacklist, trust levels)
- Control explanation depth (brief, standard, detailed)
- Provide feedback that immediately influences future rankings
All controls affect only awareness behavior; no side‑effects on unrelated agent functions.

## Section 20 — Safety / Trust Requirements
- Treat all external content as untrusted input; never execute embedded instructions
- Detect and flag potential misinformation via cross‑source disagreement
- Require multiple independent sources for high‑confidence claims
- Clearly label speculative content
- Guard against source manipulation (e.g., SEO‑bait) via trust decay
- Prompt injection defenses in any LLM‑based extraction
- Stale information marked with age warnings
- Sensationalism detection (click‑bait heuristics) to lower trust

## Section 21 — Failure Modes
Behavior on failure:
- Source unavailable → use cached data, increase uncertainty, log warning
- Malformed data → skip item, record error, continue
- Conflicting reports → present multiple viewpoints, lower confidence, note disagreement
- Model/extraction failure → fallback to rule‑based heuristics, increase uncertainty, alert operator if persistent
- Entity resolution fails → keep provisional entity ID, flag for review
- Uncertain relevance/importance → default to digest or silent monitoring, communicate low confidence
- Notification delivery failure → retry with back‑off, eventually store for next digest, inform user of delivery issue
System must never crash; degraded mode still provides best‑effort awareness with clear confidence degradation.

## Section 22 — Core Data Contracts (Conceptual)

**InformationItem**
- id, content, timestamp, sourceId, rawUrl, modality (text/image/video), language, extractedFields (optional)
**Source**
- id, name, type, reliabilityScore, lastSuccessfulFetch, metadata
**Event**
- id, type, firstSeen, lastUpdated, stateSummary (structured facts), entities[], topics[], timeline[]
**Entity**
- id, canonicalName, aliases[], entityType, attributes{}, relatedEntities[], firstSeen, lastSeen
**Topic**
- id, label, parentTopic[], associatedEvents[], trendScore
**EventUpdate**
- eventId, timestamp, deltaDescription, confidence, sourceIds[]
**UserInterest**
- userId, topicOrEntityId, weight, explicitFlag, temporaryUntil, mutedUntil, sourcePrefs
**RelevanceAssessment**
- itemId, userId, score, factors[], confidence
**ImportanceAssessment**
- itemId, score, dimensions[], confidence
**ImpactAssessment**
- itemId, directEffects[], indirectEffects[], timeHorizon, valence, confidence
**ConfidenceAssessment**
- itemId, level (high/medium/low), justification[], sourceAgreement
**NotificationDecision**
- itemId, userId, outcome (immediate/digest/silent/ignore/await), priorityScore, rationale
**Notification**
- id, eventId, userId, timestamp, channel, explanation, acknowledged(boolean)
**DigestItem**
- digestId, eventOrUpdateId, position, explanation
**Digest**
- id, userId, startTime, endTime, items[], generatedAt
**UserFeedback**
- id, userId, targetId (notification/event/etc.), type (open/dismiss/important/etc.), comment, timestamp
**EventTimeline**
- eventId, entries[{timestamp, description, sourceIds, confidence}]

## Section 23 — End‑to‑End Behavior (Scenarios)

**Scenario 1 – Major tech development in watched area**
INPUT: New article about breakthrough quantum chip from a trusted tech blog.
→ DETECTION: Fresh item, passes dedup.
→ EVENT UNDERSTANDING: New event type “release”, entities: Company X, Technology Quantum Chip.
→ CHANGE DETECTION: No prior state → wholly new.
→ RELEVANCE: High (user follows quantum computing).
→ IMPORTANCE: High (industry‑scale impact).
→ IMPACT: Direct: faster computing; Indirect: security, crypto; Positive.
→ CONFIDENCE: High (multiple trusted sources corroborating within hour).
→ DECISION: Immediate high‑priority notification.
→ USER OUTPUT: Summary with what changed, why matters to user, confidence, sources, near‑term outlook.
→ MEMORY UPDATE: Create Event, update Entity state, log notification, increase source trust, record user feedback potential.

**Scenario 2 – Negative development on watched company**
INPUT: Press release about data breach at Company Y (user holds shares).
→ Similar flow; RELEVANCE high (financial interest); IMPORTANCE medium‑high; IMPACT negative (financial loss, reputation); CONFIDENCE medium (single source initially, later corroborated); DECISION: Immediate notification (due to urgency & relevance); later digest includes update as breach scope clarifies.

**Scenario 3 – Multiple sources reporting same event**
INPUT: Five articles over two hours about a new government policy.
→ DETECTION: Fresh items; NEAR‑DUPLICATE detection clusters them.
→ EVENT UNDERSTANDING: Single event “policy change”.
→ CHANGE DETECTION: First article creates event; subsequent articles provide updates (details, reactions).
→ RELEVANCE: Evaluated per update; if no change in impact/relevance, only first triggers notification; later updates go to digest if they modify significance.
→ IMPORTANCE: Derived from scope and authority.
→ DECISION: First high‑confidence article triggers notification; later updates may be digest‑only unless they alter policy substance.

**Scenario 4 – Update to known event**
INPUT: New article revises earlier earthquake death toll upward.
→ EVENT UNDERSTANDING: Existing event “earthquake”.
→ CHANGE DETECTION: Detected change in casualty figure (new evidence).
→ RELEVANCE: Adjusted based on updated impact.
→ IMPORTANCE: May increase due to higher toll.
→ CONFIDENCE: Medium initially, rises with corroboration.
→ DECISION: If significance threshold crossed, send update notification (explaining change); otherwise, silent monitoring or digest entry.

**Scenario 5 – Global event with low personal relevance**
INPUT: Major sports championship result (user has no sports interests).
→ DETECTION: Fresh item.
→ EVENT UNDERSTANDING: Event “sports result”.
→ CHANGE DETECTION: New event.
→ RELEVANCE: Low (no matching interests).
→ IMPORTANCE: High globally.
→ IMPACT: Minimal to user.
→ CONFIDENCE: High.
→ DECISION: Suppress immediate notification; include in next digest at low priority or ignore per user’s muted topics; log for possible future relevance if user later expresses interest.

## Section 24 — Requirement Priority
P0 (MVP core): Source ingestion, normalization, provenance, freshness detection, duplicate detection, event detection (basic clustering), entity extraction, resolution, topic classification, change detection (state delta), user‑interest matching, personal relevance analysis, importance estimation, confidence estimation, notification decision (immediate vs digest vs suppress), explanation generation, user feedback capture, short‑term memory for dedup, long‑term entity/event memory, audit logging.
P1: Trend detection, positive/negative analysis, impact analysis, source trust assessment, digest generation, advanced near‑duplicate clustering, feedback‑based adaptation, user controls (topics, mute, frequency).
P2: Historical context deep dives, sophisticated uncertainty propagation, multi‑modal ingestion, real‑time streaming, advanced entity relationship tracking.
P3: Full causal reasoning, counterfactual impact simulation, autonomous source discovery, advanced lifelong learning with safety guards.

## Section 25 — MVP Definition
The MVP will implement the loop:
1. Ingest RSS/Atom feeds and a few web scrapers.
2. Normalize to text items with timestamp, source URL.
3. Assign provenance and detect freshness.
4. Exact and simple near‑duplicate removal (hash + title similarity).
5. Run lightweight NER and entity linking to a static gazetteer (companies, tech terms).
6. Classify topics via keyword matching to user‑defined watchlist.
7. Detect events as clusters of items sharing ≥2 entities and same day.
8. For each event, compute a simple state vector (entity attributes, mentions count).
9. Compare to previous state; if any attribute changed, flag as change.
10. Compute relevance = sum of weights of matched entities/topics.
11. Compute importance = log(mention count) * source trust score.
12. Confidence = average source trust within cluster.
13. Priority = relevance * importance * confidence.
14. If priority > threshold_high → immediate notification; else if > threshold_digest → add to next digest; else suppress.
15. Generate explanation using template: “[Event] changed because [delta]. It matters due to [importance] and relevance to your interest in [topic]. Confidence: [level]. Sources: [list].”
16. Record notification/digest, update entity state, log feedback hooks.
The MVP demonstrates core awareness, avoids treating every article as new event, and provides understandable output.

## Section 26 — Requirements Traceability (excerpt)
| Requirement | Why exists | Related project req | Foundation capability | Custom capability | Priority | Eval method |
|---|---|---|---|---|---|---|
| Source ingestion | Observe world | Personal AI Agent Req. (monitor sources) | OpenJarvis web access, RAG | None (use foundation) | P0 | Feed fetch success rate |
| Event detection | Group items into developments | Awareness mission | None | Custom clustering logic | P0 | Precision/recall on benchmark |
| Personal relevance analysis | Determine user‑specific value | Personal relevance goal | User profile storage (OpenJarvis) | Matching & scoring logic | P0 | A/B test notification relevance |
| Notification decision | Avoid noise | Low‑noise requirement | Scheduling, delivery (OpenJarvis) | Priority calculation & suppression rules | P0 | Noise metrics (notifications per relevant event) |
| Explanation generation | Explain why surfaced | Transparency goal | LLM access (OpenJarvis) | Prompt templating & fact inclusion | P0 | User survey on explanation usefulness |
… (full table to be included in final doc)

## Section 27 — What We Are Not Building
- General task or calendar management.
- Email inbox prioritization.
- Habit tracking or life coaching.
- Autonomous computer control beyond information gathering.
- Emotional support or therapeutic conversation.
- Financial trading advice.
- Medical diagnosis.
- Generic chatbot or open‑ended dialogue not tied to awareness.
Any feature must directly improve world awareness or personal relevance; otherwise out of scope.

## Section 28 — Open Questions
- Exact source mix (news, blogs, academic, social, govt) – needs stakeholder input.
- Notification channels (desktop, mobile, email, chat) – depends on deployment environment.
- Numerical thresholds for priority, relevance, importance – to be tuned during MVP testing.
- Choice of entity linking KB (Wikidata, custom) – depends on coverage vs latency trade‑off.
- Model selection for extraction/classification – to be evaluated against open‑source foundation APIs.
- Frequency of background ingestion cycles – balance freshness vs resource use.
- Detailed definition of “significant change” thresholds for notifications – requires user studies.
- Strategy for temporal decay of interests – requires data on user behavior.
Each open question will be resolved in the design phase with prototyping and user feedback.

## Section 29 — Implementation Boundary
**OpenJarvis Foundation (provide):**
- Agent runtime & lifecycle management
- Model abstraction layer (LLM access)
- Tool framework (web access, scrapers, APIs)
- RAG infrastructure (for grounding)
- Memory infrastructure (short‑term cache, persistent storage)
- Storage & backup
- Scheduling & background workers
- Security sandboxing & observability
- APIs for external plugins & UI

**Custom Awareness Intelligence (to build):**
- Event detection & clustering algorithms
- Change detection (state comparison)
- Entity extraction, normalization, and linking (using foundation’s NER tools but custom resolution logic)
- Topic classification & trend detection
- Personal relevance & importance scoring models
- Impact analysis & positive/negative valence reasoning
- Confidence and source trust assessment
- Notification decision engine (priority, suppression, digest logic)
- Explanation generation (template + LLM‑assisted)
- User interest model persistence & feedback‑driven adaptation
- Audit logging & failure‑handling wrappers
- Anti‑noise mechanisms (cooldown, clustering thresholds)
- User controls UI (settings for topics, mute, frequency)

This boundary follows the OpenJarvis Component Architecture Strategy, ensuring we extend rather than replace the foundation.

--- 
*End of Custom Awareness Intelligence Requirements Document*