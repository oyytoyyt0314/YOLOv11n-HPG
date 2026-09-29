# YOLOv11n-HPG for Pavement Crack Detection

This repository contains the research implementation of YOLOv11n-HPG, a lightweight pavement-crack detector built on Ultralytics YOLO. The model is designed to preserve fine crack responses during downsampling, enhance directional and multi-scale crack features, and improve feature propagation across detection scales.

## Method overview

The proposed detector combines three components:

- **HIDown**: a dual-path downsampling block for retaining complementary fine-scale responses.
- **PSA-SMLCA**: partial spatial-channel feature enhancement using axial multi-scale processing and local-global channel recalibration.
- **GSNeckV2**: a lightweight neck organization using GSConv and OSAGSBlock for cross-scale feature aggregation.

The code reorganizes established operations for pavement-crack detection; individual underlying operations are not claimed as newly introduced primitives.

## Repository contents

```text
ultralytics/
├── nn/
   └── Addmodules/                 # Custom modules required by the released models
 YOLOv11n-HPG.yaml       # Proposed detector definition
训练代码.py                          # Detector training entry point
```

Only modules and model configurations used by the paper should be included in the public release. If custom modules require registration or parsing changes, retain the corresponding modified files under `ultralytics/nn/` as well.

## Environment

Use the Python and PyTorch versions recorded in `requirements.txt` (to be added with the tested environment). From the repository root, install the package in editable mode:

```bash
pip install -e .
```

Before release, record the tested Python, PyTorch, CUDA, and package versions and include them in `requirements.txt` or an environment file.

## Datasets

Datasets are not bundled with this repository. Download RDD2022 and CRACK500 from their authorized sources and prepare them locally.

For the RDD2022 China_MotorBike subset, D00, D10, and D20 are mapped to a single `crack` class. D40 and Repair are excluded from the target labels; images containing only excluded categories are retained as negative samples. Use the fixed split manifest associated with the experiments when reproducing the reported results.

For cross-platform evaluation on China_Drone, apply the same target definition: D00, D10, and D20 are mapped to `crack`; D40, Repair, and Block-crack annotations are excluded from target labels. Images containing only excluded categories remain negative samples. China_Drone is used for evaluation only.

For the downstream detector-guided segmentation and geometry experiments, prepare CRACK500 using the original image-level split before extracting patches. The exact data paths and manifest locations should be set in the relevant configuration files; do not commit dataset images or private local paths.

## Training

1. Set the dataset and output paths in `训练代码.py` (or its configuration file) to your local paths.
2. Confirm that the model YAML path points to `ultralytics/cfg/models/11/YOLOv11n-HPG.yaml`.
3. Run the training entry point from the repository root:

```bash
python "训练代码.py"
```

The reported detector protocol uses a 640 × 640 input, SGD, an initial learning rate of 0.01, momentum of 0.937, weight decay of 0.0005, cosine learning-rate decay, batch size 8, up to 200 epochs, and early stopping with patience 30. The group-constrained dataset partition is fixed across runs; training seeds are 0, 42, and 2026.

Check the released training script before use to confirm that its arguments and defaults match the protocol above.

## Evaluation and downstream analysis

The paper reports detector evaluation on RDD2022, cross-platform evaluation on China_Drone, and detector-guided segmentation and geometric measurement experiments on CRACK500. Add the corresponding evaluation commands here when the evaluation scripts are included in the release. Do not use China_Drone for training, model selection, or threshold adjustment.

## Checkpoints and results

Training outputs, checkpoints, logs, and generated result folders are not included by default. If trained weights are released separately, provide their download location, checksum, and the exact configuration used to produce them.

## Reproducibility notes

- Preserve the dataset split manifest and class-mapping rules used for the reported results.
- Record the exact dependency versions, random seeds, and command-line arguments.
- Do not change the test set, confidence threshold, or post-processing settings when reproducing the reported evaluation.
- Dataset images and annotations remain subject to their original licenses and terms of use.

## License and acknowledgments

This project is based on Ultralytics YOLO. Retain the upstream copyright and license notices and include the applicable license file with the release. Review the licensing terms for the exact upstream version and all third-party components before redistribution.


