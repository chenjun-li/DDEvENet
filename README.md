# DDEvENet
"DDEvENet: Evidence-based Ensemble Learning for Uncertainty-aware Brain Parcellation using Diffusion MRI"
## Overview
Evidential Ensemble Neural Network based on Deep learning and Diffusion MRI (DDEvENet) is a novel uncertainty-aware deep learning method for anatomical brain parcellation of cortical and subcortical regions directly from dMRI data. The key innovation of DDEvENet is its utilization of evidential deep learning to quantify uncertainty at each voxel during a single inference. The development of the model is based on [FastSurfer](https://github.com/Deep-MI/FastSurfer/tree/dev).
## Usage
### Installation
DDEvENet is built and tested in an environment the same as that specified by FastSurfer (See [here](https://github.com/Deep-MI/FastSurfer/blob/dev/doc/overview/INSTALL.md#native-ubuntu-2004-or-ubuntu-2204) for more details). For a native install on Ubuntu 22.04, simply clone the project and create a new conda environment by running `conda env create -f DDEvENet_cpu.yml` or `conda env create -f DDEvENet_gpu.yml`.

### Running DDEvENet Parcellation

The input of the pretrained DDEvENet models must be **320x320x320** dMRI image with a voxel size of **1.25x1.25x1.25 \( mm^3 \)**.

The `script_to_run.txt` file provides the example command lines to run DDEvENet. For example, to obtain the anatomical brain parcellation and subnetwork uncertainty estimation on a `FA` image of `Subject X`, run

```
python3 /DDEvENetCNN/run_prediction.py \
--sd /PATH/TO/SUBJECT_DATA/FA \
--sid SUB_X \
--t1 /PATH/TO/SUBJECT_DATA/SUB_X/fa.nii.gz \
--lut /DDEvENetCNN/config/DDEvENet_ColorLUT.tsv \
--aparc_aseg_segfile pred.nii.gz \
--cfg_ax /DDEvENetCNN/config/EvidentialSurferFA_1k_axial.yaml \
--ckpt_ax /Trained_models/FA/Axial_Best_training_state.pkl \
--cfg_cor /DDEvENetCNN/config/EvidentialSurferFA_1k_coronal.yaml \
--ckpt_cor /Trained_models/FA/Coronal_Best_training_state.pkl \
--cfg_sag /DDEvENetCNN/config/EvidentialSurferFA_1k_sagittal.yaml \
--ckpt_sag /Trained_models/FA/Sagittal_Best_training_state.pkl \
--batch_size 1 \
--viewagg_device cpu
```
### Perform Ensemble

To ensemble the results from `FA, MD, E3` and obtain final uncertainty estimation of `Subject X`, run the `/DDEvENetCNN/utils/deep_ensemble.py` after specifying corresponding input paths.
