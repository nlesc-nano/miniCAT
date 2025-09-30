# miniCAT — Compound Attachment Tool (mini)

**miniCAT** is a lightweight tool to attach chemical ligands (from SMILES) to nanocrystal / QD structures.
It provides:
- RDKit-based functional-group detection and ionic transforms,
- geometry utilities to place/anchor ligands on specified “dummy” sites (e.g. Cl, Br, I, Rb),
- control over multi-ligand, multi-pass passivations with ratios and spatial distributions.

> This repository is a slimmed-down version of CAT (Compound Attachment Tool), focused on just the essentials.

---

## Installation (conda/mamba recommended)

Create and activate the environment:

```bash
mamba env create -f environment.yml
mamba activate minicat
```

Install the package in editable mode to get the `miniCAT` CLI:

```bash
pip install -e .
```

> **Note:** RDKit is provided via conda-forge in the environment; keeping it in the conda env avoids pip/rdkit compatibility issues.

---

## Quick start

Inputs:
- `initial_dot.xyz` — your QD/nanocrystal with “dummy” attachment sites (e.g. Cl, Rb).
- SMILES strings for ligands.

Example: two-pass passivation — first passivate Cl with a 25/75 segmented mix of two anionic ligands, then passivate Rb with a 40/60 random mix of two cationic ligands.

```bash
miniCAT   --qd initial_dot.xyz   --out_prefix final_passivated_dot   --job-ligands "CCCC(=O)O" "CCCCS"   --job-dummy "Cl"   --job-dist "0.25:0.75:segmented"   --job-ligands "C[NH3+]" "C(C)(C)N"   --job-dummy "Rb"   --job-dist "0.40:0.60:random"
```

Key flags (subset):
- `--qd` : input `.xyz` QD structure.
- `--out_prefix` : base filename for outputs.
- `--job-ligands` : one or more SMILES strings (repeatable per pass).
- `--job-dummy` : the target dummy species for that pass (e.g., `Cl`, `Br`, `I`, `Rb`).
- `--job-dist` : ratio and distribution strategy; format: `r1:r2:mode` where `mode ∈ {random, segmented}`.
- Multiple “passes” are specified by repeating the `--job-ligands/--job-dummy/--job-dist` triplet.

Special-cased inputs:
- Hydrogen halides can be given as `HCl`, `HBr`, `HI` (or just `Cl`, `Br`, `I`) and are normalized internally.
- `H2S` may be given as `S`.

---

## Package layout

```
minicat/
  main.py                        # CLI entry point, exposed as the `miniCAT` command
  functional_groups_class.py     # RDKit functional-groups & transforms
```

Run help:
```bash
miniCAT -h
```

---

## Development

Optional tooling:
```bash
mamba install black isort
```
Format:
```bash
black minicat
isort minicat
```

---

## License

See [LICENSE](./LICENSE).
