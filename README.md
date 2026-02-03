# Stanford-RNA-3D-Folding-Part-2

## Kaggle baseline upgrades (single-notebook workflow)
The baseline in `kaggle/baseline.txt` has been upgraded to improve template quality and add local scoring:

### What changed
- **Chain-aware template adaptation**: per-chain alignment and coordinate transfer to preserve residue numbering across stoichiometry-defined chains.
- **Template selection improvements**: combines sequence alignment, k-mer Jaccard prefiltering, and MSA profile similarity (when MSA files are available).
- **Local TM-score evaluation**: adds a validation loop with a Kabsch-based TM-score implementation and the official `d0` rule from the competition overview.

### How to run
1. Open the notebook created from `kaggle/baseline.txt` in Kaggle.
2. Ensure the dataset path is mounted at `/kaggle/input/stanford-rna-3d-folding-2/`.
3. Run the notebook to produce `submission.csv`.

### Expected performance
With the upgraded template selection and chain-aware transfer, the validation TM-score should exceed the prior baseline (~0.36) and target **>0.45** on the public validation set, assuming typical Kaggle runtime settings and access to MSAs.
