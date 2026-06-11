# 🚗 Vehicle Damage Detection using YOLOv8

> **YOLOv8 기반 차량 파손(Damage) 모델 학습 및 객체 탐지(Object Detection) 연구 프로젝트**
> 구글 코랩(Google Colab) 환경을 활용하여 차량의 외관 스크래치, 찌그러짐, 부품 파손 등 다양한 손상 유형을 탐지하는 딥러닝 파이썬 파이프라인을 구축했습니다.

---

## 📅 프로젝트 개요
* **수행 기간**: 2026.01 ~ 2026.02
* **개발 환경**: Google Colab (Tesla T4 GPU)
* **주요 기술**: Python, Ultralytics YOLOv8, PyTorch

---

## 📊 데이터셋 소개 (Dataset)
차량 외관에서 발생할 수 있는 주요 손상 및 파손 유형을 분류하기 위해 총 **8개 클래스**로 구성된 커스텀 데이터셋을 활용했습니다.

* **Target Classes (8개)**: 
  `Broken part`, `Corrosion`, `Cracked`, `Dent`, `Flaking`, `Missing part`, `Paint chip`, `Scratch`

### 🖼️ 검증 데이터 탐지 샘플 (Validation Batch Sample)
모델이 학습 과정에서 실제 차량의 파손 부위를 바운딩 박스(Bounding Box)로 예측하고 래벨링한 결과물입니다.
<p align="center">
  <img width="1920" height="732" alt="val_batch0_labels" src="https://github.com/user-attachments/assets/470ac1ec-3223-4d3b-b11e-97cebce07111" />

---

## ⚙️ 학습 조건 및 환경 (Training Hyperparameters)
모델의 수렴과 최적의 하이퍼파라미터를 탐색하기 위해 대규모 에포크(Epoch) 실험을 설계했습니다.

* **사용한 모델**: YOLOv8 (Ultralytics)
* **학습 에포크 (Epochs)**: 250 Epochs
* **최적화 도구 (Optimizer)**: SGD / Adam (기본 설정 자동 최적화)

---

## 📈 결과 및 성능 지표 (Results & Metrics)

### 1. 학습 곡선 (Training Curves)
250 Epoch 진행 동안 `box_loss`, `cls_loss`, `dfl_loss`가 안정적으로 우하향하며 수렴하는 것을 확인했습니다. 최종적으로 **mAP50 지표는 약 0.33**에 도달하였습니다.
<p align="center">
  <img width="2400" height="1200" alt="results" src="https://github.com/user-attachments/assets/8c50c246-91c2-4e0a-90a8-0554ae604feb" />

</p>

### 2. 혼동 행렬 (Confusion Matrix)
학습 완료 후 클래스별 분류 성능을 정밀 분석한 결과입니다. 
* `Broken part`(424개), `Dent`(328개), `Scratch`(349개) 등 데이터 수가 풍부한 주요 파손 클래스에서 준수한 탐지 성공 성능을 보였습니다.
* 반면, 미세한 파손이나 데이터가 부족한 일부 클래스의 경우 background(배경)와의 오탐지 경향이 발견되어, 추후 데이터 증강(Data Augmentation) 전략이 필요함을 인사이트로 도출했습니다.
<p align="center">
  <img width="3000" height="2250" alt="confusion_matrix" src="https://github.com/user-attachments/assets/64ad287a-ae93-4668-9e02-11357506e93d" />

</p>

---

## 💡 인사이트 및 회고
1. **시행착오와 끈기**: 250 에포크라는 장기 학습 세션을 안정적으로 제어하며 오버피팅(Overfitting) 없이 모델이 점진적으로 성장하는 과정을 loss 그래프를 통해 검증했습니다.
2. **데이터 불균형 인지**: 오탐지 매트릭스 분석을 통해 AI 모델의 성능은 단순히 알고리즘뿐만 아니라 클래스별 데이터 균형이 미치는 영향이 크다는 점을 정량적으로 체감했습니다.
3. **향후 발전 방향**: 탐지 성능(mAP)을 더욱 끌어올리기 위해 이미지 증강 기법을 도입하거나, YOLOv11 등 최신 경량화 모델로 아키텍처를 확장하여 성능을 비교 분석해볼 계획입니다.
