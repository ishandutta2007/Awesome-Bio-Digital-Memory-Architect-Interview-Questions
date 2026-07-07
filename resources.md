# Additional Resources

A short list of resources for going deeper on DNA-based digital data storage. (Not exhaustive — PRs welcome.) This is a small, young, and fast-moving field spanning information theory, molecular biology, and systems engineering — cost figures, throughput numbers, and state-of-the-art techniques change quickly, so cross-check anything specific against current literature before an interview.

## Foundational Reading
- Church, Gao & Kosuri, "Next-Generation Digital Information Storage in DNA" (Science, 2012) — one of the foundational papers demonstrating modern DNA data storage feasibility, useful historical and technical grounding.
- Goldman et al., "Towards practical, high-capacity, low-maintenance information storage in synthesized DNA" (Nature, 2013) — another foundational early demonstration, including early discussion of encoding scheme design.
- Erlich & Zielinski, "DNA Fountain enables a robust and efficient storage architecture" (Science, 2017) — an influential paper on advanced encoding/error-correction approaches specifically for DNA storage.
- Organick et al., "Random access in large-scale DNA data storage" (Nature Biotechnology, 2018) — foundational work specifically on the random-access retrieval challenges discussed in section 4.
- General information theory and error-correction coding textbooks (e.g., covering Reed-Solomon codes, fountain codes, and related concepts) for the underlying coding-theory foundations these DNA-storage-specific schemes build on.

## Key Organizations & Groups (good to be conversationally familiar with)
- Microsoft Research's DNA storage research program and publications.
- University research groups with dedicated, publicly active DNA data storage research programs (search recent publications, since this is an actively evolving academic space with groups entering and shifting focus over time).
- Commercial DNA synthesis providers relevant to the massively parallel synthesis approaches discussed in section 3.

## Communities & Discussion
- r/biotech and r/MachineLearning for occasional relevant discussion threads (DNA storage is a niche enough topic that dedicated communities are limited; academic conference proceedings and preprint servers are generally more productive sources).
- bioRxiv and arXiv (particularly the information theory and quantitative biology categories) for recent preprints in this space.

## Companion Repos (adjacent, complementary content)
- **Synthetic Biology Designer** — DNA assembly, synthesis, and genome editing fundamentals relevant to the physical/wet-lab side of this field.
- **Computational Biologist** — core algorithms and sequence analysis fundamentals relevant to the encoding/decoding and sequencing-data-processing side.
- **ML Engineer (Biotech)** — data pipeline and system architecture practices relevant to section 5's discussion of integrating digital and wet-lab system components.

## Staying Current
- This field's cost curves (particularly DNA synthesis cost) and technical state-of-the-art are moving targets — recent conference proceedings, preprints, and company announcements are more reliable sources of current figures than any static reference, including this repo.
- Because this is a young and comparatively small field, be prepared for interviewers at different organizations to have meaningfully different views on which specific technical approaches (e.g., specific encoding schemes, synthesis technologies) represent the most promising path forward — this repo aims to present balanced, foundational understanding rather than advocate for any single organization's specific technical approach.
