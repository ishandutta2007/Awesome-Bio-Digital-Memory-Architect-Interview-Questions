# 5. System Architecture & Data Pipeline Design

Integrating digital computation and wet-lab biochemistry into a single, coherent working storage system — a genuinely hybrid engineering challenge.

---

### Q: What are the main stages of an end-to-end DNA data storage pipeline, from receiving raw digital input data to eventually retrieving and reconstructing that data later? 🟡

**Answer:**
A representative end-to-end pipeline:
1. **Encoding (digital):** the input digital data is logically encoded into a nucleotide sequence representation, incorporating error-correction redundancy, satisfying biological sequence constraints, and adding addressing information (sections 1, 2, 4) — an entirely computational stage.
2. **Synthesis (wet-lab/hybrid):** the designed nucleotide sequences are physically synthesized into real DNA molecules (section 3), transitioning from a purely digital representation to a physical biochemical artifact.
3. **Storage (physical):** the synthesized DNA is stored under appropriate physical/environmental conditions (section 6) for the archival period.
4. **Retrieval and amplification (wet-lab):** when data needs to be read, the relevant physical DNA sample is retrieved from storage and (for random access) selectively amplified via PCR using appropriate address-targeting primers (sections 3, 4).
5. **Sequencing (wet-lab/hybrid):** the amplified DNA is sequenced, producing raw sequencing reads — transitioning back from a physical biochemical artifact to a digital representation (albeit a noisy, error-prone one).
6. **Decoding (digital):** the raw sequencing reads are processed computationally — aligned/consensus-called across redundant reads, error-corrected using the applied error-correction scheme, and decoded back into the original digital data.

A DNA storage system architect needs to design and reason about this entire pipeline holistically, since design choices at any one stage (e.g., the specific encoding scheme chosen in stage 1) have direct, consequential implications for the performance, cost, and reliability of other stages (e.g., stage 6's decoding complexity and achievable reliability) — this is fundamentally a co-design problem spanning digital computation and wet-lab biochemistry together, not a set of independently optimizable stages.

**Follow-ups:**
- Which of these pipeline stages do you think currently represents the biggest overall bottleneck (in terms of cost, speed, or reliability) for making DNA storage practically viable at scale, and why?

---

### Q: How would you approach designing the software/computational infrastructure needed to manage a DNA storage system's encoding, decoding, and metadata (e.g., tracking what data is stored where, and its associated addressing scheme)? 🟡

**Answer:**
- **Treat the encoding/decoding software as a rigorously tested, versioned system component**, analogous to any critical data-integrity software system — given that a bug or inconsistency in the encoding/decoding logic could silently corrupt archived data in a way that might not be discovered until a (potentially very distant, future) retrieval attempt fails, rigorous software testing and validation discipline is especially important here, arguably even more so than in many conventional software systems, given the very long time horizons and potentially very high cost of a retrieval failure long after the original encoding was performed.
- **Maintain comprehensive, durable metadata** tracking exactly which encoding scheme version, error-correction parameters, and addressing scheme were used for each specific stored dataset — since a DNA archive might be stored for a very long time (potentially longer than any single specific encoding scheme's practical software lifetime), the system needs robust provenance tracking to ensure a future retrieval attempt (potentially using updated decoding software) can still correctly identify and apply the correct original decoding logic for older archived data, directly analogous to the versioning/provenance discipline discussed in the ML Engineer (Biotech) companion repo, applied here to the storage system's own core data-recovery logic.
- **Design the software architecture to be resilient to the possibility that decoding software itself needs to be run and maintained over a much longer timescale than typical software systems** — considering how encoding/decoding logic and its dependencies will remain runnable and correctly interpretable potentially decades after the original data was archived, which is a distinctive long-term-software-maintenance challenge given DNA storage's archival value proposition centers specifically on very long storage timescales.

**Follow-ups:**
- How would you approach ensuring that decoding software written today will still be correctly runnable and produce correct results if a retrieval attempt happens several decades from now, given how much conventional software infrastructure typically changes over that kind of timescale?

---

### Q: How would you design the physical/wet-lab and digital/computational teams' collaboration and interface for a DNA storage system development project, given that this work genuinely spans both domains? 🟡

**Answer:**
- **Establish clear, well-specified interfaces between the digital encoding/decoding logic and the wet-lab synthesis/sequencing processes** — e.g., precisely specifying what sequence format and constraints the digital encoding stage needs to hand off to the wet-lab synthesis stage, and what raw sequencing data format/quality the wet-lab sequencing stage needs to hand back to the digital decoding stage — so that each side can iterate and improve their respective component somewhat independently within these agreed interface constraints, rather than requiring constant, tightly-coupled coordination for every change.
- **Build shared, empirically-grounded error models** connecting both sides' expertise — the digital/computational team needs accurate, empirically-validated error rate models from the wet-lab team's actual synthesis/sequencing experiments to design and validate appropriate error-correction schemes (rather than relying on purely theoretical or vendor-specification error assumptions), while the wet-lab team benefits from understanding which specific error types and rates the digital encoding scheme is most sensitive to, to help prioritize wet-lab process improvements that would most benefit overall system reliability.
- **Run frequent, small-scale integrated pilot tests** (actually encoding, synthesizing, storing, retrieving, and decoding a small test dataset end-to-end) rather than only validating each domain's component in isolation — since subtle integration issues (a digital encoding assumption that doesn't quite match actual wet-lab behavior, or vice versa) are often only caught through genuine end-to-end testing, not by validating each side independently against its own assumptions about the other side's behavior.

**Follow-ups:**
- Describe how you'd design an initial small-scale end-to-end pilot test to validate a new DNA storage system design before committing to synthesizing and archiving a much larger, more costly dataset.

---

### Q: How would you design a DNA storage system's architecture to support incremental scaling — i.e., starting with a small proof-of-concept system and scaling up storage capacity over time — without requiring a complete redesign at each scaling stage? 🔴

**Answer:**
- **Design the addressing scheme with headroom for future scale from the start** (see section 4's discussion of hierarchical addressing), since retrofitting an addressing scheme originally designed only for a small proof-of-concept scale to support a much larger future archive can be far more disruptive than planning for reasonable future scale upfront, even if the initial deployed system doesn't yet use that full addressing capacity.
- **Modularize the physical storage/organization of the archive** (e.g., organizing physical DNA storage into discrete, independently-manageable pools or containers corresponding to the hierarchical addressing structure) so that additional storage capacity can be added incrementally as new, independently-organized physical pools, rather than requiring the entire existing archive's physical organization to be redesigned or consolidated each time capacity is expanded.
- **Validate that the chosen encoding scheme and error-correction approach's performance characteristics scale reasonably as archive size grows** — e.g., confirming that addressing scheme cross-reactivity risk (section 4) remains acceptably low as the total number of distinct addresses in use grows toward the anticipated larger future scale, rather than only validating performance at the small initial proof-of-concept scale and assuming it will hold at much larger scale without further validation.
- **Plan for the practical reality that wet-lab synthesis/sequencing throughput, not just software/addressing design, is often the actual binding constraint on how quickly a system can scale up** — coordinating the system architecture's scaling plan with realistic wet-lab process throughput scaling expectations, rather than assuming software/addressing design is the only relevant scaling bottleneck to plan around.

**Follow-ups:**
- How would you decide how much future scaling headroom to build into an addressing scheme upfront, given that overprovisioning too aggressively costs sequence overhead/density today for capacity that might not be needed for years?
