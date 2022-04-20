# 物件偵測專班 - Person ReID 專題實作
![c1_11](https://user-images.githubusercontent.com/41776044/164142014-0709b4a2-94cc-4796-b8cf-8dce5e25b5f0.jpg)

## Motivation
我們想在不同攝影機間找到對應的目標對象，進而做到更多事情，例如：
1. 在街道上可以幫助警察尋找通緝犯或肇事者。
2. 在遊樂園上可以幫助家長尋找走失的小朋友。
3. 在商場上可以配合推薦系統搭建出個性化的推薦服務。
4. 在相簿中可以將同一個人的照片分類在一起，方便管理。

## Target
我們希望能在校園中找到相應的人，但是由於攝影機的解析度等硬體缺陷很難得到清晰的人臉，因此人臉辨識技術不太適用於此場景，我們需要使用 ReID (Re-identification) 技術來解決此問題。

<img width="1080" alt="image" src="https://user-images.githubusercontent.com/41776044/163923575-4638f000-0832-48c9-bd6f-08f56b702e3a.png">

## Datasets

這個資料集是在 [**PRW (Person Re-identification in the Wild) Dataset**](http://zheng-lab.cecs.anu.edu.au/Project/project_prw.html) 中縮減得來的，共由 6 個攝影機收集而來，其中 5 個高清鏡頭 (1920×1080)、1 個低清鏡頭 (720 × 576)。

<img width="1080" alt="image" src="https://user-images.githubusercontent.com/41776044/163923694-08d4d75f-c59d-4212-a704-299612f749ed.png">

### Training data:
Cam 4, Cam 5, Cam 6 為訓練資料，共有 723 張影像，其中包含 100 個不同的人，共有 1438 個行人框 (ReID Bbox)。

<img width="1080" alt="image" src="https://user-images.githubusercontent.com/41776044/163923758-03b30bc5-ad16-4e0e-b227-b1cbc857be33.png">

|     | Frame | ReID Bbox |
| :-: | :-: | :-: |
| Cam 4 | 62 | 87 | 
| Cam 5 | 190 | 431 | 
| Cam 6 | 471 | 920 | 
| Total | 723 | 1438 | 

### Testing data:
Cam 1, Cam 2, Cam 3 為測試資料，共有 153 張影像，需要偵測出行人，並知道是 100 個人中的哪一個人。

<img width="1080" alt="image" src="https://user-images.githubusercontent.com/41776044/163923784-b787d751-d14f-49ca-b0c3-81315805cf14.png">

|     | Frame |
| :-: | :-: | 
| Cam 1 | 72 |  
| Cam 2 | 34 |  
| Cam 3 | 47 |  
| Total | 153 | 

## Pedestrian Detection

### Pretrained Models & Comparison

| Model | Test Size | AP<sup>val</sup> | AP<sub>50</sub><sup>val</sup> | AP<sub>75</sub><sup>val</sup> | AP<sub>S</sub><sup>val</sup> | AP<sub>M</sub><sup>val</sup> | AP<sub>L</sub><sup>val</sup> | weights |
| :-- | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | 
| [**YOLOv4**](/cfg/yolov4.cfg) | 640 | **49.7%** | **68.2%** | **54.3%** | **32.9%** | **54.8%** | **63.7%** | - |
|  |  |  |  |  |  |  |
| [**YOLOR-CSP**](/cfg/yolor_csp.cfg) | 640 | **49.2%** | **67.6%** | **53.7%** | **32.9%** | **54.4%** | **63.0%** | [yolor-csp.pt](https://drive.google.com/file/d/1ZEqGy4kmZyD-Cj3tEFJcLSZenZBDGiyg/view?usp=sharing) |
| [**YOLOR-P6**](/cfg/yolor_p6.cfg) | 1280 | **52.5%** | **70.6%** | **57.4%** | **37.4%** | **57.3%** | **65.2%** | [yolor-p6.pt](https://drive.google.com/file/d/1WyzcN1-I0n8BoeRhi_xVt8C5msqdx_7k/view?usp=sharing) |
| [**YOLOR-W6**](/cfg/yolor_w6.cfg) | 1280 | **54.0%** | **72.1%** | **59.1%** | **38.1%** | **58.8%** | **67.0%** | [yolor-w6.pt](https://drive.google.com/file/d/1KnkBzNxATKK8AiDXrW_qF-vRNOsICV0B/view?usp=sharing) |
| [**YOLOR-E6**](/cfg/yolor_e6.cfg) | 1280 | **54.6%** | **72.5%** | **59.8%** | **39.9%** | **59.0%** | **67.9%** | [yolor-e6.pt](https://drive.google.com/file/d/1jVrq8R1TA60XTUEqqljxAPlt0M_MAGC8/view?usp=sharing) |
| [**YOLOR-D6**](/cfg/yolor_d6.cfg) | 1280 | **55.4%** | **73.5%** | **60.6%** | **40.4%** | **60.1%** | **68.7%** | [yolor-d6.pt](https://drive.google.com/file/d/1WX33ymg_XJLUJdoSf5oUYGHAtpSG2gj8/view?usp=sharing) |
|  |  |  |  |  |  |  |

### YOLOv4 
#### Training:

```
python PyTorch_YOLOv4/train.py --device 0 --batch-size 4 --img-size 416 --data data/pedestrian.yaml --cfg cfg/yolov4_pedestrian.cfg --weights '' --name yolov4_pedestrian --epochs 300 --multi-scale
```

#### Testing:

```
python PyTorch_YOLOv4/test.py --img 416 --conf 0.001 --batch 8 --device 0 --data data/pedestrian.yaml --names data/pedestrian.names --cfg cfg/yolov4_pedestrian.cfg --weights weights/yolov4.pt
```

| Class | Images | Targets | Precision | Recall | mAP@.5 | mAP@.5:.95 |
| :-: | :-: | :-: | :-: | :-: | :-: | :-: |
| all | 145 | 269 | 0.829 | 0.967 | 0.971 | 0.711 |


### YOLOR_CSP 
#### Training:

```
python yolor/train.py --device 0 --batch-size 8 --img 640 640 --data data/pedestrian.yaml --cfg cfg/yolor_csp_pedestrian.cfg --weights 'yolor_csp.pt' --name yolor_csp_pedestrian --hyp data/hyp.scratch.640.yaml --epochs 300 --multi-scale
```

#### Testing:

```
python yolor/test.py --img 640 --conf 0.001 --batch 8 --device 0 --data data/pedestrian.yaml --names data/pedestrian.names --cfg cfg/yolor_csp_pedestrian.cfg --weights weights/yolor_csp.pt
```

| Class | Images | Targets | Precision | Recall | mAP@.5 | mAP@.5:.95 |
| :-: | :-: | :-: | :-: | :-: | :-: | :-: |
| all | 145 | 269 | 0.919 | 0.967 | 0.977 | 0.764 |


### YOLOR_P6 
#### Training:

```
python yolor/train.py --device 0 --batch-size 4 --img 1280 1280 --data pedestrian.yaml --cfg cfg/yolor_p6_pedestrian.cfg --weights 'yolor_p6.pt' --name yolor_p6_pedestrian --hyp data/hyp.scratch.1280.yaml --epochs 300
```

#### Testing:

```
python yolor/test.py --img 1280 --conf 0.001 --batch 8 --device 0 --data data/pedestrian.yaml --names data/pedestrian.names --cfg cfg/yolor_p6_pedestrian.cfg --weights weights/yolor_p6.pt
```

| Class | Images | Targets | Precision | Recall | mAP@.5 | mAP@.5:.95 |
| :-: | :-: | :-: | :-: | :-: | :-: | :-: |
| all | 145 | 269 | 0.922 | 0.952 | 0.969 | 0.751 |

## Person ReID
### Pretrained Models & Comparison

| Model | Rank@1 | mAP | weights |
| :-- | :-: | :-: | :-: | 
| PCB | 92.64% | 77.47% | [pcb.pth](https://github.com/d246810g2000/Person_ReID/releases/download/v1.0/pcb.pth) | 
| Swin (all tricks+Circle) | 93.65% | 83.65% | [swin.pth](https://github.com/d246810g2000/Person_ReID/releases/download/v1.0/swin.pth) | 

### Training

- PCB

```
python train.py --name PCB --PCB --train_all --lr 0.02
```

- Swin

```
# all tricks+Circle
python train.py --use_swin --name swin_p0.5_circle_w5 --erasing_p 0.5 --circle --warm_epoch 5; python test.py --name swin_p0.5_circle_w5
```

## Pedestrian Detection + Person ReID

| Detection Model | ReID Model | Dataset | Test Size | AP<sup>test</sup> | AP<sub>50</sub><sup>test</sup> | AP<sub>75</sub><sup>test</sup> |
| :-- | :-: | :-: | :-: | :-: | :-: | :-: |
| YOLOv4 | ResNet-50 (Cosine) | Own Dataset | 416 | 58.61% | 70.39% | 64.48% |
| YOLOv4 | PCB | Market1501 | 416 | 66.64% | 83.13% | 73.84% |
| YOLOv4 | Swin (all tricks+Circle) | Market1501 | 416 | 66.28% | 83.37% | 73.12% |
| YOLOR-CSP | ResNet-50 (Cosine) | Own Dataset | 640 | 62.04% | 72.47% | 69.45% |
| YOLOR-CSP | PCB | Market1501 | 640 | <font color=#FF0000>74.39%</font> | <font color=#FF0000>89.57%</font> | <font color=#FF0000>83.79%</font> |
| YOLOR-CSP | Swin (all tricks+Circle) | Market1501 | 640 | 74.10% | 88.86% | 83.49% |
| YOLOR-P6 | ResNet-50 (Cosine) | Own Dataset | 1280 | 60.38% | 69.66% | 65.96% |
| YOLOR-P6 | PCB | Market1501 | 1280 | 74.17% | 87.52% | 82.12% | 
| YOLOR-P6 | Swin (all tricks+Circle) | Market1501 | 1280 | 74.09% | 87.61% | 81.79% |


## Reference
- [WongKinYiu/PyTorch_YOLOv4](https://github.com/WongKinYiu/PyTorch_YOLOv4)
- [WongKinYiu/ScaledYOLOv4](https://github.com/WongKinYiu/ScaledYOLOv4/tree/yolov4-large)
- [WongKinYiu/yolor](https://github.com/WongKinYiu/yolor)
- [layumi/Person_reID_baseline_pytorch](https://github.com/layumi/Person_reID_baseline_pytorch)
- [MathGaron/mean_average_precision](https://github.com/MathGaron/mean_average_precision)
