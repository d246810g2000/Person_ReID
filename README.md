# 物件偵測專班 - person reid 專題實作

## Motivation
我們想在不同攝影機間找到對應的目標對象，進而做到更多事情，例如：
1. 在街道上可以幫助警察尋找通緝犯或肇事者。
2. 在遊樂園上可以幫助家長尋找走失的小朋友。
3. 在商場上可以配合推薦系統搭建出個性化的推薦服務。
4. 在相簿中可以將同一個人的照片分類在一起，方便管理。

## Target
我們希望能在校園中找到相應的人，但是由於攝影機的解析度等硬體缺陷很難得到清晰的人臉，因此人臉辨識技術不太適用於此場景，我們需要使用 ReID (Re-identification) 技術來解決此問題。
<img width="1080" alt="image" src="https://user-images.githubusercontent.com/41776044/163923575-4638f000-0832-48c9-bd6f-08f56b702e3a.png">



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


