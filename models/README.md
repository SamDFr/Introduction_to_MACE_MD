# MACE Models

Place the MACE checkpoint files in this directory.

The notebooks select the model with:

- `MODEL_PATH`
- `MODEL_HEAD`

Examples of files that can live here:

- `mace-mh-1.model`
- other MACE foundation model checkpoints
- your own fine-tuned `.model` files

If the model is a multi-head foundation model, set `MODEL_HEAD` in the notebook to the appropriate head. If the model is single-head, leave `MODEL_HEAD = None`.
