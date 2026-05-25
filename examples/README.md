# Pipeline examples

Snapshots of each stage of the notebook's pipeline, taken from one
representative document in the DIBCO dataset. The notebook itself is
shipped with cleared outputs (to keep `image-final-project.ipynb`
small); these PNGs preserve the visual results so the work is
inspectable without running the code.

| File | Stage | What it shows |
|---|---|---|
| `01-dataset-samples.png`         | Inspection      | Three random pairs of (degraded document, ground-truth binary mask) drawn from the dataset, side-by-side. |
| `02-selected-image.png`          | Inspection      | The single image auto-picked for the demo (the one with the highest contrast / standard deviation in the dataset). |
| `03-sauvola-binarization.png`    | Binarization    | The Sauvola local-threshold output for the selected image — the binarization that the notebook concludes works best for DIBCO documents. |
| `04-morphological-cleanup.png`   | Morphology      | The Sauvola result before vs. after `MORPH_OPEN` (removes specks) and `MORPH_CLOSE` (fills small gaps). |
| `05-vs-ground-truth.png`         | Evaluation      | Final cleaned binarization placed next to the ground-truth mask for visual comparison. |

To regenerate them: open `../image-final-project.ipynb`, run all cells,
right-click each rendered figure → *Save image as…*. (See the
[Playbook](../PLAYBOOK.md) for the full run.)
