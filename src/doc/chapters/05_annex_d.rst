Annex D (informative): notes for the binding and disclosure clauses
===================================================================

These notes are not proposed text for the freshness clause. They record what its requirements assume about binding a result to a request and about disclosure, for the Deliverable 2 clauses that own those (charter, sections 4 and 8) and for the Deliverable 1 gap list.

D1. **Request binding.** The instance signs the Relying Party's challenge and the request identifier together, with a key held in the measured component and identified in the Attestation Result. That the measured code signs only requests originating inside the agent is a property the appraisal of that code has to establish.

D2. **Shared environments.** A deployment that shows only where the key is held claims only that, and instances sharing one environment are identified as that environment.

D3. **Scoped identifiers.** The instance key and the request identifier are scoped to the Relying Party that sees them, unless that Relying Party needs linking across Relying Parties and the principal the agent acts for has agreed.

D4. **Ages, and rounding their start.** The Attestation Result carries ages, never absolute measurement or launch times. A Verifier can also round each age's start down to a whole multiple of a granularity it publishes, so rounding only lengthens an age and every result about one measurement gives the same rounded time; a result generated soon after a measurement still narrows it, so rounding reduces linkage and some remains. A Relying Party would then fix (R2) only Verifiers whose granularity, added to the result age and the measurement age it expects, stays within its smallest age limit.

D5. **Raw Evidence.** The Relying Party reads raw Evidence only for a claim its policy names and the Attestation Result does not carry. In the background-check model (RFC 9334, section 5.2), it relays Evidence it cannot read, for example Evidence encrypted to the Verifier.

D6. **Verifier linkage.** A Verifier states whether and when it links one instance's appraisals across Relying Parties, for example to warn them of a compromise, and how long it keeps appraisal records. Anonymous attestation, such as TPM 2.0 ECDAA, hides the attestation key, though the clock fields in each TPM 2.0 quote can still link one TPM's quotes.

Disclosure, as charter section 8 asks:

.. list-table::
    :header-rows: 1
    :widths: 24 20 28 28

    * - Item
      - Received by
      - Linkable across Relying Parties
      - A failed appraisal reveals
    * - Tier per aspect
      - Relying Party
      - No, beyond the build's tier
      - Which aspect failed
    * - Attestation Result
      - Relying Party
      - It can, if it carries values unique to the instance; a nonce stops replay to another Relying Party and leaves linkage open (R9)
      - Which aspects it covers
    * - Measurement age
      - Relying Party
      - Yes, possibly, where ages are exact; partly, where they start from a rounded time shared by instances measured in the same interval, since a result generated soon after a measurement narrows that interval (D4); a timestamp on the result can add a link (R9)
      - That an aspect is stale
    * - Instance key
      - Relying Party
      - No, where scoped per Relying Party (D3)
      - Nothing further
    * - Request identifier
      - Relying Party
      - No: one request, one Relying Party
      - Nothing further
    * - Raw Evidence
      - Relying Party, only under D5 (relayed unreadable in the background-check model)
      - Yes, possibly: full software state
      - The named claim's value
    * - Raw Evidence, and each Relying Party's nonce
      - Verifier
      - It could, as it sees every appraisal for every Relying Party; D6 asks it to say when it does
      - The full appraised state

Tests these notes suggest, for the clauses that take them up: a signature requested by a caller outside the measured agent, or made by a key the result does not identify, fails (D1); two instances sharing one environment receive results that identify the environment and neither instance (D2); one instance presenting to two Relying Parties cannot be linked by key or request identifier (D3); a result carries no absolute measurement or launch time, and, for an age whose start falls late in a grid interval, its generation time minus that age equals the start rounded down to the published granularity (D4).
