# 1st TRF Summer School Project

Lecture and hands-on project code for the **1st TRF Summer School**, in collaboration with the **EU ChronoPilot project**.

The materials explore reinforcement learning, evidence accumulation, cognitive load, and subjective time through a multi-armed bandit task. The project extends these ideas with a controller that selects an agent's engagement level and compares its behavior with random engagement.

## Repository contents

| File | Description |
| --- | --- |
| `lecture_part_2.ipynb` | Lecture exercises: an interactive bandit task, leaky competing accumulator simulations, and experiments on option values, number of alternatives, learning, and uncertainty. |
| `Controller_demo.ipynb` | Project demonstration comparing controlled and random engagement using reaction times, choices, and predicted subjective time. |
| `pyeam.py` | Implementation of the leaky competing accumulator (`LCA`) and plotting helpers. |
| `pyrleam.py` | Reinforcement Learning and Evidence Accumulator Model (`RLEAM`), including learning rules, simulations, and the interactive bandit task. |
| `data.csv` | Trial data with participant, block, option, reward, reaction-time, and accuracy fields. The controller includes an inactive example for extracting reward distributions from it; the active examples define rewards directly. |
| `requirements.txt` | Python dependencies and Jupyter tools. |

## Setup

Use Python 3.10 as a starting point: the lecture notebook records Python 3.10.14 in its metadata. Run the following commands from the repository directory.

### Windows (PowerShell)

```powershell
py -3.10 -m venv .venv
.\.venv\Scripts\python.exe -m pip install --upgrade pip
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
.\.venv\Scripts\python.exe -m ipykernel install --user --name trf-summer-school --display-name "Python (TRF Summer School)"
.\.venv\Scripts\python.exe -m jupyterlab
```

### macOS / Linux

```bash
python3.10 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
python -m ipykernel install --user --name trf-summer-school --display-name "Python (TRF Summer School)"
python -m jupyterlab
```

Select **Python (TRF Summer School)** as the notebook kernel. The saved notebooks may refer to the original author's environment, which is not included here.

The local modules `pyeam` and `pyrleam` are loaded directly from this directory. The `ssms` import is supplied by **ssm-simulators**. Its version is pinned to [0.7.8, which documents the legacy simulator import](https://pypi.org/project/ssm-simulators/0.7.8/) used in this code. These requirements are a dependency list, not a fully tested environment lockfile.

## Working through the materials

1. Open `lecture_part_2.ipynb` and run cells in order. Later sections reuse variables and helper functions from earlier cells.
2. The `model.PlayMultiArmedBandit()` cell waits for keyboard input. Enter `q`, `w`, `e`, or `r` when prompted, or skip this cell to proceed directly to the simulations.
3. Explore the effects of accumulator inputs, learning parameters, and reward uncertainty on reaction times and choice accuracy.
4. Open `Controller_demo.ipynb`, apply the import workaround below, and run its cells in order to compare controlled and random engagement.

For shorter exploratory runs, reduce `n_sims` in the LCA examples or participant counts (`n_par` / `num_par`). If changing trial counts, preserve the divisibility conditions in each notebook's `Generate_trials` helper.

The simulations use random sampling. To make the NumPy and Python random streams repeatable, add this cell before generating trials or running simulations:

```python
import random
import numpy as np

random.seed(42)
np.random.seed(42)
```

External simulators may manage additional random state.

## Controller import workaround

The controller defaults to `eam_model="race"`, but the `simulator` import in `pyrleam.py` is commented out. Importing it in the notebook alone does not define it inside that module, so the current race path raises `NameError`.

Before running the controller simulations, add and execute this notebook cell:

```python
import pyrleam
from ssms.basic_simulators.simulator import simulator

pyrleam.simulator = simulator
```

Alternatively, uncomment the existing simulator import near the top of `pyrleam.py` and restart the kernel. The lecture's LCA examples do not require this workaround.
