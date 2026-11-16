Annex B (informative): what common evidence can establish
=========================================================

How the age rules (R5 to R7) apply to four widely used evidence types. Each binds a challenge into fresh Evidence. They differ in whether components loaded after launch are measured. None carries a launch time, but three can bind an unpredictable value fixed at launch, which places launch after that value was issued.

.. list-table::
    :header-rows: 1
    :widths: 16 22 20 24 18

    * - Evidence
      - Freshness anchor
      - Launch measurement
      - Measured before use, after launch
      - Launch placed in time
    * - TPM 2.0 quote
      - Qualifying data (nonce) in the quote
      - PCRs from measured boot
      - PCRs with an event log, such as Linux IMA
      - Not carried; clock and reset counters are relative
    * - Intel TDX quote
      - REPORTDATA
      - MRTD
      - RTMRs with an event log
      - Through a value bound in MRCONFIGID
    * - AMD SEV-SNP report
      - REPORT_DATA
      - MEASUREMENT
      - None in the report; a virtual TPM can add it
      - Through a value bound in HOST_DATA
    * - Arm CCA realm token
      - Challenge
      - RIM
      - REMs with an event log
      - Through a value bound in the Realm Personalization Value

What follows for the age rules:

- **Measured before use** (R5): where the event log covers every component loaded since launch, a challenge-fresh report gives each such aspect the age of that report. TPM with IMA, TDX with RTMRs and CCA with REMs can meet this.
- **Launch only** (R6, R7): where the measured launch state keeps the aspect fixed, such as a read-only, integrity-verified file system, each fresh report showing the same launch measurement gives it that report's age. Where nothing keeps it fixed and the launcher binds an unpredictable Verifier-issued value at launch, age runs from that value's issue; without it, the aspect has no measurement age. The binding costs one Verifier round trip per launch, and some confidential-container stacks already use these fields for policy digests, so the value may have to share the field. An SEV-SNP report alone covers nothing loaded after launch, unless the deployment adds a runtime measurement source such as a virtual TPM.
- **Changing without loading**: Linux IMA can re-measure some kernel-held state when it changes (critical data, such as device-mapper state). None of the four measures an agent's context or memory; those aspects need the runtime measurement Deliverable 2 defines.
- **Epoch markers** can place a runtime measurement after a known point without a local clock, giving it a measurement age where a nonce round trip is impractical.
