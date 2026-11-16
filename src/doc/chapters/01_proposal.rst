Proposal
========

Scope and motivation
--------------------

The WG1 draft charter starts from one decision: before a service releases a payment, a secret or access to a production system to an agent, it needs to know continuously what is actually being executed (charter, section 2). This item calls the moment of release the **commitment point**: the Relying Party commits something it cannot take back, whether cost (work, compute, a payment), a secret or access. In the time model of `RFC 9334 <https://www.rfc-editor.org/rfc/rfc9334.html>`__ (Appendix A), it is the event OP, "operation performed".

An agent changes at runtime: its model, code, tools, policies and context can all change (charter, section 2). An appraisal that was fresh when a session began may not describe what sends the instruction at the commitment point. RFC 9334 leaves the freshness threshold to local policy (section 10) and notes that claim values may be generated long before they are signed (section 10.4). `AR4SI <https://datatracker.ietf.org/doc/draft-ietf-rats-ar4si/>`__ says a Relying Party may track when a Verifier expires its confidence in individual Trustworthiness Claims, but defines no mechanism for it (section 2.4). No requirement yet ties the age of each required aspect's measurement to the commitment point.

This item proposes such requirements for WG1, with Deliverable 2 as their home. It covers when, and against what limits, measurements are checked; how each part of an agent is measured is the dynamic-attestation work of Deliverable 2 itself. It defines no protocol, message or document format (charter, section 4).

The Attestation Result is one input to the Relying Party's enforcement decision, which belongs to WG2 (charter, sections 4 and 7). This item states only what the result carries and when it is checked. Whether a request starts, how retries are deduplicated, and whether an authority is valid all stay outside it.

What this item adds
-------------------

- **The commitment event as a freshness deadline.** The Relying Party declares the event at which it commits; each required aspect must be within its age limit at that event. It complements the risk-keyed triggers the charter lists (section 4), which say when to refresh; the deadline says by when.
- **A measurement age per required aspect, carried in the Attestation Result.** The Verifier reports each age relative to the result's own generation, so the check needs no clock shared by the Verifier and the Relying Party and works with nonce-based freshness (R9). It sets requirements for the per-claim expiry AR4SI leaves open; how a format carries it is a question for IETF RATS.
- **Age rules that match real evidence.** Where every component is measured before it loads, a fresh report over the complete log shows everything loaded up to that report, and the aspect's age is the report's age. Where measurements are taken only at launch, an aspect the launch state keeps fixed takes the age of the freshest report showing that launch; one that can change takes its age from the issue of an unpredictable Verifier value bound into the evidence at launch, and otherwise has no age. Parts that change without being loaded, such as context and memory, need the runtime measurement Deliverable 2 defines, which this item asks of it. Annex B shows what four common evidence types can establish.
- **Tools loaded mid-task.** An agent that loads a tool during a task changes what it executes after its last appraisal; under R5 the tool appears in the next report, and the age limit bounds how long it can go unseen.
- **Negative tests that isolate attestation.** Two builds share every other input, so only the appraisal can separate them, and a gate refusing everything fails.

Terms
-----

Terms from RFC 9334 and AR4SI are used as defined there. This item adds four, offered to WG0's terminology work (charter, section 7).

Commitment event
   The event that marks the commitment point, declared by the Relying Party for each kind of request it accepts; an instance of RFC 9334's event OP, operation performed.

Measurement age
   For an aspect, the time from the latest point at which the Evidence establishes the aspect's state to the generation of the Attestation Result, time(RG). Evidence establishes the state at its own freshness point where every load into the aspect since launch is in the log it covers (R5), or where the measured launch state keeps the aspect fixed (R6). Otherwise, where the launch measurement covers the aspect, the age runs from the issue of an unpredictable value the Verifier issued before launch and the Evidence binds; without one, or for an aspect the launch measurement does not cover, such as context and memory, there is no age until Deliverable 2 defines its measurement (R6, R7). A freshness point is placed by a value whose issue time the Verifier knows: its own nonce, an epoch ID or a trusted timestamp (RFC 9334, section 10).

Measured before use
   An aspect whose every component is measured into a log before it loads, so fresh Evidence covering the complete log shows every component loaded up to that moment. It shows what has loaded; changes inside a loaded component do not appear.

Accepted exposure
   An aspect past the Relying Party's age limit, or with no measurement age, on which the Relying Party's enforcement decision (WG2) proceeds anyway. This item asks only that the Relying Party records it as exposure, never as a freshness pass (R12).

Requirements
------------

Each requirement names who must do what, in ITU-T form ("is required to"). Together they are the proposed input to the Deliverable 2 clause on freshness and validity, and they stand on their own. Annex D records what they assume about binding and disclosure, for the clauses that own those.

**What the Attestation Result carries**

R1. For each kind of request, the Relying Party is required to declare, with its challenge or in a policy it publishes, the commitment event, the aspects it requires at that event (for example executables, configuration, model, tools, policy and context) and a maximum age for each.

R2. Before it accepts requests, the Relying Party is required to fix the Verifiers it accepts, their trust anchors, and the version of its own appraisal policy.

R3. For each required aspect, the Verifier is required to report its measurement age, counted from the point at which that age starts under R5 or R6, or that it has none (R7), and, where an AR4SI Trustworthiness Claim covers the aspect (for example executables for agent and tool code, configuration for configuration and policy, file-system for model files, sourced-data for context drawn from external systems), that claim's Trustworthiness Tier (AR4SI, section 3.2). Aspects no AR4SI claim covers, such as context the agent builds itself (conversation state and memory), are a gap for Deliverable 1 and IETF RATS, and until it closes they have no tier.

R4. When it re-signs a claim, the Verifier is required to keep the start of its measurement age, so the age runs from that start to the new result's generation and is never reduced.

R5. For an aspect measured before use, the Verifier is required to take as the measurement age the age of the freshest Evidence covering the complete log.

R6. For an aspect measured only at launch, the Verifier is required to take as the measurement age the age of the freshest Evidence showing that launch measurement, where the measured launch state includes enforcement keeping the aspect fixed afterwards (for example a read-only, integrity-verified file system). Where nothing keeps it fixed and the Evidence binds an unpredictable value the Verifier issued before launch, such as its nonce, the age runs from that value's issue (Annex B); otherwise the aspect has no measurement age.

R7. The Verifier is required to report an aspect with no measurement age as having none, never as fresh.

R8. The Verifier is required to show in the Attestation Result when a required aspect is missing, negative or incomplete, and to identify the Reference Values and appraisal policy it applied, so that a withdrawn Reference Value can be matched to the results that relied on it. With staleness found under R10 and R11, the enforcement decision can act on all four conditions (charter, section 4).

**When it is checked**

R9. The Relying Party is required to establish the Attestation Result's age by a mechanism of RFC 9334 section 10: its own nonce carried into the result, an epoch ID, or a timestamp only where it trusts the Verifier's clock. A nonce makes the result specific to that Relying Party.

R10. At the commitment event, the Relying Party is required to add the Attestation Result's age to each required aspect's reported measurement age and check the sum against that aspect's limit.

R11. The Relying Party is required to treat an aspect past its limit, or with no measurement age, as stale. Whether it then refuses the request or proceeds is its enforcement decision (WG2); if it proceeds, the aspect is accepted exposure.

R12. The Relying Party is required to record accepted exposure as exposure, never as a freshness pass.

R13. In long-running work, the Relying Party is required to repeat R9 to R12 at each further commitment event.

**Gap for Deliverable 2: context and memory**

This item asks Deliverable 2 to define how the context and memory an agent acts on for a request are measured at that request and bound to it, so that the aspect can have a measurement age. Until then the Verifier reports the aspect as having none (R7).

Tests
-----

The negative tests the charter asks for (section 4). Each declares, before it runs, its expected outcome, its trust inputs and which components are real or simulated.

a. Two builds share every input other than attestation, all valid for both. Each run of this test names the appraised measurement and rule that separate the two builds. The accepted build passes and the excluded build fails, so a gate refusing everything fails (R1, R3).
b. An old measurement re-signed is aged from its original start, and past the limit it counts as stale (R4, R11).
c. An aspect measured before use passes with fresh Evidence over the complete log, and a tool loaded after that Evidence appears in the next (R5).
d. An aspect measured only at launch passes with each fresh report when its launch state keeps it fixed; otherwise it passes while within its limit when a Verifier-issued launch value is bound in, and has no age when none is (R6, R7).
e. Work paused past an age limit before a later commitment event is rechecked at that event (R13).
f. A result bound to one Relying Party's nonce fails at another (R9).
g. An Attestation Result whose own age, and the measurement age it reports for an aspect, are each within the limit the Relying Party declared for that aspect fails when their sum exceeds that limit (R1, R10).
h. An aspect with a negative appraisal, or missing from the Evidence, is shown as such in the Attestation Result, never omitted, and the result identifies the Reference Values it was appraised against (R8).
i. A run that proceeds on a stale aspect records it as exposure, never as a freshness pass (R11, R12).
j. A result from a Verifier, or under a trust anchor, that the Relying Party did not fix before accepting requests fails (R2).

Every requirement has at least one test. No interoperability is claimed for any mapping the tests do not cover.

Where the requirements land
---------------------------

.. list-table::
    :header-rows: 1
    :widths: 22 78

    * - Requirements
      - WG1 deliverable
    * - R1 to R8
      - Deliverable 2: when fresh Evidence is due, how long Attestation Results stay valid, trustworthiness tiers per aspect
    * - R9 to R13
      - Deliverable 2: refresh and validity at the commitment event
    * - Gap for Deliverable 2
      - Deliverable 2: Evidence about an agent's context, which the charter places in scope (section 4)
    * - Annex D notes
      - Deliverable 2, binding and disclosure (charter, sections 4 and 8); Deliverable 3, where instances share an environment
    * - Gaps found
      - Deliverable 1: building blocks and gaps; Deliverable 4, if confirmed: conveying tiers and ages

Relations to other work
-----------------------

- **RFC 9334** supplies the roles, the passport model (section 5.1) and the time model this item builds on (section 10 and Appendix A).
- **AR4SI** (draft-ietf-rats-ar4si-11) supplies Trustworthiness Claims and Tiers. Its AR-augmented Evidence (section 4.2) binds a result to the Relying Party's own proof of freshness and lets the Relying Party calculate the time between the Verifier's proof of freshness and its own; R9 and R10, with the request binding Annex D notes, carry that to each aspect, checked at the commitment event. The per-aspect expiry its section 2.4 leaves open goes to IETF RATS as a question; WG1's part is the requirement on Relying Parties.
- **Epoch Markers** (`draft-ietf-rats-epoch-markers-08 <https://datatracker.ietf.org/doc/draft-ietf-rats-epoch-markers/>`__) give a shared notion of freshness without a local clock: one way to place a runtime measurement in time.
- **EAR** (`draft-ietf-rats-ear-05 <https://datatracker.ietf.org/doc/draft-ietf-rats-ear/>`__), which embeds AR4SI's trustworthiness vector in an EAT (`RFC 9711 <https://www.rfc-editor.org/rfc/rfc9711.html>`__), is the natural carrier for the tier and age per aspect; this item defines neither a format nor an extension.
- **Contribution 12** (`fg-tida-contribution-12 <https://tida-datatracker.org/doc/fg-tida-contribution-12/>`__), a proposed Technical Specification on security requirements for trusted environment measurement and attestation of AI agents, also declared for WG1, gives each measurement report a measurement time and a validity window (notBefore, notAfter) and requires re-measurement before the window ends or when the environment changes (its REQ-TM-01 to REQ-TM-04). The two fit as layers: contribution 12 sets each report's measurement time and validity window where Evidence is produced and checked; this item ages each aspect in the Attestation Result and has the Relying Party check that age at the commitment event. Annex C sets out one combined reading.
- **WG2** makes the enforcement decision this result feeds.
- **Closest IETF drafts**, which guard the same boundary for one operation but do not bound how old each aspect's measurement is there: `Action Evidence Boundary <https://datatracker.ietf.org/doc/draft-schrock-action-evidence-boundary/>`__ (-07), an executor-side model binding evidence and authority to one exact effect; `Attestation-Bound Execution Finality <https://datatracker.ietf.org/doc/draft-das-rats-attestation-bnd-execution-finality/>`__ (-04), which makes a protected effect reachable only through a verifying boundary.

The item grew from `use case 35 <https://github.com/fg-tida-wg0/use-cases/issues/35>`__ in WG0's repository, also recorded on the DataTracker as a WG0 use case. Annex A works its central case through the requirements.

Out of scope
------------

How each part of an agent is measured; enforcement decisions and whether a request starts (WG2); the validity of authorities; payment authorization; new protocols, message or document formats; liability and certification.

Requested decision and milestones
---------------------------------

At Paris: allocation to WG1 and adoption as a time-boxed item, recorded as input to the Deliverable 2 outline due at Geneva (`WG1 draft charter <https://github.com/fg-tida-wg1/wg1-cdra-charter/blob/main/charter.md>`__, sections 5 and 6), where the clause on when fresh Evidence is due and how long Attestation Results stay valid would start from these requirements; Annex C offers one reading of how they combine with contribution 12's timeliness requirements, for WG1 and contribution 12's authors to settle. Annex D's notes go to the binding and disclosure clauses and the Deliverable 1 gap list. At Geneva the item folds into that outline or closes. The author would serve as an editor of that clause once WG1 appoints its Deliverable 2 editors.

.. list-table::
    :header-rows: 1
    :widths: 32 68

    * - Date
      - Output
    * - Paris, 30 November to 1 December 2026
      - Allocation to WG1 and adoption as a time-boxed item; the first test list.
    * - December 2026
      - Requirements text and tests; the expiry question to IETF RATS.
    * - Geneva, 26 to 27 January 2027
      - In the Deliverable 2 outline, or closed.
