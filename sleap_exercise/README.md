# Extra: Raw SLEAP Output Files (optional)

[← Back to the main page](../README.md)

This folder is **optional practice and is not part of the graded project**.
The supervised task gives you the fly poses as a ready-made `.csv` table. Here you can work with the files that tables like that are made from: the `.h5` output of **SLEAP** after a human checked and corrected the tracking ("proofread"), and a file of features computed from it.
Use them to see where the `.csv` data come from, and to practice building features and doing statistics straight from the `.h5` files.

## The files

There are two experiments, each **30 minutes** of video at **150 frames per second** (about 270,000 frames), with one male and one female fly:

| Experiment | Female age |
|---|---|
| `01_04` | 1–4 hours after eclosion (emerging from the pupa) |
| `43_46` | 43–46 hours after eclosion |

Each experiment has two files:

| File | Size | Content |
|---|---|---|
| `01_04_000000.mp4.inference.cleaned.210809_124446_18159206_rig1_2.proofread.000_000000.analysis.h5` | 68 MB | SLEAP key points after proofreading (`01_04`) |
| `43_46_000000.mp4.inference.cleaned.210825_090819_18159206_rig1_2.proofread.000_000000.analysis.h5` | 73 MB | SLEAP key points after proofreading (`43_46`) |
| `01_04_features.h5` | 278 MB | Features extracted from the `01_04` key points |
| `43_46_features.h5` | 288 MB | Features extracted from the `43_46` key points |

## Download

The files are too large to be stored in the repository itself. They are attached to a **release** of this repository:

**➜ [Download page: SLEAP exercise data](https://github.com/deutschlab/computational-ethology-final-project/releases/tag/sleap-data-v1)** (click a file name under *Assets*)

In **Google Colab** or a terminal, download them directly into a folder named `sleap_data`:

```bash
BASE=https://github.com/deutschlab/computational-ethology-final-project/releases/download/sleap-data-v1
mkdir -p sleap_data && cd sleap_data
wget -q $BASE/01_04_000000.mp4.inference.cleaned.210809_124446_18159206_rig1_2.proofread.000_000000.analysis.h5
wget -q $BASE/43_46_000000.mp4.inference.cleaned.210825_090819_18159206_rig1_2.proofread.000_000000.analysis.h5
wget -q $BASE/01_04_features.h5
wget -q $BASE/43_46_features.h5
```

(In a Colab cell, put `!` in front of each line, and use `%cd sleap_data` instead of `cd sleap_data`.)

Or from Python, on any computer:

```python
import os, urllib.request

BASE = "https://github.com/deutschlab/computational-ethology-final-project/releases/download/sleap-data-v1/"
FILES = [
    "01_04_000000.mp4.inference.cleaned.210809_124446_18159206_rig1_2.proofread.000_000000.analysis.h5",
    "43_46_000000.mp4.inference.cleaned.210825_090819_18159206_rig1_2.proofread.000_000000.analysis.h5",
    "01_04_features.h5",
    "43_46_features.h5",
]
os.makedirs("sleap_data", exist_ok=True)
for name in FILES:
    path = os.path.join("sleap_data", name)
    if not os.path.exists(path):
        print("Downloading", name)
        urllib.request.urlretrieve(BASE + name, path)
```

You need the `h5py` package to read the files: `pip install h5py` (already installed in Colab).

## Inside an `analysis.h5` file

The main datasets (all names are keys inside the file):

| Dataset | Shape | Meaning |
|---|---|---|
| `tracks` | (2 flies, 2 coordinates x/y, 13 body parts, frames) | Position of every body part in pixels. Missing points are `NaN`. |
| `node_names` | 13 | Body-part names: head, thorax, abdomen, wingL, wingR, forelegL4, forelegR4, midlegL4, midlegR4, hindlegL4, hindlegR4, eyeL, eyeR |
| `track_names` | 2 | Name of each tracked fly |
| `edge_names`, `edge_inds` | 12 × 2 | The skeleton: which body parts are connected |
| `point_scores`, `instance_scores`, `tracking_scores` | | SLEAP's confidence values |
| `track_occupancy` | (frames, 2) | 1 where a fly was found in a frame |

Example: the thorax position of the first fly.

```python
import h5py

with h5py.File("sleap_data/43_46_000000.mp4.inference.cleaned.210825_090819_18159206_rig1_2.proofread.000_000000.analysis.h5", "r") as f:
    tracks = f["tracks"][()]                                  # (2, 2, 13, frames)
    node_names = [n.decode() for n in f["node_names"][()]]
    track_names = [n.decode() for n in f["track_names"][()]]

thorax = node_names.index("thorax")
x = tracks[0, 0, thorax, :]                                   # fly 0, x coordinate, thorax, all frames
y = tracks[0, 1, thorax, :]
print(track_names, x.shape)
```

To list everything inside any `.h5` file: `with h5py.File(path) as f: print(list(f.keys()))`.

---

## Notes from the data providers

The text below is the README that came with the data. The same text is also in [`SleapExcercise_README`](SleapExcercise_README).

> The first two are key point detection from Sleap after human proofreading.
> The other two contain feature that where extracted from the first two files.
>
> Here you can find example Sleap notebooks: https://docs.sleap.ai/latest/notebooks/notebooks-overview/
> Including this one - https://docs.sleap.ai/latest/notebooks/Analysis_examples/
>
> That can help you understanding the structure and usage of the analysis.h5 files.
>
> 1-4: the Drosophila female was 1-4 hour old (after eclosion/birth)
> 43-46: the Drosophila female was 43-46 hour old (after eclosion/birth)
>
> Example code (see more in the notebooks mentioned above; If you don't have h5py yet, install it with pip install h5py or conda install h5py.)-

```python
import h5py
import numpy as np
import matplotlib.pyplot as plt

F = '<path-to-file>/01_04_features.h5'
with h5py.File(F, 'r') as f:
    Dist = f['/mfDist'][()].squeeze()   # squeeze turns a 1×N array into a plain vector

fs = 150                                # sampling rate in Hz – replace with your frame rate
t = np.arange(len(Dist)) / fs           # time axis in seconds

plt.plot(t, Dist)
plt.xlabel('Time (s)')
plt.ylabel('Dist')
plt.show()
```

> Dist is a vector that discribes the male-female distance (mfDist) for the entire experiment (30 minutes).
