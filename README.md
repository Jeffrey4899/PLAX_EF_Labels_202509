# PLAX EF Labels Dataset

This repository contains two datasets of EF (ejection fraction) labels for PLAX (parasternal long-axis) echocardiographic videos, derived from the [MIMIC-IV-ECHO](https://www.physionet.org/content/mimic-iv-echo/0.1/) and [MIMIC-IV-Note](https://physionet.org/content/mimic-iv-note/2.2/) dataset. These labels were generated through two distinct approaches and are intended for research use only. Each dataset includes labels formatted to align with the MIMIC-IV-ECHO dataset's structure, enabling seamless integration with corresponding echocardiographic files for those with appropriate access to MIMIC-IV.

---

## 🚀 Live Demos

- **[Hugging Face Demo](https://huggingface.co/spaces/...)** – Try EF estimation from PLAX and A4C clips.  
- **[Google Colab Notebook](https://colab.research.google.com/...)** – Run inference on your own videos step by step.  

⚠️ **Note**: For simplicity, the demo and Colab use **one PLAX model** and **one A4C model**. In our research setting, EF prediction was aggregated from **four PLAX models + one A4C model**.

---

## 📂 Description of Datasets

### 1. Ground Truth Dataset
This dataset contains EF labels derived from clinical notes in the MIMIC-IV-NOTE dataset. Using time-based correlation and GPT-4 NLP, EF values were extracted from discharge summaries and paired with corresponding PLAX videos. After rigorous filtering and validation:
- **Size**: 1,708 videos across 295 studies.
- **Methodology**:
  - Correlated echocardiography studies and clinical notes within a 1-day window.
  - Extracted EF values from free-text notes using GPT-4.
  - Validated EF values using a trained A4C model, achieving a mean absolute error (MAE) of 6.64%.
- **Purpose**: Serves as an independent test set for evaluating PLAX EF prediction models.

### 2. Generated Dataset
This dataset contains EF labels generated through the following pipeline:
- **View Classification**: Applied a fine-tuned video view classifier to identify PLAX and A4C views in MIMIC-IV-ECHO.
- **EF Value Generation**:
  - Used a pre-trained A4C model to predict EF values for A4C videos.
  - Averaged EF predictions across A4C videos in each study to assign EF labels to corresponding PLAX videos.
- **Size**: 25,532 videos across 4,822 studies.
- **Purpose**: Enables training of machine learning models for PLAX EF prediction.

---

## 📑 File Format
Both datasets are provided as CSV files with the following columns:
- **subject_group**: Corresponds to the `pXX` group in MIMIC-IV-ECHO.
- **subject_id**: Corresponds to the `pXXXXXXX` ID in MIMIC-IV-ECHO.
- **study_id**: Corresponds to the `sXXXXXXX` ID in MIMIC-IV-ECHO.
- **file_id**: Corresponds to the `XXXX_XXXX` file identifier in MIMIC-IV-ECHO.
- **EF_value**: Ejection fraction value (percentage).

Example row:
```
p10,p10872900,s96990073,96990073_0057,66.43
```


Example PLAX Echocardiographic View:

<img src="Examples/PLAX_example.jpg" alt="Illustrative PLAX View" width="500"/>

*Note: This image is not sourced from the MIMIC dataset. It is an illustrative example obtained from an online source.*

---

## 🔧 Usage Instructions
To use these labels:
1. Ensure you have access to the MIMIC-IV-ECHO dataset.
2. Use the `subject_group`, `subject_id`, `study_id`, and `file_id` columns to locate the corresponding DICOM echo files in MIMIC-IV-ECHO.
3. The `EF_value` column provides the EF percentage for the corresponding video.

---

## 📦 Models and Code
- Example inference code is provided in the [Colab notebook](https://colab.research.google.com/...).  
- Pretrained models are available on [Hugging Face Hub](https://huggingface.co/...).  
- If you want to replicate the full aggregation setup (4×PLAX + 1×A4C), we recommend downloading all model checkpoints from Hugging Face.  

We do **not** host large model files directly in this repository.  
Instead, use:
```python
from huggingface_hub import hf_hub_download

model_path = hf_hub_download(repo_id="your-hf-repo", filename="model.pt")
```

## License
This dataset is derived from the MIMIC-IV-ECHO and MIMIC-IV-NOTE datasets. Use of this dataset must comply with the MIMIC-IV Data Use Agreement. The labels are shared under the Creative Commons Attribution-NonCommercial 4.0 International (CC BY-NC 4.0) license. For full license details, see [CC BY-NC 4.0](https://creativecommons.org/licenses/by-nc/4.0/).

## Citation
If you use this dataset, please credit the repository as:
```
@misc{plax_labels_github,
  author = {Zhiyuan Gao, Dominic Yurk, Yaser S. Abu-Mostafa},
  title = {PLAX EF Labels Dataset},
  year = {2025},
  note = {Available at: https://github.com/Jeffrey4899/PLAX_EF_Labels_202509}
}
```

## References
This dataset is derived from the following publicly available datasets:
- MIMIC-IV-ECHO: Johnson AEW, Pollard TJ, Shen L, et al. (2020). MIMIC-IV Clinical Database. Version 2.0. Available at: https://www.physionet.org/content/mimic-iv-echo/0.1/
- MIMIC-IV-NOTE: Johnson AEW, Pollard TJ, Shen L, et al. (2020). MIMIC-IV Clinical Database. Version 2.0. Available at: https://physionet.org/content/mimic-iv-note/2.2/

Access to the original datasets requires completion of the PhysioNet Data Use Agreement.

