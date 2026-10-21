# HMIA Ethics notebooks

Teaching notebooks and live pages for the ethics module of **Human and Machine Intelligence Alignment (HMIA)** at Johns Hopkins.

## What's here

| File | What it is |
|---|---|
| `Minimal_Agent_Harness.ipynb` | A minimal agent harness, built box by box: the agent is the loop, the LLM is a function it calls |
| `Agent_Harness_Ethical_Oracles.ipynb` | The harness consults a "perfect" ethical oracle (consequentialist, deontologist, virtue) |
| `Scout_Crosswalk_RL.ipynb` | Part 1. A scout at a crosswalk: reinforcement learning, rewards as weighted features, and inverse RL (elimination and Bayesian) |
| `Scout_Crosswalk_CIRL.ipynb` | Part 2. A helper robot learns to "fit in": cooperative inverse RL and shared meaning (Marx, Mead) |
| `*-notebook.md` | Notes on each notebook: design, results, exercises |
| `live/` | Quarto pages where the notebooks' code runs in the browser, with sliders and no visible code |
| `requirements.txt` | Python packages needed to run the notebooks locally |

## Running the notebooks

**In Google Colab:** open a notebook from this repository (File → Open notebook → GitHub). Nothing to install.

**On your own machine** (uses [uv](https://docs.astral.sh/uv/)):

```bash
uv venv ~/venvs/hmia --python 3.12
uv pip install --python ~/venvs/hmia/bin/python -r requirements.txt
```

Then open the notebooks in VS Code or Jupyter with that environment.

## The live pages

Requires [Quarto](https://quarto.org). From the `live/` folder:

```bash
quarto preview knob1.qmd
```

The page runs Python in the browser (via Pyodide), so the first load takes several seconds. It must be served (as `quarto preview` does), not opened directly as a file.
