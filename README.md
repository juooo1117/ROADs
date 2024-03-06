# ROADs
Road Obstacle AI Detection service


도로시설물 파손에 따른 민원증가 및 유지보수가 미흡하다는 문제점을 발견하였다. 
따라서 운전자들이 사용하는 네비게이션 기반으로, 그들의 주행경로에서 파손 시설물을 자동으로 탐지 후 프로그램 상에서 정보를 관리하는 방법을 사용하여, 빠르게 쾌적한 도로 환경을 구현하고 사고를 방지하고자 한다.


## Dataset

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

## Inference Logic
<p align="center">
  <img src="https://github.com/jsj5605/ROADs/assets/95035134/4bd41722-4adc-4c4c-af46-7de706afec77">
</p>
