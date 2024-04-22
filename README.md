# MaterialSeg3D: Segmenting Dense Materials from 2D Priors for 3D Assets
Project Homepage

<!-- <br> -->
[Zeyu Li<sup>1#</sup>](https://lizeyuking.github.io/), Ruitong Gan<sup>2#</sup>,  Chuanchen Luo<sup>3*</sup>, Yuxi Wang<sup>4</sup>,  Jiaheng Liu<sup>5</sup>, Ziwei Zhu<sup>6</sup>, Man Zhang<sup>1</sup>, Qing Li<sup>2</sup>,Xucheng Yin<sup>6</sup>, Zhaoxiang Zhang<sup>3</sup>, and Junran Peng<sup>3</sup>.
<!-- <br> -->

(#Equal contribution)

[<sup>1</sup>Beijing University of Posts and Telecommunications](https://www.sjtu.edu.cn/), 
[<sup>2</sup>Hong Kong Polytechnic University](https://open.youtu.qq.com/#/open)
[<sup>3</sup> Institute of Automation, Chinese Academy of Sciences](https://open.youtu.qq.com/#/open)
[<sup>4</sup>CARI, HKISI-CAS](https://open.youtu.qq.com/#/open)
[<sup>5</sup>Beijing University of Aeronautics and Astronautics](https://open.youtu.qq.com/#/open)
[<sup>6</sup>Beijing University of Aeronautics and Astronautics](https://open.youtu.qq.com/#/open)

[![arXiv](https://img.shields.io/badge/arXiv-2312.05767-b31b1b.svg)](https://arxiv.org/abs/2312.05767)

[Project Page](https://sjtuplayer.github.io/anomalydiffusion-page/)



## Dataset
Our data is coming soon.


## Overview
Anomalydiffusion is a few-shot anomaly generation model for anomaly inspection (detection, localization and classification). 
The overall process can be divided into the following 5 steps:

(1) Train the anomaly generation model and mask generation model;

(2) Generate anomalous masks by the mask generation model in step (1);

(3) Generate anomaly mask-image pairs by the anomaly generation model based on the generated masks in step (2);

(4) Train the anomaly detection (for both detection and localization) and classification model based on the anomalous image-mask pairs in step (3).



## Prepare


### (1) Prepare the environment
```
Ubuntu
python 3.8
cuda==11.8
gcc==7.5.0
conda env create -f environment.yaml
conda activate Anomalydiffusion
```


### (2) Checkpoint for LDM

Download the official checkpoint of the latent diffusion model:
```
mkdir -p models/ldm/text2img-large/
wget -O models/ldm/text2img-large/model.ckpt https://ommer-lab.com/files/latent-diffusion/nitro/txt2img-f8-large/model.ckpt
```

## Generating anomalous image-mask pairs

In this section, you can first train the anomaly generation model by (1). After that, you can run (2), which
contains training mask generation models, generating anomalous masks and generating anomalous image-mask pairs.

### (1) Train the anomaly generation model by:

```
CUDA_VISIBLE_DEVICES=$gpu_id python main.py --spatial_encoder_embedding --data_enhance
 --base configs/latent-diffusion/txt2img-1p4B-finetune-encoder+embedding.yaml -t 
 --actual_resume models/ldm/text2img-large/model.ckpt -n test --gpus 0, 
  --init_word anomaly  --mvtec_path=$path_to_mvtec_dataset
```

### (2) Train the mask generation model and generate image-mask pairs by:
```
CUDA_VISIBLE_DEVICES=$gpu_id python run-mvtec.py --data_path=$path_to_mvtec_dataset
```
## Test the anomaly inspection performance

[//]: # (### &#40;1&#41; Generating anomlay image-mask pairs)

[//]: # (After training &#40;or downloading&#41; the anomalous generation model and mask generation model,)

[//]: # (you can generate anomaly masks first, and then generate anomalous image-mask pairs.)

[//]: # ()
[//]: # (To generate **anomaly masks**, you can run:)

[//]: # ()
[//]: # ()
[//]: # (After generating anomalous masks, you can generate **anomalous image-mask paris** by:)

After generating anomalous image-mask pairs,
you can train and test the **anomaly detection model** (for both anomlay detection and localization) by:
```
python train-localization.py --generated_data_path $path_to_the_generated_data  --mvtec_path=$path_to_mvtec
python test-localization.py --generated_data_path $path_to_mvtec
```

you can train and test the **anomaly classification model** by:
```
python train-classification.py --mvtec_path=$path_to_mvtec --generated_data_path=$path_to_the_generated_data
python test-classification.py --mvtec_path=$path_to_mvtec --generated_data_path=$path_to_the_generated_data
```
## Citation

If you make use of our work, please cite our paper:

```
@inproceedings{hu2023anomalydiffusion,
  title={AnomalyDiffusion: Few-Shot Anomaly Image Generation with Diffusion Model},
  author={Hu, Teng and Zhang, Jiangning and Yi, Ran and Du, Yuzhen and Chen, Xu and Liu, Liang and Wang, Yabiao and Wang, Chengjie},
  booktitle={Proceedings of the AAAI Conference on Artificial Intelligence},
  year={2024}
}
```
