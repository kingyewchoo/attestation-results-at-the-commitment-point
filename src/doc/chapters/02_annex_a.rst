Annex A (informative): one paid request
=======================================

This annex works one case through the requirements. It adds no requirement to them.

A deployer runs one agent build for about 17,000 consumers. Each orders one £15 answer a week from a seller of expert time, paying on their own card within £20 per-purchase and £100 weekly limits. The case is hypothetical; amounts and volumes are illustrative.

The seller's acceptance rule concerns the build: one appraisal of it governs about 17,000 purchases a week, while each user's spending limits bind only that user. Payment approval, even with a card token bound to a registered agent, shows which agent may spend; it says nothing of the state that agent runs in. A fault in one build reaches every one of those purchases at once.

.. list-table::
    :header-rows: 1
    :widths: 32 68

    * - Requirement
      - In this case
    * - Commitment event (R1)
      - Dispatch of the expert task
    * - Required aspects (R1)
      - Executables and configuration of the agent build, its tools, and its context
    * - Age rule (R5, R6)
      - Executables and tools measured before use; configuration measured at launch and kept fixed by a read-only, integrity-verified file system
    * - Request binding (Annex D, D1)
      - A stable purchase identifier, signed with the seller's challenge
    * - Spending authority
      - Bound to the request at the authorization interface (charter, section 7), a dependency of this item
    * - Age check (R10)
      - At dispatch, after payment approval and before work starts

The outcome: executables and tools pass with each fresh report. Configuration passes with each fresh report showing the same launch, with no relaunch needed. Context has no measurement age today, so it counts as stale, and the seller either refuses or proceeds on accepted exposure (R11). For it to pass, it needs the measurement this item asks of Deliverable 2 (the context the agent acts on for this request, measured at the request and bound to it) and a tier under R3.

Payment authorization itself stays with the payment actors. Whether a payment method can carry these items is for its authors to say.
