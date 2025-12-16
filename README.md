# Predicting Translation Efficiency from Synonymous Coding Sequences Using Codon-Level Embeddings

This repository contains code and analysis for benchmarking **codon-level sequence representations** in predicting **translation efficiency** from synonymous coding sequences. The work focuses on isolating **translation elongation–driven effects** by comparing shallow and transformer-based codon embeddings on a controlled experimental dataset.

## Overview

Synonymous codon substitutions do not alter the amino-acid sequence of a protein, yet they can lead to large differences in protein expression due to effects on translation elongation, mRNA stability, and co-translational folding. Accurately modelling these effects is important for:

- Gene expression and translational regulation research  
- Synthetic biology and protein engineering  
- mRNA-based therapeutics and vaccine design  

In this project, we benchmark two codon-level embedding approaches:

- **CodonVE** — a Word2Vec-style embedding that captures local codon co-occurrence inspired by the work of Codon2Vec  
- **CodonFM** — a transformer-based foundation model trained on large-scale coding sequence corpora  

Both methods are evaluated using a controlled synonymous-variant dataset to assess how well their embeddings predict translation efficiency.

## Dataset

The experiments use the **mRFP synonymous-codon expression dataset** introduced by Goodman et al. (2013).

Key characteristics:
- 1,459 coding-sequence variants
- Identical amino-acid sequence across all variants
- Fixed promoter, ribosome binding site, and 5′UTR
- Variation only in synonymous codon usage within the CDS
- Protein expression measured via fluorescence (proxy for translation efficiency)
- Predefined training, validation, and test splits

This design isolates **elongation-related effects** while minimising variation due to translation initiation.

## Methodology

1. **Preprocessing**
   - CodonVE: sequence cleaning, codon tokenisation, Word2Vec training
   - CodonFM: direct encoding using a pretrained codon-level transformer

2. **Embedding Generation**
   - CodonVE: 100-dimensional sequence embeddings via mean pooling
   - CodonFM: 2048-dimensional contextual sequence embeddings

3. **Predictive Modelling**
   - Regression models: Random Forest
   - Evaluation metrics: R² and Spearman correlation

4. **Analysis**
   - Predictive performance comparison
   - Embedding structure and biological plausibility
   - Interpretation under fixed initiation conditions

## Reference Implementation

The CodonFM experiments are based on the official NVIDIA Digital Biology notebook:

- **NVIDIA CodonFM – EnCodon Downstream Task (mRFP Expression)**  
  https://github.com/NVIDIA-Digital-Bio/CodonFM/blob/main/notebooks/5-EnCodon-Downstream-Task-mRFP-expression.ipynb

Our work extends this reference by performing a systematic benchmark comparison against CodonVE using identical data splits and evaluation metrics.

## Key Findings

- CodonFM embeddings consistently outperform CodonVE across all metrics.
- Transformer-based contextual modelling captures elongation-related regulatory signals beyond local codon co-occurrence.
- Long-range and positional dependencies within coding sequences are important for predicting translation efficiency.

## Repository Structure

```
├── notebooks/
│   ├── codonVE_mRFP.ipynb
│   ├── codonFM_mRFP.ipynb
├── data/
│   └── mRFP_Expression.csv
├── results/
│   └── figures_and_tables/
├── README.md
├── LICENSE
```

## Dependencies

- Python 3.9+
- PyTorch
- Gensim
- Scikit-learn
- XGBoost
- NumPy / Pandas
- Matplotlib

(See notebooks for exact versions.)

## License

This project is released under the **Apache License 2.0**.

- Free for academic and commercial use
- Includes an explicit patent grant
- Suitable for open research and industry use

## Citation

If you use this work, please cite:

- Goodman et al. (2013) — mRFP synonymous-codon dataset  
- Wint et al. (2021) — Codon2Vec  
- NVIDIA Digital Biology (2024) — CodonFM  
- This repository — benchmark comparison and analysis

## Future Work

- Joint modelling of translation initiation and elongation using variable 5′UTR datasets
- Integration of ribosome profiling and mRNA stability data
- Task-specific fine-tuning of codon-level transformer models
- Applications to mRNA therapeutic and vaccine design
