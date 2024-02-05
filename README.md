# ROADs
Road Obstacle AI Detection service




### Dataset

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

### Inference Logic
<p align="center">
  <img src="https://github.com/jsj5605/ROADs/assets/95035134/4bd41722-4adc-4c4c-af46-7de706afec77" width="650" height="100">
</p>

