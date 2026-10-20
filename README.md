# cnt-protein-corona
Corona enrichment analysis of LC-MS/MS proteomics of chemically modified single-walled carbon nanotubes in human plasma.

Source data and statistical results for:
> Miller, J., Coscia, A., Nadeem, A. & Kim, M. *Structure-affinity correlations and separable optical activity in carbon nanotube protein coronas.* (2026). 
DOI: to be added

School of Chemistry and Biochemistry, Georgia Institute of Technology. Corresponding author: Mijin Kim (mkim445@gatech.edu).

---

## Study in brief

We profiled the human-plasma protein corona and the near-infrared fluorescence response of a library of 25 chemically modified (6,5) single-walled carbon nanotubes (SWCNTs). The library is 5 non-covalent polymer wrappings * 5 covalent quantum-well-defect (QWD) types. Corona proteins were quantified by TMT-labelled LC-MS/MS relative to a pooled-plasma reference channel.

For every identified protein we computed 97 physicochemical features. These were related to corona enrichment in two ways:
- per-protein Pearson correlations;
- enrichment-weighted corona descriptors, one per nanotube construct.

The variance in corona composition and in fluorescence response was partitioned into polymer, defect, interaction and residual terms, with permutation tests.

This repository contains the processed data behind Figures 2–4 and the related Supplementary Figures.

## Repository contents

| File | What it contains | Used in |
|---|---|---|
| `All Pearson Correlations.xlsx` | Pearson correlations between each of 97 protein features and log2 fold change (corona / plasma) across all identified proteins, for each construct and polymer-group ratio | Fig. 3a; Supplementary Fig. 11 |
| `Figure 3 raw data.xlsx` | Enrichment-weighted corona descriptor (one value per feature) for each of the 25 constructs | Fig. 3b–d; Supplementary Figs. 12–13 |
| `VP_Proteomics_Permutation_Results.xlsx` | Variance partitioning of corona composition (all 25 constructs) with permutation null distributions | Fig. 2b |
| `VP_Proteomics_No Pristine_Permutation_Results.xlsx` | Same analysis excluding pristine (unfunctionalised) nanotubes | Supplementary Fig. 9 |
| `E11 dwl Permutation Results.xlsx` | Variance partitioning of the E11 emission wavelength shift after plasma incubation | Fig. 4d |
| `E11 dint Permutation Results.xlsx` | Same, E11 intensity change | Fig. 4d |
| `E11- dwl Permutation Results.xlsx` | Same, E11⁻ (defect) emission wavelength shift | Fig. 4d |
| `E11- dint Permutation Results.xlsx` | Same, E11⁻ intensity change | Fig. 4d |

All files are Excel workbooks (`.xlsx`).

## Nanotube library and naming

**Polymer wrappings (5)**

| Manuscript | Labels in files | Class |
|---|---|---|
| (AT)15 ssDNA | `(AT)15` | DNA, 30-nt |
| (GT)15 ssDNA | `(GT)15` | DNA, 30-nt |
| CT2C3T2C ssDNA | `CT2C3T2C` | DNA, 9-nt |
| DSPE-PEG(2000)-amine | `DSPE` | PEG |
| DMG-PEG 2000 | `DMG` | PEG |

**Quantum-well defects (5)**

| Manuscript | Labels in files |
|---|---|
| 3,4,5-trifluoroaryl | `Trifluoro` |
| 4-carboxyaryl | `Carboxyl` |
| N,N-diethyl-4-aminoaryl | `Amino` |
| 3,5-dinitroaryl | `Dinitro` |
| unfunctionalised | `Pristine` |

Construct IDs in `Figure 3 raw data.xlsx` take the form `<Defect>_<Polymer>`, e.g. `Carboxyl_(GT)15`.

## File descriptions

### `All Pearson Correlations.xlsx`

**Layout:**
- One sheet per defect: `Trifluoro`, `Carboxyl`, `Dinitro`, `Amino`, `Pristine`.
- Each sheet is a set of side-by-side column blocks. Each block is `feature | correlation | p_value`, separated by an empty column.
- The block header names the corona or ratio being correlated:

| Block | Response variable correlated with each feature |
|---|---|
| `(AT)15`, `CT2C3T2C`, `DMG`, `DSPE`, `(GT)15` | log2(corona / plasma) for that construct |
| `All/Plasma` | log2 of the mean over all five polymers relative to plasma |
| `DNA/Plasma`, `PEG/Plasma` | log2 of the DNA-group or PEG-group mean relative to plasma |
| `DNA/PEG` | log2 ratio of the DNA-group mean to the PEG-group mean |
| `30mer/9mer` | log2 ratio of the (AT)₁₅/(GT)₁₅ mean to CT₂C₃T₂C |
| `DSPE/DMG` | log2 ratio of DSPE-PEG to DMG-PEG |

**Notes:**
- `correlation` is Pearson's r across all proteins with a valid fold change and feature value. `p_value` is the two-sided p for r. No multiple-testing correction is applied; Fig. 3a shows p < 0.05.
- The `Dinitro` sheet has no `30mer/9mer` or `DSPE/DMG` blocks.
- Each block has 97 feature rows (see *Protein features* below).

### `Figure 3 raw data.xlsx`

One sheet, `Fig3_Weighted_Corona_Features`.

| Columns | Content |
|---|---|
| `SampleID`, `Defect`, `Polymer`, `Polymer_Class` | Construct identifiers; `Polymer_Class` is `DNA` or `PEG` |
| 102 feature columns | Enrichment-weighted descriptor for each feature |

**Rows:** 25, one per construct.

**Weighted descriptor:** for each construct *j* and feature *f*, each corona is collapsed into a single descriptor by weighting each protein's feature value by its fold change relative to plasma:

$$F_{f,j}=\frac{\sum_{i=1}^{N_j} FC_{i,j}\,F_{f,i}}{\sum_{i=1}^{N_j} FC_{i,j}}$$

- *N*ⱼ is the number of proteins identified in construct *j*.
- *FC*ᵢ,ⱼ is the abundance ratio of protein *i* in construct *j* relative to plasma.
- *F*f,ᵢ is the value of feature *f* for protein *i*.

Highly enriched proteins therefore contribute most. Values are on the scale of the original feature (e.g. pI in pH units, fractions between 0 and 1).

**Feature columns:**
- These are the 97 features of the correlation file, with five AlphaFold-derived structural features added (`AF: pLDDT`, `AF: SASA`, `AF: Rg`, `AF: Anisotropy`, `AF: Dipole (Debye)`) plus `% Nonpolar`.
- `% Neutral Polar` is not included.

**Statistics used in the figures:**
- Group comparisons (DNA vs PEG, QWD vs pristine): unpaired, two-sided Mann-Whitney tests.
- QWD chemical descriptors (Hammett constant, hydrodynamic size, H-bond capacity, logP): Spearman correlations.

### Variance-partitioning files (`VP_*` and `E11*`)

All six workbooks share one layout.

| Sheet | Content |
|---|---|
| `Summary` | Variance fractions (`Polymer_effect`, `Defect_effect`, `Interaction_effect`, `Noise`), sums of squares (`SS_total`, `SS_polymer`, `SS_defect`, `SS_interaction`, `SS_noise`) and permutation p-values (`p_polymer`, `p_defect`, `p_interaction`) |
| `Effect_Sizes` | The four variance fractions only |
| `SS_Components` | The five sums of squares only |
| `Null_Distributions` | Variance fraction of each term in every permuted dataset (`polymer_null`, `defect_null`, `interaction_null`); 500 rows, one per permutation |

**Model:**
- A two-factor model with categorical polymer and defect predictors and their interaction.
- Sums of squares are sequential: polymer first, then defect, then interaction. Each fraction is SS(term) / SS(total).
- **Proteomics (`VP_*`):** the response is the multivariate log2 fold-change matrix of the 289 proteins quantified in all coronas, with triplicate measurements per construct.
- **Fluorescence (`E11*`):** each spectral feature is modelled separately because wavelength and intensity have different units. `dwl` = peak wavelength shift (nm); `dint` = intensity change after incubation in 20% plasma; triplicate measurements. E11 is the intrinsic (6,5) emission; E11⁻ is the defect-state emission.

**Permutation test:**
- Polymer and defect labels were permuted independently to build null distributions for the main effects.
- The interaction term used a Freedman–Lane scheme.
- p-values are (k + 1)/(N + 1), where k is the number of permuted fractions ≥ the observed fraction. With N = 500, the smallest attainable p is 1/501 ≈ 0.002.

**Headline results** (from the `Summary` sheets):

| Response | Polymer | Defect | Interaction | Residual |
|---|---|---|---|---|
| Corona composition, all constructs | 25.1% (p = 0.002) | 47.3% (p = 0.002) | 15.0% (p = 0.002) | 12.6% |
| Corona composition, pristine excluded | 31.7% (p = 0.002) | 38.5% (p = 0.002) | 14.9% (p = 0.002) | 14.9% |
| E11 wavelength shift | 53.8% (p = 0.002) | 10.6% (p = 0.08) | 35.3% (p = 0.002) | 0.7% |
| E11 intensity change | 47.4% (p = 0.002) | 9.8% (p = 0.17) | 42.4% (p = 0.002) | 2.7% |
| E11⁻ wavelength shift | 91.7% (p = 0.002) | 2.6% (p = 0.70) | 6.1% (p = 0.002) | 0.2% |
| E11⁻ intensity change | 67.6% (p = 0.002) | 7.7% (p = 0.22) | 26.4% (p = 0.002) | 0.7% |

## Protein features

97 features per protein, in four categories:

| Category | Features | Source |
|---|---|---|
| Global | `# AAs`, `MW [kDa]`, `pI`, `GRAVY`, `Instability Index`, `% Aromatic` | BioPython `ProtParam`, from UniProt sequences |
| Structural / flexibility | `Flex: Mean / S.D. / Var / Max / Min / Median`, `SS: Helix / Turn / Sheet` (sequence-based propensities); `% Helix`, `% Strand`, `% Other` (predicted secondary structure) | BioPython; NetSurfP-3.0 |
| Surface-exposed | `% Amino Acids Exposed`, `RSA: Mean / Median / S.D.`, `ASA: Sum`; `% Exp/All X` and `% Exp/Exp X` for each amino acid and residue class (nonpolar, polar, positive, negative, aromatic) | NetSurfP-3.0 |
| Compositional | `% X` for each of the 20 amino acids; `% Positive`, `% Negative`, `% Polar`, `% Neutral Polar` | BioPython |

**Definitions:**
- **Exposed residue:** relative solvent accessibility (RSA) ≥ 0.25.
- **`% Exp/All X`:** exposed residues of type X ÷ all residues.
- **`% Exp/Exp X`:** exposed residues of type X ÷ all exposed residues.
- **Residue classes:**
  - nonpolar = G, A, V, L, I, M, F, W, P;
  - polar = S, T, N, Q, C, Y, D, E, K, R, H;
  - positive = R, H, K;
  - negative = D, E;
  - aromatic = F, Y, W, H (for the exposed-aromatic features).
- **`AF:` features** (in `Figure 3 raw data.xlsx` only) come from AlphaFold DB models: mean pLDDT, Shrake–Rupley SASA, radius of gyration, inertia anisotropy, and an estimated dipole moment.

## Raw data

LC-MS/MS raw files and Proteome Discoverer 3.2 (Chimerys) search results are available from the corresponding author upon reasonable request.

Fluorescence spectra were processed with custom MATLAB (R2024b) code: Lorentzian peak fitting and background subtraction.

## Code

Statistical analyses were run in Python 3.12 with pandas 2.0, NumPy 1.24 and SciPy 1.11. Figures were made in GraphPad Prism 11.0.0 and BioRender.

The analysis scripts (feature extraction, weighted descriptors, correlations, variance partitioning) are available from the corresponding author upon reasonable request.

## How to cite

If you use these data, please cite the article above.

## License

The processed data files in this repository are released under the [Creative Commons Attribution 4.0 International License (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/). You may share and adapt them for any purpose, provided you cite the article above and indicate any changes you made.

The licence covers only the files in this repository. It does not cover the LC-MS/MS raw data or the analysis code, which are not distributed here; requests should go to the corresponding author.

## Contact

Mijin Kim, School of Chemistry and Biochemistry, Georgia Institute of Technology. mkim445@gatech.edu
