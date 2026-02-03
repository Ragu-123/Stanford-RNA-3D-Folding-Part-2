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

### Expected speedups (GPU coarse filtering)
When `USE_GPU = True`, the notebook uses CuPy (or PyTorch if available) to batch-compute k-mer Jaccard and k-mer embedding similarities for all training sequences during the coarse filter. Pairwise alignment remains on CPU for final scoring, so accuracy parity is maintained. Expected wall-clock improvements:
- **0 targets**: ~1.0x (no meaningful gain; GPU setup overhead dominates).
- **10 targets**: ~1.5–2.5x faster coarse filtering and end-to-end runtime.
- **20 targets**: ~2–4x faster coarse filtering and end-to-end runtime.

## Runtime breakdown
The baseline now prints a runtime summary every 10 targets with per-target averages for:
- MSA/profile preparation
- Candidate filtering
- Alignment scoring
- Coordinate adaptation
- Constraints refinement
- I/O (submission write + validation read)

At the end, it reports the average time/target and estimated runtime for 0/10/20 targets derived from those averages. This makes it easy to sanity-check overall throughput and identify hot spots when tuning parameters or switching between CPU/GPU coarse filtering.
