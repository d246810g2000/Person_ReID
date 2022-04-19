# 物件偵測專班 - person reid 專題實作

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


