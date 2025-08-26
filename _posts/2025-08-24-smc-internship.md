---
title: "Arrest vs ROSC classification"
date: 2025-08-25 23:40:00 +0900
categories: ["My projects", "SMC Summer Internship"]
tags: ["internship", "smc"]
---
## MARS Lab. Summer Project ##

### TASK : CPR UltraSound video(image) through CAC for Arrest vs ROSC Classification end-to-end sysyem

  - 진행 기간: 2025.08.04 ~ 2025.08.26
  - 진행 팀원 : 2명
  - 역할 분배:
      - 박민선: Data pre-processing & YOLOv12n fine tuning & ML(LightGBM, 1D-CNN) & Experiments
      - 임우진: Data pre-processing & SAM2 fine tuning & ML(LightGBM, 1D-CNN, GRU) & Experiments
  - 해당 프로젝트는 성균관대학원 MARS Lab에서 주관하였으며, 각종 학습에 필요한 dataset은 강남삼성병원에서 제공 받아 비식별화 및 마스킹 이후 사용하였습니다.

### Introduction

  - Model selection: YOLOv12n, SAM2, GRU
  - Project pipeline: Input Video(30fps) -> YOLOv12n for artery, vein detection -> SAM2 for artery, vein segmentation -> GRU(Arrest, ROSC classification)
  - Plan:
      1. Data pre-processing: Masking error, image size, labeling, dataset construction
         - image size: 640 x 640
           
         - labeling:
             - 1 - artery, 2 - vein (on Detection, segmentation model)
             - 0 - Arrest, 1 - ROSC (on Classification)
    
         - dataset construction:
             - YOLO dataset - pixel BGR 값을 통한 x,y 좌표 추출 및 .txt 생성, .ymal file 생성
             - SAM2 dataset - raw image: jpg, mask image: png
             - Classification - patient-wise video 생성 및 pixel BGR 값을 통한 artery, vein area 추출 csv 파일 생성
               
      2. YOLOv12 fine tuning & Model research
           - Segmentation model: Mobile SAM, Mobile SAMv2, efficientSAM, FastSAM
             
      3. SAM2 fine tuning & lightGBM, 1D-CNN, GRU fine tuning
           - 1D-CNN: 심전도 신호 분류를 위한 1D CNN 모델 구성 요소의 최적화(KCI) model architecture 참조 하여 tensorflow로 작성
           - GRU: GRU-TV: Time- and velocity-aware GRU for patient representation on multivariate clinical time-series data 참조하여 tensorflowf로 작성
             
      4. Experiments
           - YOLO: acc, precision, recall, mAP, F1 score
           - SAM: IoU, hausdroff distance...
           - classification: acc, precision, recall, F1 score
         
      6. Announcing
         
  - Datasets: SMC 응급의학과 CAC ultrasound video & image 제공
      - Arrest: ICV가 collapse 된 상태이고 심장 압박 중에도 대동맥이 펄스 없이 늘어지거나 움직임이 없음
      - ROSC: ICV가 collapse 되어 있지만 심장 압박 시 대동맥이 둥글고 팽팽한 형태를 유지함

   
### Information concerning files
  - Classification: Arrest/ROSC binary classification, 앙상블 모델(XGBM, LGBM, SVM, RF) & 1D-CNN & GRU fine tuning script
  - Experiments: 활용한 모델들의 대한 성능평가 진행
  - Main: Inference to patient_wise video
  - Make_are: Classification 학습을 위해서 동맥과 정맥의 면적을 csv 형태로 추출
  - Yolo: Artery, Vein for Detection
  - sam2.1: Yolo's bbox를 넘겨 받아 Artery, Vein segmentation 수행
  - patient_wise_generate_video.py -> patient_wise하게 동일한 환자 id 기준으로 video 생성

### Reference

 - Point-of-care ultrasound compression of the carotid artery for pulse determination in cardiopulmonary resuscitation
 - Artificial intelligence-based evaluation of carotid artery compressibility via point-of-care ultrasound in determining the return of spontaneous circulation during cardiopulmonary resuscitation
- Automatic diagnosis of common carotid artery disease using different machine learning techniques 
- FASTER SEGMENT ANYTHING: TOWARDS LIGHTWEIGHT SAM FOR MOBILE APPLICATIONS
- Mobile SAMv2: Faster Segment Anything to Everything
- SAM 2: Segment Anything in Images and Videos
- 심전도 신호 분류를 위한 1D CNN 모델 구성 요소의 최적화(KCI)
- GRU-TV: Time- and velocity-aware GRU for patient representation on multivariate clinical time-series data



