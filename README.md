# Artificial Intelligence–based Quantification of Pleural Plaque Volume and Association With Lung Function in Asbestos-exposed Patients :computer: :mag_right: 
Open sourcing segmentation models for pleural plaques in thoracic CT scans following our publication: https://journals.lww.com/thoracicimaging/abstract/9900/artificial_intelligence_based_quantification_of.107.aspx

Managed by Kevin Groot Lipman (k.groot.lipman[at]nki[dot]nl, k.b.w.grootlipman[at]gmail[dot]com)

## 1. Setting up the environment :deciduous_tree:
Install miniconda3: https://docs.conda.io/en/latest/miniconda.html
or 
```
curl -O https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
bash Miniconda3-latest-Linux-x86_64.sh
 ```
Create and activate a conda environment with Python
```
conda create --name torch python=3.11
conda activate torch
 ```
Install latest PyTorch version with conda and cuda support: https://pytorch.org/get-started/locally/

Only then install nnUNet

```
pip install nnunet
 ```

Please read the updated nnUNet repo for instructions: https://github.com/MIC-DKFZ/nnUNet


## 2. Preprocess your DICOM files 
### 2.1 Convert DICOM to Nifti
nnUNet models do not accept DICOM files. Therefore, we must first convert our selected CT scans to the Nifti format.
Install conversion software:
```
conda activate torchv2
git clone https://github.com/JoostJM/nrrdify.git
cd nrrdify
pip install .
 ```
Run the conversion script:

```
nrrdify DICOM_FOLDER --out OUTPUT_FOLDER --format nii.gz
 ```
If DICOM scans are not being converted, it is most likely due to inconsistent slice spacing. This is intended behavior since we don't process CT scans with inconsistent 'gaps'.

## 3. How to run the segmentation model 🚀 

### 3.1 Download the weights

Download ``models/TaskXXX_PP.zip``
Replace XXX with the task number you want to run (200-204)

### 3.2 Set your environment variables
See: https://github.com/MIC-DKFZ/nnUNet/blob/master/documentation/set_environment_variables.md

### 3.3 Running inference
- Unzip TaskXXX_PP.zip in your ``nnUNet_results`` folder defined in 3.2
- Run ``nnUNet_predict -d TaskXXX_PP -i INPUT_FOLDER -o OUTPUT_FOLDER -f all -tr nnUNetTrainer -c 3d_fullres -p nnUNetPlans``

The script will run and the AI model will output binary segmentation masks in 'OUTPUT_FOLDER'.

## 4. Inspection and correction output
## 4.1 Install Slicer 
- Download Slicer **5.3** at https://download.slicer.org/
- Start Slicer, go to 'File' -> 'Add data' -> 'Choose File(s) to Add'. Select the Nifti CT scan (with _0000.nii.gz at the end) and the corresponding segmentation (same name without _0000).
- Before you click 'OK', ensure that the CT scan is loaded as 'Volume' (in column 'Description') and the segmentation is loaded as 'Segmentation'.
- The CT scan and corresponding segmentation is now visible
- Click on 'Welcome to Slicer', then 'Segment Editor'.
- Here are several tools to correct the segmentation; the most common ones are the 'Paint' and the 'Erase' tool on the left.

## 5. Troubleshooting 🔨 
- If nnUNet cannot find the CT scans in your folder, you probably forgot to append the __0000_ before _.nii.gz_
- Other problems, please don't hesitate to reach out
  
## 6. Acknowledgments 
- This project heavily relied on nnUNet; big shoutout to them: https://github.com/MIC-DKFZ/nnUNet
