# Kunitz Domain Detection Pipeline: A Structure-Guided HMM Approach

## Project Overview
This project establishes a bioinformatics pipeline to detect the **Kunitz-type protease inhibitor domain** (Pfam ID: `PF00014`) in protein sequences.  By leveraging **Profile Hidden Markov Models (HMMs)** built from high-resolution 3D structural data, the pipeline provides a probabilistic "fingerprint" capable of identifying remote homologs that standard sequence-based searches might miss. 

### Author
**Lama Al Wannas**  
MSc Bioinformatics – University of Bologna  
---

## Technical Workflow

### 1. Data Collection & Structural Filtering
* **Source:** RCSB Protein Data Bank (PDB). 
* **Filters applied:** * Annotation: Pfam ID `PF00014` 
    * Resolution: ≤ 3.5 Å (ensuring high structural quality) 
    * Sequence Length: 40 < L < 80 (optimized for monodomains) 
* **Redundancy Reduction:** Sequences were clustered using **MMseqs2** (minimum identity 0.95, coverage 0.9) to ensure a diverse training set. 

### 2. Structural Alignment & Model Building
* **Alignment Tool:** `mTM-align` was used to perform multiple structural alignments on representative chains. 
* **HMM Construction:** The structural alignment (`kunitz.ali`) was converted into a probabilistic model using `hmmbuild`. 
* **Validation:** The model's signature was visualized using **Skylign**, confirming the conservation of the **six essential Cysteines** (positions 5, 14, 30, 38, 51, and 55) and the **Y-F-Y** hydrophobic core. 

### 3. Validation & Performance Evaluation
The model was tested against the **Swiss-Prot** database (Reviewed entries): 
* **Positive Set:** 398 Kunitz-containing proteins. 
* **Negative Set:** 574,229 non-Kunitz proteins. 
* **Strategy:** 2-fold cross-validation with an optimized E-value threshold. 

---

## Key Performance Metrics
The pipeline evaluates the model using **Matthews Correlation Coefficient (MCC)**, which is the gold standard for imbalanced biological datasets.

| Metric | Set 1 | Set 2 |
| :--- | :--- | :--- |
| **Optimal E-value** | 1e -07 | 1e-05 |
| **Max MCC** | 1.0 | 0.989 |
| **Accuracy (Q2)** | 1.0 | 0.999 |

> **Note on False Negatives:** Analysis revealed that "missed" sequences (e.g., `KUNI_ORNKA`) were often divergent tick toxins or nematode proteins that deviated from the canonical monodomain fingerprint. 

---

## Required Tools
To run this pipeline, install the following via Conda:
```bash
conda install -c bioconda hmmer cd-hit blast muscle
conda install -c conda-forge biopython mtm-align
```

## How to Run
1.  **Extract:** Run `get_chain.py` to isolate specific protein chains from PDB files. 
2.  **Align:** Use `mtm-align` on your representative seeds. 
3.  **Build:** Create the model with `hmmbuild kunitz.hmm kunitz.ali`. 
4.  **Search:** Validate using `hmmsearch` against your positive and negative FASTA sets. 
5.  **Evaluate:** Run `python performance.py` to calculate MCC and Accuracy. 

---

