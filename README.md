# 物件偵測專班 - person reid 專題實作

## Motivation
- 我們想在不同攝影機間找到對應的目標對象，進而做到更多事情，例如：
1. 在街道上可以幫助警察尋找通緝犯或肇事者。
2. 在遊樂園上可以幫助家長尋找走失的小朋友。
3. 在商場上可以配合推薦系統搭建出個性化的推薦服務。
4. 在相簿中可以將同一個人的照片分類在一起，方便管理。
<img width="965" alt="image" src="https://user-images.githubusercontent.com/41776044/163923350-15b36f39-5434-4ac9-b4a4-b82735b30466.png">


## Training

- YOLOv4:

```
python train.py --batch-size 8 --img 640 640 --data pedestrian.yaml --cfg cfg/yolov4_pedestrian.cfg --weights '' --device 0 --name yolov4_pedestrian --hyp hyp.scratch.640.yaml --epochs 300
```

- ScaledYOLOv4:

```
python train.py --batch-size 8 --img 640 640 --data pedestrian.yaml --cfg cfg/yolov4_csp_pedestrian.cfg --weights '' --device 0 --name scaledyolov4_pedestrian --hyp hyp.scratch.640.yaml --epochs 300
```

- YOLOR:

```
python train.py --batch-size 8 --img 640 640 --data pedestrian.yaml --cfg cfg/yolor_pedestrian.cfg --weights '' --device 0 --name yolor_pedestrian --hyp hyp.scratch.640.yaml --epochs 300 --multi-scale
```

```
python train.py --batch-size 8 --img 640 640 --data pedestrian.yaml --cfg cfg/yolor_csp_pedestrian.cfg --weights 'yolor_csp.pt' --device 1 --name yolor_pedestrian --hyp hyp.scratch.640.yaml --epochs 300 --multi-scale
```

## Inference


