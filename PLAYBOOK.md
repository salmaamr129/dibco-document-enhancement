# Playbook -- DIBCO Document Enhancement

Step-by-step guide to get the notebook running locally on the DIBCO
dataset, from a clean machine to a completed run with evaluation plots.
If you've never run a Jupyter notebook before, this is the file to
follow.

## 1. Prerequisites

Install the following once per machine. Pinned versions are what the
project was developed against -- newer minor versions also work.

| Tool | Version | Where to get it |
|---|---|---|
| Python    | 3.9+  | https://www.python.org/downloads/    |
| pip       | bundled with Python | comes with the Python installer |
| Git       | any   | https://git-scm.com/downloads        |

No local install is strictly required: the notebook is designed to also
run unmodified on **Kaggle** or **Google Colab** (see sections 8 and 9).

Verify Python is installed:

```bash
python3 --version
# should print "Python 3.9.x" or newer
```

## 2. Clone the repo

```bash
git clone https://github.com/salmaamr129/dibco-document-enhancement.git
cd dibco-document-enhancement
```

## 3. Create a virtual environment and install dependencies

Create an isolated environment so the project's libraries don't collide
with anything else on the system:

```bash
python3 -m venv .venv
```

Activate it. **Linux / macOS:**

```bash
source .venv/bin/activate
```

**Windows (PowerShell or cmd):**

```powershell
.venv\Scripts\activate
```

Once activated (your prompt should show `(.venv)`), install the
dependencies:

```bash
pip install opencv-python numpy matplotlib tqdm scikit-image jupyter
```

> A `requirements.txt` would be a nice future addition so this becomes a
> single `pip install -r requirements.txt`. For now, the explicit list
> above is the source of truth.

## 4. Get the DIBCO dataset

The dataset is not bundled with the repo. Download it from one of:

- The official DIBCO 2019 page: https://vc.ee.duth.gr/dibco2019/
- A Kaggle mirror: https://www.kaggle.com/datasets (search for **"DIBCO"**)

Extract the archive into the repo directory (or anywhere on disk -- you
will point the notebook at it in the next step). A typical layout:

```
dibco-document-enhancement/
  image-final-project.ipynb
  data/
    dibco/
      images/          (degraded inputs)
      ground_truth/    (binary targets)
```

You need both the degraded input images and the matching ground-truth
images -- evaluation in the last cells compares the two.

## 5. Configure the dataset path in the notebook

The notebook was originally run on Kaggle, so the dataset path in one of
the first cells looks something like:

```python
DATASET_PATH = "/kaggle/input/dibco-..."
```

Open the notebook and edit that string to point at your local copy, for
example:

```python
DATASET_PATH = "./data/dibco"
```

If image and ground-truth directories are defined as separate variables,
update each one. Save the notebook.

## 6. Launch Jupyter and run the notebook

With the venv still activated:

```bash
jupyter notebook image-final-project.ipynb
```

(Or `jupyter lab image-final-project.ipynb` if you prefer JupyterLab.)
Jupyter will open in your browser. From the menu choose
**Kernel -> Restart & Run All** (or **Run -> Run All Cells**).

## 7. Verify it works

A successful run shows:

- **tqdm progress bars** complete cleanly as the notebook loops over the
  dataset (preprocessing, then per-image binarization).
- **Side-by-side comparison plots** appear inline: original image,
  binarized output, and ground truth, for several sample documents.
- **Printed metric numbers** -- per-pixel comparison of the binarized
  output against the ground truth (Sauvola should come out on top in
  the experiments).
- No `FileNotFoundError`, `ModuleNotFoundError`, or red traceback
  anywhere in the notebook.

If the plots render but look empty / all-black / all-white, the dataset
path is probably pointing at the wrong subdirectory -- jump to
troubleshooting.

## 8. Alternative: run on Kaggle (no local install)

1. Go to https://www.kaggle.com/code and click **New Notebook**.
2. Use **File -> Import Notebook** and upload `image-final-project.ipynb`.
3. In the right-hand sidebar, click **Add Data** and attach a DIBCO
   dataset (search the Kaggle datasets catalog for "DIBCO").
4. Update `DATASET_PATH` in the notebook to match the Kaggle mount path
   (usually `/kaggle/input/<dataset-slug>`).
5. **Run All**. Kaggle already has `opencv-python`, `numpy`,
   `matplotlib`, `tqdm`, and `scikit-image` pre-installed.

## 9. Alternative: run on Google Colab

1. Go to https://colab.research.google.com and choose
   **File -> Upload notebook**, then pick `image-final-project.ipynb`.
2. Install any missing libraries as the first cell:

   ```python
   !pip install scikit-image tqdm
   ```

   (`opencv-python`, `numpy`, and `matplotlib` are already on Colab.)
3. Get the dataset onto the Colab VM, either by:
   - Uploading the extracted folder to `/content/dibco/` via the file
     panel on the left, or
   - Mounting Google Drive:

     ```python
     from google.colab import drive
     drive.mount('/content/drive')
     ```

     and pointing `DATASET_PATH` at the folder inside your Drive.
4. Update `DATASET_PATH` to match, then **Runtime -> Run all**.

## 10. Troubleshooting

| Symptom | Likely cause | Fix |
|---|---|---|
| `ModuleNotFoundError: No module named 'cv2'` | venv not activated, or `opencv-python` not installed | Re-activate the venv (step 3) and run `pip install opencv-python` |
| `ModuleNotFoundError: No module named 'skimage'` | `scikit-image` was missed during install | `pip install scikit-image` |
| `FileNotFoundError` on the dataset path | The notebook still points at the original Kaggle path | Edit `DATASET_PATH` (step 5) to match your local extracted folder |
| Plots don't render inline in Jupyter | Stale kernel, or `%matplotlib inline` not active | **Kernel -> Restart & Run All**; if it persists, add `%matplotlib inline` near the imports |
| Notebook is extremely slow | Expected on the full dataset, especially the per-image binarization loop | While developing, slice the file list to the first few images (e.g. `files[:5]`) and only run the full set for the final evaluation |
| `jupyter: command not found` | `jupyter` was installed outside the active venv | Confirm the venv is active (`which python` should point inside `.venv`) and run `pip install jupyter` |

## 11. Stop

Save the notebook (**Ctrl+S** / **Cmd+S**), then in the terminal where
Jupyter is running press **Ctrl+C** twice to shut down the server. When
you're done with the project for the day, deactivate the venv with:

```bash
deactivate
```
