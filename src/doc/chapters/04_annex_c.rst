Annex C (informative): how this item and contribution 12 combine
================================================================

Contribution 12, a proposed Technical Specification of its own, and this item answer two halves of one timeliness question. Contribution 12 works where Evidence is produced and checked: when it was measured and how long a report stays valid. This item works on the Attestation Result and the Relying Party's check: how old each aspect's measurement is when it commits. The table gives one combined reading, offered for WG1 and the authors of contribution 12 to settle.

.. list-table::
    :header-rows: 1
    :widths: 22 26 26 26

    * - Question
      - Contribution 12
      - This item
      - Combined
    * - When was it measured?
      - measurementTime from a trusted time source, on each report (REQ-TM-01)
      - An age per aspect (R3)
      - Absolute times stay in Evidence to the Verifier; ages go in the Attestation Result to the Relying Party (Annex D, D4)
    * - How long is it valid?
      - notBefore and notAfter, set by environment type and risk (REQ-TM-01, REQ-TM-02)
      - A maximum age per aspect, declared by the Relying Party (R1)
      - The report's window bounds reuse of Evidence; each aspect's age is checked at the commitment event (R10)
    * - When is it refreshed?
      - Before the window ends and on environment-change events (REQ-TM-03, REQ-TM-04)
      - The commitment event as the deadline (R1, R10)
      - Their triggers say when to refresh; the commitment event says by when
    * - Does trust fall within the window?
      - Linear or step-wise decay of trust levels within the validity period (REQ-TD-05)
      - No decay: each aspect is within its limit or stale at the commitment event (R10, R11)
      - Decay can grade trust inside the window; the commitment-event check is the outer limit
    * - A launch measurement in a new report
      - The new report gets a new measurementTime (REQ-TM-01)
      - Keeps its start, or has no age if no Verifier value was bound at launch, unless the launch state keeps the aspect fixed (R4, R6)
      - To settle together: whether a re-reported launch measurement restarts the window or keeps its start
