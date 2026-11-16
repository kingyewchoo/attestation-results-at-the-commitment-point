
.. raw:: latex

    \pagestyle{plain}
    \thispagestyle{plain}
    %% Suppress all TOC entries for the overview chapter (sections, subsections,
    %% Copyright addcontentsline) — restored just before \sphinxtableofcontents
    %% so chapters from Introduction onward appear in the TOC normally.
    \let\osiaorigaddcontentsline\addcontentsline
    \renewcommand{\addcontentsline}[3]{}

.. _chapter-overview:

Attestation Results at the commitment point
===========================================

.. only:: html

    **Proposed text for FG-TIDA WG1 Deliverable 2**

.. raw:: latex

    \begin{center}\textbf{Proposed text for FG-TIDA WG1 Deliverable 2}\end{center}

Abstract
--------

Before a service commits something it cannot take back on an AI agent's instruction (cost, a payment, a secret or access to a production system), it needs to know what the agent is executing at that moment, which its last appraisal may no longer show. RFC 9334 separates when a claim's value was generated from when the Relying Party performs the operation, and AR4SI leaves the expiry of individual Trustworthiness Claims undefined. This contribution proposes text for WG1 Deliverable 2 on that moment, the commitment point: a declared freshness deadline, a measurement age per required aspect carried in the Attestation Result, age rules for evidence measured before use or only at launch, and negative tests in which only the appraisal separates two builds, with the measurement of context and memory left to Deliverable 2. Annex C shows how it combines with the timeliness requirements of contribution 12, and Annex D notes binding and disclosure points for other clauses. It defines no protocol or format.

History
-------

.. list-table::
    :header-rows: 1
    :widths: 10 26 18 14 32

    * - Version
      - Document
      - Approval
      - Focus Group
      - Working Group
    * - 0.0
      - Attestation Results at the commitment point
      - Not yet adopted
      - TIDA
      - CDRA

To access this Document as a PDF, open the published documentation site in your web
browser and click the PDF download link, or navigate directly to
https://kingyewchoo.github.io/attestation-results-at-the-commitment-point/specification.pdf.

Authors
-------

.. list-table::
    :header-rows: 1
    :widths: 30 40 30

    * - Name
      - Affiliation
      - Contact
    * - King Yew Choo (proposed editor)
      - True Primary
      - k.choo@trueprimary.com

Keywords
--------

Remote attestation, Attestation Results, freshness, measurement age, commitment point, Relying Party, AR4SI, disclosure.

.. raw:: latex

    \newpage
    \osiacenternextsection

Foreword
--------

Agentic AI systems now act as autonomous entities capable of reasoning, planning, and
executing tasks across services, infrastructures, and organizational boundaries. This
fundamentally changes the nature of identity in digital systems: identity is no longer
limited to human users and static machine instances but must also encompass autonomous,
dynamic, and goal-oriented agents. And identity alone is insufficient — the core question
extends beyond "what an entity is" to "whether, and under what conditions, that entity
should be trusted to act".

The Focus Group on Trust and Identity for humans and agentic AI (FG-TIDA) addresses trust
management and interoperable digital identity infrastructure for humans and for agentic AI,
supporting the development of secure, trustworthy digital ecosystems in which humans and
agentic AI can safely interact and collaborate.

FG-TIDA was established under Recommendation ITU-T A.7 (Focus groups: Establishment and
working procedures), with ITU-T Study Group 17 as its parent group. Further details are
available in the group's terms of reference.

For more news and updates about the TIDA Community, including its supporter organizations,
please refer to the community's published resources.

.. raw:: latex

    \osiacenternextsection

Note
----

This draft proposes input to the WG1 Deliverable 2 outline: requirements for the clause on when fresh Evidence is due and how long Attestation Results stay valid, with Annex D's notes on binding and disclosure for other clauses; the section Requested decision and milestones states the decision requested.

.. Intellectual Property Rights — section commented out. To restore it, remove the
   leading ".." and the indentation below, and re-add a ".. raw:: latex" block with
   "\osiacenternextsection" before the heading (to centre it on the PDF).

   Intellectual Property Rights
   ----------------------------

   ITU draws attention to the possibility that the practice or implementation of this
   Recommendation may involve the use of a claimed Intellectual Property Right. ITU takes no
   position concerning the evidence, validity or applicability of claimed Intellectual Property
   Rights, whether asserted by ITU members or others outside of the Recommendation development
   process.

   As of the date of approval of this Recommendation, ITU had not received notice of intellectual
   property, protected by patents/software copyrights, which may be required to implement this
   Recommendation. However, implementers are cautioned that this may not represent the latest
   information and are therefore strongly urged to consult the appropriate ITU-T databases
   available via the ITU-T website at http://www.itu.int/ITU-T/ipr/.
   Implementers should also be aware that the organization that originated the technically
   equivalent document listed in the Bibliography may have received notices of intellectual
   property required for the implementation of this Recommendation.

.. raw:: latex

    %% Copyright notice commented out. To restore, uncomment the lines below.
    %% \vspace{4em}
    %% \begin{center}\textcopyright{} ITU 2024\end{center}
    %%
    %% \medskip
    %% All rights reserved. No part of this publication may be reproduced, by any means whatsoever, without the
    %% prior written permission of ITU.

    %% Restore \addcontentsline so the TOC below (and Chapter 1 onward) populate.
    \let\addcontentsline\osiaorigaddcontentsline
    \sphinxtableofcontents
    \osiamainmatter

.. toctree::
    :maxdepth: 3
    :numbered:
    :hidden:

    chapters/01_proposal

.. toctree::
    :maxdepth: 2
    :hidden:

    chapters/02_annex_a
    chapters/03_annex_b
    chapters/04_annex_c
    chapters/05_annex_d
    chapters/06_references
