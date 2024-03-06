# ROADs
Road Obstacle AI Detection service

- 도로시설물 파손에 따른 민원증가 및 유지보수가 미흡하다는 문제점을 발견하였다. 
- 따라서 운전자들이 사용하는 네비게이션 기반으로, 그들의 주행경로에서 파손 시설물을 자동으로 탐지 후 프로그램 상에서 정보를 관리하는 방법을 사용하여, 빠르게 쾌적한 도로 환경을 구현하고 사고를 방지하고자 한다.


## Dataset
AI HUB: 지자체 도로부속시설물 파손 데이터(도로부속시설물 정상 및 파손 → 총 16개 클래스)

### Data Converting
**Annotation file** : 각 json 파일에서 아래 정보만 추출해서 활용

```
.json dataset (image 1개에 annotation 여러개 존재)
├─ image                  
│   ├─ id                 
│   ├─ width              # 1920
│   ├─ height             # 1080
│   └─ file_name          # real_sunny_AM_20220614_112456_sedan_Dong-gu_00026.jpg
└─ annotations                
    ├─ id                 # detected image id
    ├─ image_id           # original image id
    ├─ category_id        # class name
    └─ bbox               # 좌상단x, 좌상단y, bbox의 W, bbox의 H
```
**Bounding Box** : 

YOLOv8 형식으로 추출한 bbox 데이터 정규화
YOLOv8의 바운딩박스  [x_center, y_center, width, height] 형식과 수집한 데이터의 바운딩박스 [좌상단 x좌표, 좌상단 y좌표, bbox_width, bbox_height] 형식이 달라서 center point를 잡으면서 데이터 정규화하는 과정진행



## Modeling
### Transfer Learning
  -  YOLOv8n (2023. 1. release) → 기존 YOLO 모델 대비 빠른 속도를 제공하는 **v8** & 세부모델 중 낮은 복잡도와 빠른 추론속도인 **nano** 모델 선택
  -  Experiment : 세부모델 선정을 위해서 FPS 측정 진행 (목표 - 10FPS 이상)
  -  COCO Dataset으로 학습된 YOLOv8n + Custom Dataset (**학습환경: colab GPU (Tesla T4))

#### 1차 학습 (20,000개의 무작위 데이터로 진행)
  -  모든 class 에 대한 mAP는 0.873으로 비교적 높게 나왔으나(*20 epoch), 데이터별로 편차가 심함
  -  이는 전체 train dataset에서 검출된 객체를 나타내는 instances 의 숫자가 너무 차이나서 발생한 결과라고 볼 수 있겠음

#### 2차 학습 (instances 개수를 비교적 균등하게 맞춤)
  -  모든 class 를 비슷하게 해서(각 class 당 1000개씩은 모으기! 1000개 찍으면 다른 걸로 넘어가는 걸로, minimum 개수를 설정) →  Dataset : 총 14,821 장 사용
  -  그 외 조건은 모두 1차 Training과 동일한 조건으로 진행
  -  1차학습보다 사용한 이미지 양이 적어서, 전체 카테고리에 대해서 평균을 낸 mAP는 0.01 정도 감소했지만, 1차학습에서 낮게 나온 카테고리들의 mAP 값을 높인 2차학습 모델을 채택!

### Model Evaluation
  -  학습에 사용되지 않은 새로운 test dataset 약 3,000장으로 test 했을 때, 1차 모델은 0.675, 2차 모델은 0.802로 약 18.81% mAP가 증가한 것을 확인할 수 있었다.
  -  개별 category를 살펴보아도, 대부분의 category mAP 값이 1차 모델 대비 2차모델에서 상승한 것을 볼 수 있다.


## Inference Logic
<p align="center">
  <img src="https://github.com/jsj5605/ROADs/assets/95035134/4bd41722-4adc-4c4c-af46-7de706afec77">
</p>

## Project & Member
#### [Period] 2024. 1. 10. ~ 2024. 2. 5.
#### 👩🏻‍💻 Team Member
- 조성준(PM, DB설계, 데이터 수집)
- 박주현(데이터 수집 및 정제, 모델 학습 및 평가, Inference code 설계, DB 연동)
- 이신철(데이터 수집 및 정제, 모델 학습, Tracking code 설계, Flask 구현)
- 이장한(웹페이지 개발, DB구축, Back-end)

