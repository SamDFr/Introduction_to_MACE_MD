# Introduction to MACE MD

Minimal repository for clean and generic MACE notebooks.

## Files kept on GitHub

- `notebooks/01_mace_foundation_models.ipynb`
- `notebooks/02_mace_molecule_surface_zpe.ipynb`
- `models/README.md`
- `requirements.txt`
- `.gitignore`

## Environment

Create a local environment yourself. It should not be committed.

```bash
python3 -m venv .venv
source .venv/bin/activate
python3 -m pip install -r requirements.txt
```

If you use the interactive trajectory viewer at the end of the notebooks, make sure the notebook is running from this environment so `nglview` is available.

## Models

Put local MACE checkpoints in `models/`, for example:

```text
models/
├── README.md
└── mace-mh-1.model
```

These `.model` files are ignored by Git.

## Notes

- The notebooks save MD trajectories in `.xyz`.
- The molecule-surface notebook can either load a slab from file or build one directly.
- The default slab is a built `4x4x3` graphite slab.
- For periodic systems, center-of-mass drift is removed, but rigid-body `ZeroRotation` is intentionally skipped because it is not meaningful under PBC and triggers ASE warnings.
