# PRETRAIN\_41K: A Text-Based Person Retrieval Dataset

## Dataset Structure

The **PRETRAIN** dataset is a large-scale database for Text-to-Image Person Re-identification task which consists of 2 versions:

| Dataset | Release Time | No. image | No. description | No. ids | Avg word/text |
|----------|--------------|-----------|----------------|---------|---------------|
| [PRETRAIN](#PRETRAIN) (41K) | 2025 | 41,017 | 82,034 | 4,080 | 68.12 |
| [PRETRAIN](#PRETRAIN) (110K) | 2025 | 109,937 | 219,874 | 4,391 | 68.99 |

The dataset is stored in **PRETRAIN** directory with the following structure:

### Folder Structure

```
PRETRAIN/
|-- imgs/               # Folder containing all images
    |-- ENTIReID        # Containing images of ENTIReID dataset
    |-- IUSTPersonReID  # Containing images of IUSTPersonReID dataset
|-- PRETRAIN_110K.json  # JSON file containing captions, id, (image quality) score, split, ... for 110K version
|-- PRETRAIN_41K.json   # JSON file containing captions, id, (image quality) score, split, ... for 41K version
|-- README.md
```

## Data Sources

The images from this dataset are collected from two publicly available person re-identification datasets:

1. [**IUST\_PersonReID**](https://github.com/ComputerVisionIUST/IUST_PersonReId) (31K samples for **41K** version and 100K samples for **110K** samples)
2. [**ENTIRe-ID**](https://github.com/serdaryildiz/ENTIRe-ID) (\~10K samples, including query and bounding\_box\_test. The train set of this dataset has not been released yet.)

### JSON File Structure

The **PRETRAIN\_41K.json** and **PRETRAIN\_110K.json** file contains metadata for the **PRETRAIN\_41K** dataset and the **PRETRAIN\_110K** dataset. Each entry in the JSON file has the following fields:

- **file\_path** (string): Path to the image file inside the `imgs/` folder.
    - There are 2 kinds of image name structure:
        - `<personID>_<cameraID>_<sequenceID>_<frameID>.<extension>`: These files belong to **IUST\_PersonReID** dataset.
        - `<personID>_<UNKNOWN1>_<UNKNOWN2>.<extension>`: These files belong to **ENTIRe-ID** dataset.
- **captions** (list of 2 strings): Two different textual descriptions for the image.
- **split** (string): Indicates whether the image belongs to the training set (`train`) or validation set (`val`).
- **score** (float): Overall image quality score, computed using the [DeQA-Score](https://github.com/zhiyuanyou/DeQA-Score) (CVPR 2025) method.
- **ID** (string): The original identity of the person in the image, extracted from the filename (by taking the first element after splitting value of `file_path` by the character `_`).
- **id** (int): An integer version of the identity, differing from `ID` but still uniquely representing the person.
- **source** (string): The name of the source ReID dataset which the image comes from (either [**IUST\_PersonReID**](https://github.com/ComputerVisionIUST/IUST_PersonReId) or [**ENTIRe-ID**](https://github.com/serdaryildiz/ENTIRe-ID)).

## Caption Generation

Each image in the dataset is annotated with **two captions** generated using public vision-language models on huggingface:

1. [**Llama3.2-Vision-11B**](https://huggingface.co/meta-llama/Llama-3.2-11B-Vision)
2. [**Qwen2.5-VL-7B-Instruct**](https://huggingface.co/neuralmagic/Qwen2.5-VL-7B-Instruct-FP8-Dynamic)
3. [**InternVL2\_5-8B-MPO**](https://huggingface.co/OpenGVLab/InternVL2_5-8B-MPO)
4. [**Aya-vision-8b**](https://huggingface.co/CohereForAI/aya-vision-8b)
5. [**Ovis2-8B**](https://huggingface.co/AIDC-AI/Ovis2-8B)
6. [**Ovis2-34B-GPTQ-Int4**](https://huggingface.co/AIDC-AI/Ovis2-34B-GPTQ-Int4)

These captions provide diverse textual descriptions for each image, aiding in text-based person retrieval tasks.

## Usage

The **PRETRAIN\_41K** and **PRETRAIN\_110K** dataset is suitable for pretraining image-text architecture on **text-based person retrieval** task, where given a textual description, the task is to retrieve the most relevant image(s) from the dataset.