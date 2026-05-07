It is great to see you reaching the final stages of your project, Lama! [cite_start]You have done a significant amount of work transitioning from raw PDB data to a validated Hidden Markov Model. [cite: 1, 167, 220, 421]

Below is a structured **README.md** tailored for your GitHub repository. [cite_start]It bridges the technical "Steps" you performed with the formal "Project Overview" from your instructor, making it clear to any visitor exactly what you achieved. [cite: 1, 421]

---

# Kunitz Domain Detection Pipeline: A Structure-Guided HMM Approach

## Project Overview
[cite_start]This project establishes a bioinformatics pipeline to detect the **Kunitz-type protease inhibitor domain** (Pfam ID: `PF00014`) in protein sequences. [cite: 6] [cite_start]By leveraging **Profile Hidden Markov Models (HMMs)** built from high-resolution 3D structural data, the pipeline provides a probabilistic "fingerprint" capable of identifying remote homologs that standard sequence-based searches might miss. [cite: 171, 175, 179]

### Author
[cite_start]**Lama Al Wannas** [cite: 421]  
MSc Bioinformatics – University of Bologna  
*Project based on methodology by Mohsin Nazir Bhat*

---

## Technical Workflow

### 1. Data Collection & Structural Filtering
* [cite_start]**Source:** RCSB Protein Data Bank (PDB). [cite: 3]
* [cite_start]**Filters applied:** * Annotation: Pfam ID `PF00014` [cite: 4]
    * [cite_start]Resolution: $\le 3.5$ Å (ensuring high structural quality) [cite: 8]
    * [cite_start]Sequence Length: $40 < L < 80$ (optimized for monodomains) [cite: 9, 11]
* [cite_start]**Redundancy Reduction:** Sequences were clustered using **MMseqs2** (minimum identity 0.95, coverage 0.9) to ensure a diverse training set. [cite: 53, 56, 61]

### 2. Structural Alignment & Model Building
* [cite_start]**Alignment Tool:** `mTM-align` was used to perform multiple structural alignments on representative chains. [cite: 93, 155]
* [cite_start]**HMM Construction:** The structural alignment (`kunitz.ali`) was converted into a probabilistic model using `hmmbuild`. [cite: 167, 171]
* [cite_start]**Validation:** The model's signature was visualized using **Skylign**, confirming the conservation of the **six essential Cysteines** (positions 5, 14, 30, 38, 51, and 55) and the **Y-F-Y** hydrophobic core. [cite: 203, 215, 218]

### 3. Validation & Performance Evaluation
[cite_start]The model was tested against the **Swiss-Prot** database (Reviewed entries): [cite: 221]
* [cite_start]**Positive Set:** 398 Kunitz-containing proteins. [cite: 227]
* [cite_start]**Negative Set:** 574,229 non-Kunitz proteins. [cite: 227]
* [cite_start]**Strategy:** 2-fold cross-validation with an optimized E-value threshold. [cite: 229, 305]

---

## Key Performance Metrics
[cite_start]The pipeline evaluates the model using **Matthews Correlation Coefficient (MCC)**, which is the gold standard for imbalanced biological datasets. [cite: 369, 374]

| Metric | Set 1 | Set 2 |
| :--- | :--- | :--- |
| **Optimal E-value** | $1e^{-07}$ | $1e^{-05}$ |
| **Max MCC** | 1.0 | 0.989 |
| **Accuracy (Q2)** | 1.0 | 0.999 |

> [cite_start]**Note on False Negatives:** Analysis revealed that "missed" sequences (e.g., `KUNI_ORNKA`) were often divergent tick toxins or nematode proteins that deviated from the canonical monodomain fingerprint. [cite: 442, 448, 451]

---

## Required Tools
To run this pipeline, install the following via Conda:
```bash
conda install -c bioconda hmmer cd-hit blast muscle
conda install -c conda-forge biopython mtm-align
```

## How to Run
1.  [cite_start]**Extract:** Run `get_chain.py` to isolate specific protein chains from PDB files. [cite: 104]
2.  [cite_start]**Align:** Use `mtm-align` on your representative seeds. [cite: 154]
3.  [cite_start]**Build:** Create the model with `hmmbuild kunitz.hmm kunitz.ali`. [cite: 167]
4.  [cite_start]**Search:** Validate using `hmmsearch` against your positive and negative FASTA sets. [cite: 231, 239]
5.  [cite_start]**Evaluate:** Run `python performance.py` to calculate MCC and Accuracy. [cite: 310]

---



Does this cover everything you need for the Readme, or would you like me to expand on any specific script instructions?

