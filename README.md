# Introduction to MACE MD

This repository contains two generic notebooks for running molecular dynamics with MACE foundation models. The aim is to keep the repository simple while still making the notebooks usable as standalone teaching material and as a starting point for your own tests.

## Repository content

- `notebooks/01_mace_foundation_models.ipynb`
  Introduction to loading a structure, selecting a MACE model, optimizing if needed, and running simple MD.
- `notebooks/02_mace_molecule_surface_zpe.ipynb`
  Molecule-surface workflow with slab generation or loading, optional slab optimization, optional slab thermalization, ZPE initialization of the molecule, and final NVE scattering dynamics.
- `models/`
  Directory for MACE checkpoint files.
- `requirements.txt`
  Minimal Python dependencies for the notebooks.
- `outputs/`
  Runtime-generated files, separated by notebook in subdirectories.

## Environment setup

Create a local virtual environment and install the dependencies:

```bash
python3 -m venv .venv
source .venv/bin/activate
python3 -m pip install -r requirements.txt
```

Launch Jupyter from this environment so that `mace-torch`, `torch-dftd`, and `nglview` are available to the notebooks.

## MACE models

Place the MACE checkpoint files in `models/`. The notebooks are written so you can test different models by changing:

- `MODEL_PATH`
- `MODEL_HEAD`

Typical examples are:

- `mace-mh-1.model`
- other MACE foundation checkpoints
- your own fine-tuned `.model` files

`MODEL_HEAD` matters only for multi-head foundation models. In practice:

- `omat_pbe` is useful for general materials
- `oc20_usemppbe` is useful for surfaces and adsorbates
- `omol` and `spice_wB97M` are molecule-oriented heads

If a checkpoint is single-head, leave `MODEL_HEAD = None`.

## Notebook 1

`01_mace_foundation_models.ipynb` is the general introduction notebook.

Main features:

- load an ASE-readable structure or build a simple molecule
- optimize the geometry if requested
- save the optimized geometry if requested
- run MD with logging and saved trajectories
- export trajectories in `.xyz`
- inspect the trajectory at the end with an interactive viewer

Useful parameters to edit at the top of the notebook:

- `MODEL_PATH`, `MODEL_HEAD`, `DEVICE`, `DEFAULT_DTYPE`
- `STRUCTURE_SOURCE`, `STRUCTURE_PATH`, `STRUCTURE_FORMAT`
- `RUN_OPTIMIZATION`, `OPTIMIZATION_FMAX`, `OPTIMIZATION_MAX_STEPS`
- `NSTEPS`, `TIMESTEP_FS`, `TEMPERATURE_K`
- `LOG_INTERVAL`, `TRAJ_INTERVAL`

## Notebook 2

`02_mace_molecule_surface_zpe.ipynb` is the molecule-surface notebook.

The workflow is separated into three stages:

1. prepare the slab
2. thermalize the slab if needed
3. run the final NVE slab-molecule dynamics

Main features:

- build a slab directly in the notebook or load it from file
- default built slab is a `4x4x3` graphite slab
- support for ASE slab loading, including `POSCAR` with `SURFACE_FORMAT = "vasp"`
- optional slab optimization
- optional slab thermalization
- selection of the thermalized snapshot used to launch the final NVE run
- ZPE-based initialization of the incident molecule
- optional early stop of the NVE trajectory when the molecule reaches again its initial height above the surface
- export of the MD trajectory in `.xyz`
- optional interactive trajectory viewing if your notebook frontend handles `nglview` reliably

Important parameter groups:

- surface setup:
  `SURFACE_SOURCE`, `SURFACE_PATH`, `SURFACE_FORMAT`, `SLAB_BUILDER`, `SLAB_SYMBOL`, `SLAB_SIZE`, `SLAB_VACUUM`
- slab preparation:
  `OPTIMIZE_SURFACE`, `FIX_BOTTOM_LAYERS`, `BOTTOM_THICKNESS`
- slab thermalization:
  `THERMALIZE_SURFACE`, `THERMALIZE_TEMPERATURE_K`, `THERMALIZE_STEPS`, `THERMALIZED_SNAPSHOT_MODE`
- molecule launch:
  `MOLECULE_NAME`, `INCIDENT_ENERGY_EV`, `HEIGHT_ABOVE_SURFACE_A`, `INCIDENT_DIRECTION`, `RANDOM_ORIENTATION`
- final MD:
  `NSTEPS`, `TIMESTEP_FS`, `LOG_INTERVAL`, `TRAJ_INTERVAL`
- scattering criterion:
  `STOP_ON_SCATTER`, `SCATTER_HEIGHT_TOLERANCE`, `REQUIRE_DESCENT_BEFORE_STOP`

## Notes

- The notebooks save trajectories in both ASE `.traj` and exported `.xyz` form.
- Notebook 1 writes into `outputs/01_mace_foundation_models/`.
- Notebook 2 writes into `outputs/02_mace_molecule_surface_zpe/`.
- For periodic systems, center-of-mass drift is removed, but rigid-body `ZeroRotation` is skipped because it is not meaningful under PBC.
- Notebook 2 keeps the interactive viewer disabled in the saved file because it has been unstable in some notebook frontends.
