# DIBCO Document Enhancement

Document image enhancement on the **DIBCO** (Document Image Binarization
Contest) dataset. The notebook walks through a complete restoration
pipeline — preprocessing, filter comparison, binarization, and
morphological cleanup — and evaluates the final binarized output against
the ground-truth images that ship with the dataset.

## What it does

| Stage | Techniques | Purpose |
|---|---|---|
| **Preprocessing**     | Grayscale conversion, resize to 256×256, denoising, background normalization | Bring images to a uniform shape, knock down sensor noise |
| **Filter comparison** | Gaussian blur, median filter, sharpening                                     | See how different smoothing strategies affect the result |
| **Binarization**      | Otsu, Adaptive Gaussian, Sauvola                                             | Convert grayscale to black-and-white text/background |
| **Morphology**        | Opening, closing                                                             | Remove specks, fill in broken strokes |
| **Evaluation**        | Per-pixel comparison vs. Ground Truth                                        | Quantify how close the output is to the target |

Sauvola was the best-performing binarization on the DIBCO documents in
the experiments.

## Tech stack

- Python 3
- [OpenCV](https://opencv.org/) (`cv2`) — core image processing
- [NumPy](https://numpy.org/) — array manipulation
- [Matplotlib](https://matplotlib.org/) — visualisation
- [tqdm](https://tqdm.github.io/) — progress bars over the dataset
- Designed to run on Kaggle / Colab; also works in any local Jupyter environment

## Running locally

> For a guided, copy-pasteable walkthrough — venv setup, dataset
> download, path configuration, Kaggle/Colab alternatives, and
> troubleshooting — see **[PLAYBOOK.md](PLAYBOOK.md)**.

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install opencv-python numpy matplotlib tqdm scikit-image jupyter
jupyter notebook image-final-project.ipynb
```

> The notebook expects the DIBCO dataset to be available at the path
> referenced in the first few cells (originally a Kaggle dataset
> directory). Adjust the dataset path at the top of the notebook to
> point at your local copy.

## Dataset

[DIBCO](https://vc.ee.duth.gr/dibco2019/) is a benchmark dataset for
document image binarization, containing pairs of:

- Degraded historical / handwritten / printed document images
- Corresponding binary ground-truth images

The dataset is not redistributed here — download it from the
[DIBCO page](https://vc.ee.duth.gr/dibco2019/) or via a
[Kaggle mirror](https://www.kaggle.com/) before running.

## Files

```
image-final-project.ipynb     The pipeline notebook (outputs cleared)
README.md                     This file
.gitignore                    Python / Jupyter ignores
```

## License

[MIT](LICENSE)
