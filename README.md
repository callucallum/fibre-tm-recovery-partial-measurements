# fibre-tm-recovery-partial-measurements

[![Paper DOI](https://img.shields.io/badge/DOI-10.1117%2F12.3077568-blue)](https://doi.org/10.1117/12.3077568)

Code and trained model weights for

> C. J. Wilson, Y. Zheng, D. B. Phillips and G. S. D. Gordon, **Using physics-informed AI to enable fibre TM recovery from minimal partial measurements enabling ultra-thin endoscopes**, *Proc. SPIE* 13865 (*Computational Optical Imaging and Artificial Intelligence in Biomedical Sciences III*), 138650A (2026). [doi:10.1117/12.3077568](https://doi.org/10.1117/12.3077568)

**Status:** research code released to accompany the paper above. It is archived and not actively maintained, and it is less polished than my more recent repositories (see [Related work](#related-work)). Some data and the simulation scripts are not included (see [Data](#data)).

## What it does

A multimode fibre's transmission matrix (TM) changes when the fibre bends. A self-attention model takes only a partial set of the TM's mode measurements (33% or 17% of the 78 modes) and predicts the fibre's bend radius and direction. The predicted bend is then passed to a physical forward model of the fibre to rebuild the full TM. The paper reports simulation results only.

## Contents

| folder | contents |
|---|---|
| `notebooks/` | the two training and evaluation notebooks below |
| `models/` | trained weights (`.weights.h5`), not the full models; see [Models](#models) |
| `data/` | training and test data for the 26x26 and 13x13 partial-TM models |
| `images/` | results plot saved by the 83% notebook |

| notebook | what it does |
|---|---|
| `Reduce Input TM (by 83%) and Predict Bend Radius.ipynb` | main notebook: predicts the bend radius from 13 of 78 modes (13x13 input), with Bayesian hyperparameter search, evaluation, and the TM error check |
| `Reduce Input TM (by 67%) and Predict Bend Radius.ipynb` | the same with 26 of 78 modes (26x26 input) |

## Data

- `data/13x13 TM Data/` and `data/26x26 TM Data/`: simulated partial TMs (`trainData_in.mat`, `testData_in.mat`) with the matching bend radii (`bend_radius_train.mat`, `bend_radius_test.mat`), plus single-bend example TMs (`±0.0840.mat`, `±0.3261.mat`, `±0.7015.mat`). The 13x13 folder also has the variants that choose which 13 modes are kept (first, last, middle, every sixth, ...).
- **Not included:** the full 78x78 TMs used to compute the TM errors in the paper, and the MATLAB simulation scripts that generated the data. The notebook cells that read `../data/78x78 TM Data/...` therefore won't run. The training, prediction and bend-radius evaluation cells use only the files in this repository.

The data are simulated, not measured.

## Models

`models/67%Model.weights.h5` and `models/83%Model.weights.h5` hold the trained weights only. Define the model as in the notebook, then load them:

```python
model.load_weights("../models/83%Model.weights.h5")
```

Retraining takes more than 10 minutes on a laptop.

## Running it

```bash
pip install -r requirements.txt
jupyter lab
```

Open a notebook in `notebooks/` and run it from that folder, since the data paths are relative (`../data/...`). The code was written with TensorFlow/Keras, and `requirements.txt` is unpinned, so if a newer version breaks something, try an older TensorFlow and Keras.

## Related work

Follow-up work on pixel-basis multimode-fibre TMs, with cleaner code and data:

- Code, weights and splits: [10.5281/zenodo.24343908](https://doi.org/10.5281/zenodo.24343908) ([GitHub](https://github.com/callucallum/mmf-tm-pca-autoencoder))
- Experimental TMs and RMs: [10.57967/hf/11679](https://doi.org/10.57967/hf/11679)
- Simulated TMs: [10.57967/hf/11677](https://doi.org/10.57967/hf/11677)

## Licence

Code under the [MIT licence](LICENSE); trained weights in `models/` under [CC BY 4.0](models/LICENSE).

## Citation

Please cite the paper:

```bibtex
@inproceedings{wilson2026physicsinformed,
  author    = {Wilson, Callum J. and Zheng, Yijie and Phillips, David B. and Gordon, George S. D.},
  title     = {Using physics-informed {AI} to enable fibre {TM} recovery from minimal partial measurements enabling ultra-thin endoscopes},
  booktitle = {Computational Optical Imaging and Artificial Intelligence in Biomedical Sciences III},
  series    = {Proc. SPIE},
  volume    = {13865},
  pages     = {138650A},
  year      = {2026},
  doi       = {10.1117/12.3077568}
}
```
