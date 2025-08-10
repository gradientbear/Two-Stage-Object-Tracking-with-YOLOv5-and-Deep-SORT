# Two Stage Object Tracking with YOLOv5 and Deep Sort

<div align="center">
  <p>
    <img src="MOT16_eval/track_pedestrians.gif" width="400" alt="Pedestrian Tracking"/> 
    <img src="MOT16_eval/track_all.gif" width="400" alt="Multiple Object Tracking"/>
  </p>
</div>

## Introduction

This repository implements a two-stage multi-object tracker combining the state-of-the-art **YOLOv5** object detector with the **Deep SORT** tracking algorithm. YOLOv5 detects objects in each frame, and Deep SORT assigns consistent IDs to track these objects over time. The system can track any class of objects that your YOLOv5 model is trained on.

- YOLOv5: [ultralytics/yolov5](https://github.com/ultralytics/yolov5)
- Deep SORT: [ZQPei/deep_sort_pytorch](https://github.com/ZQPei/deep_sort_pytorch)

---

## Tutorials & Resources

- [Train YOLOv5 on Custom Data](https://github.com/ultralytics/yolov5/wiki/Train-Custom-Data)  
- [Train Deep SORT Re-ID Model](https://github.com/ZQPei/deep_sort_pytorch#training-the-re-id-model)  
- [YOLOv5 + Deep SORT PyTorch Evaluation](https://github.com/mikel-brostrom/Yolov5_DeepSort_Pytorch/wiki/Evaluation)

---

## Setup Instructions

1. **Clone the repository recursively**:

`git clone --recurse-submodules https://github.com/mikel-brostrom/Yolov5_DeepSort_Pytorch.git`

If you already cloned and forgot to use `--recurse-submodules` you can run `git submodule update --init`

2. **Make sure that you fulfill all the requirements**: Python 3.8 or later with all [requirements.txt](https://github.com/mikel-brostrom/Yolov5_DeepSort_Pytorch/blob/master/requirements.txt) dependencies installed, including torch>=1.7. To install, run:

`pip install -r requirements.txt`

---

## Tracking sources

Tracking can be run on most video formats

```bash
python3 track.py --source ... --show-vid  # show live inference results as well
```

- Video:  `--source file.mp4`
- Webcam:  `--source 0`
- RTSP stream:  `--source rtsp://170.93.143.139/rtplive/470011e600ef003a004ee33696235daa`
- HTTP stream:  `--source http://wmccpinetop.axiscam.net/mjpg/video.mjpg`

---

## Select a Yolov5 family model

There is a clear trade-off between model inference speed and accuracy. In order to make it possible to fulfill your inference speed/accuracy needs
you can select a Yolov5 family model for automatic download

```bash
python3 track.py --source 0 --yolo_weights yolov5s.pt --img 640  # smallest yolov5 family model
```

```bash
python3 track.py --source 0 --yolo_weights yolov5x6.pt --img 1280  # largest yolov5 family model
```

---

## Filter tracked classes

By default the tracker tracks all MS COCO classes.

If you only want to track persons I recommend you to get [these weights](https://drive.google.com/file/d/1gglIwqxaH2iTvy6lZlXuAcMpd_U0GCUb/view?usp=sharing) for increased performance

```bash
python3 track.py --source 0 --yolo_weights yolov5/weights/crowdhuman_yolov5m.pt --classes 0  # tracks persons, only
```

If you want to track a subset of the MS COCO classes, add their corresponding index after the classes flag

```bash
python3 track.py --source 0 --yolo_weights yolov5s.pt --classes 16 17  # tracks cats and dogs, only
```

[Here](https://tech.amikelive.com/node-718/what-object-categories-labels-are-in-coco-dataset/) is a list of all the possible objects that a Yolov5 model trained on MS COCO can detect. Notice that the indexing for the classes in this repo starts at zero.

---

## MOT compliant results

Can be saved to `inference/output` by 

```bash
python3 track.py --source ... --save-txt
```

