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

## Object Detection Pretrained Models & Comparison

| Model | Test Size | AP<sup>val</sup> | AP<sub>50</sub><sup>val</sup> | AP<sub>75</sub><sup>val</sup> | AP<sub>S</sub><sup>val</sup> | AP<sub>M</sub><sup>val</sup> | AP<sub>L</sub><sup>val</sup> | weights |
| :-- | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | 
| [**YOLOv4-CSP**](/cfg/yolov4_csp.cfg) | 640 | **49.1%** | **67.7%** | **53.8%** | **32.1%** | **54.4%** | **63.2%** | - |
| [**YOLOv4-CSP-X**](/cfg/yolov4_csp_x.cfg) | 640 | **50.9%** | **69.3%** | **55.4%** | **35.3%** | **55.8%** | **64.8%** | - |
|  |  |  |  |  |  |  |
| [**YOLOv4-P5**](/cfg/yolov4_p5.cfg) | 896 | **51.2%** | **69.8%** | **56.2%** | **35.0%** | **56.2%** | **64.0%** | [yolov4-p5.pt](https://drive.google.com/file/d/1aXZZE999sHMP1gev60XhNChtHPRMH3Fz/view?usp=sharing) |
| [**YOLOv4-P6**](/cfg/yolov4_p6.cfg) | 1280 | **53.9%** | **72.0%** | **59.0%** | **39.3%** | **58.3%** | **66.6%** | [yolov4-p6.pt](https://drive.google.com/file/d/1aB7May8oPYzBqbgwYSZHuATPXyxh9xnf/view?usp=sharing) |
| [**YOLOv4-P7**](/cfg/yolov4_p7.cfg) | 1536 | **55.0%** | **72.9%** | **60.2%** | **39.8%** | **59.9%** | **68.4%** | [yolov4-p7.pt](https://drive.google.com/file/d/18fGlzgEJTkUEiBG4hW00pyedJKNnYLP3/view?usp=sharing)  |
|  |  |  |  |  |  |  |
| [**YOLOR-CSP**](/cfg/yolor_csp.cfg) | 640 | **49.2%** | **67.6%** | **53.7%** | **32.9%** | **54.4%** | **63.0%** | [yolor-csp.pt](https://drive.google.com/file/d/1ZEqGy4kmZyD-Cj3tEFJcLSZenZBDGiyg/view?usp=sharing) |
| [**YOLOR-CSP-X**](/cfg/yolor_csp_x.cfg) | 640 | **51.1%** | **69.6%** | **55.7%** | **35.7%** | **56.0%** | **65.2%** | [yolor-csp-x.pt](https://drive.google.com/file/d/1L29rfIPNH1n910qQClGftknWpTBgAv6c/view?usp=sharing) |
| [**YOLOR-P6**](/cfg/yolor_p6) | 1280 | **52.5%** | **70.6%** | **57.4%** | **37.4%** | **57.3%** | **65.2%** | [yolor-p6.pt](https://drive.google.com/file/d/1WyzcN1-I0n8BoeRhi_xVt8C5msqdx_7k/view?usp=sharing) |
| [**YOLOR-W6**](/cfg/yolor_w6) | 1280 | **54.0%** | **72.1%** | **59.1%** | **38.1%** | **58.8%** | **67.0%** | [yolor-w6.pt](https://drive.google.com/file/d/1KnkBzNxATKK8AiDXrW_qF-vRNOsICV0B/view?usp=sharing) |
| [**YOLOR-E6**](/cfg/yolor_e6) | 1280 | **54.6%** | **72.5%** | **59.8%** | **39.9%** | **59.0%** | **67.9%** | [yolor-e6.pt](https://drive.google.com/file/d/1jVrq8R1TA60XTUEqqljxAPlt0M_MAGC8/view?usp=sharing) |
| [**YOLOR-D6**](/cfg/yolor_d6) | 1280 | **55.4%** | **73.5%** | **60.6%** | **40.4%** | **60.1%** | **68.7%** | [yolor-d6.pt](https://drive.google.com/file/d/1WX33ymg_XJLUJdoSf5oUYGHAtpSG2gj8/view?usp=sharing) |
|  |  |  |  |  |  |  |

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
# yolor_csp
python train.py --batch-size 8 --img 640 640 --data pedestrian.yaml --cfg cfg/yolor_csp_pedestrian.cfg --weights 'yolor_csp.pt' --device 0 --name yolor_csp_pedestrian --hyp hyp.scratch.640.yaml --epochs 300 --multi-scale
```

```
# yolor_p6
python train.py --batch-size 4 --img 1280 1280 --data pedestrian.yaml --cfg cfg/yolor_p6_pedestrian.cfg --weights 'yolor_p6.pt' --device 0 --name yolor_p6_pedestrian --hyp hyp.scratch.1280.yaml --epochs 300
```

## Inference

## Reference
- [WongKinYiu/PyTorch_YOLOv4](https://github.com/WongKinYiu/PyTorch_YOLOv4)
- [WongKinYiu/ScaledYOLOv4](https://github.com/WongKinYiu/ScaledYOLOv4/tree/yolov4-large)
- [WongKinYiu/yolor](https://github.com/WongKinYiu/yolor)
- [layumi/Person_reID_baseline_pytorch](https://github.com/layumi/Person_reID_baseline_pytorch)
