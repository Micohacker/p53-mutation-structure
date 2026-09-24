# Where does cancer break p53?
### A structural analysis of TP53 tumour mutations

**Mohammad Elnakoury** · Life Sciences Gateway, McMaster University · September 2026

## Question
p53 is the most frequently mutated gene in human cancer, and unlike most tumour suppressors it is usually inactivated by single amino acid substitutions (missense mutations) rather than truncations. This project asks: **where on the p53 protein do these cancer mutations physically fall?** Specifically, do they concentrate at the surface that contacts DNA, in the buried core that holds the protein's fold together, or neither?

## Data and methods
- **Mutations:** All TP53 mutations from the 32 studies of the TCGA PanCancer Atlas, retrieved through the cBioPortal API (4,225 mutations). After keeping only missense mutations and removing duplicates, 2,731 mutations from 2,544 tumours across 30 cancer types remained.
- **Structure:** Crystal structure of the p53 DNA-binding domain bound to DNA (PDB 1TSR). Analysis used the p53 copy bound to DNA (chain [B]), covering 194 residues.
- **Measurements:** For every residue, I calculated (1) its minimum distance to any DNA atom and (2) its relative solvent exposure (Shrake-Rupley algorithm, normalized to theoretical maximum values from Tien et al., 2013).
- **Zones:** Residues were classified as *DNA contact* (≤5 Å from DNA), *buried core* (<20% exposed), or *exposed surface*.
- **Statistics:** Residues mutated in 20 or more tumours ("hotspots") were compared against all other residues using Mann-Whitney U tests.
- **Tools:** Python (pandas, Biopython, matplotlib, SciPy, py3Dmol) in Google Colab.

## Results

**1. Mutations concentrate in the DNA-binding domain, dominated by a few hotspots.**
The most frequently mutated residues were R273 (267 tumours), R248 (224) and R175 (167), matching the classic p53 hotspots reported in the literature. Residues C176 and H179, two of the four residues that coordinate p53's structural zinc ion, were also among the top 10.

![Lollipop plot](figures/fig1_lollipop.png)

**2. Hotspots are closer to DNA and more buried than other residues.**
The 37 hotspot residues sat at a median of 10.1 Å from DNA versus 19.1 Å for other residues (p = 0.0001), and had a median solvent exposure of 6% versus 28% (p < 0.0001).

![Distance vs burial](figures/fig2_distance_vs_burial.png)

**3. DNA-contact and buried-core residues are strongly enriched for mutations.**

| Zone | Residues | % of residues | % of mutations |
|---|---|---|---|
| DNA contact | 18 | 9.3% | 26.7% |
| Buried core | 83 | 42.8% | 60.7% |
| Exposed surface | 93 | 47.9% | 12.6% |

DNA-contact residues carry about three times their expected share of mutations, while exposed surface residues carry about a quarter of theirs. Together, the contact and core zones account for about half of the structure but 87% of mutations.

![3D mutation frequency](figures/fig4_mutation_frequency_3d.png)

## Interpretation
These results are consistent with the two known classes of p53 mutation: **contact mutants** (e.g. R248, R273), which remove residues that directly grip DNA, and **structural mutants** (e.g. R175, Y220, the zinc-binding residues), which destabilize the protein's fold. In both cases p53 can no longer bind its target genes and activate cell-cycle arrest or apoptosis. Mutations on the exposed surface, which disrupt neither function, are rarely selected for in tumours.

## Limitations
- The crystal structure covers only the DNA-binding domain and captures a single static conformation.
- The thresholds used (5 Å, 20% exposure, 20 tumours) are choices; results should be checked across other cutoffs.
- Mutation frequency reflects both functional impact and how easily a site mutates (e.g. CpG sites in arginine codons), not function alone.
- Structural proximity or burial does not by itself prove a mutation's mechanism; that requires experimental data.
- TCGA over-represents certain cancer types and patient populations.

## Repository contents
- `01_mutation_data.ipynb`: downloading, cleaning and summarizing mutations
- `02_structure.ipynb`: structural measurements, statistics and figures
- `data/`: cleaned datasets
- `figures/`: all figures

## References
- Cho Y, Gorina S, Jeffrey PD, Pavletich NP. Crystal structure of a p53 tumor suppressor-DNA complex. *Science*. 1994;265:346–355.
- Tien MZ, Meyer AG, Sydykova DK, Spielman SJ, Wilke CO. Maximum allowed solvent accessibilities of residues in proteins. *PLoS ONE*. 2013;8:e80635.
- Data from the TCGA PanCancer Atlas, accessed via cBioPortal (cbioportal.org).
