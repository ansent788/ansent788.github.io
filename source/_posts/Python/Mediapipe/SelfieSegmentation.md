---
title: Python 媒体识别包 Mediapipe - 自拍（SelfieSegmentation）
date: 2021-09-30 15:17:15
categories:
  - 学习笔记
tags:
  - Python
  - Mediapipe
  - OpenCV
---

### 自拍分割（SelfieSegmentation）

<!-- more -->

```python
import cv2

from mediapipe.python.solutions.selfie_segmentation import SelfieSegmentation
import numpy as np

with SelfieSegmentation(
    model_selection=1
) as selfie_segmentation:
    bg_img = cv2.imread(
        '/Users/dx/Documents/Python/PaddleXDemo/opencv/mediapipe/images/bg.jpeg')

    cap = cv2.VideoCapture(0)

    while True:
        success, img = cap.read()
        if success:
            img = cv2.cvtColor(cv2.flip(img, 1), cv2.COLOR_BGR2RGB)
            img.flags.writeable = False
            results = selfie_segmentation.process(img)

            img.flags.writeable = True
            img = cv2.cvtColor(img, cv2.COLOR_RGB2BGR)

            # 在背景图像上绘制自拍分割。
            # 要改进边界周围的分割，请考虑应用联合双边过滤器到“results.segmentation_mask”与“image”。
            # 轮廓
            condition = np.stack(
                (results.segmentation_mask,) * 3, axis=-1) > 0.1
            if bg_img.shape[:1] != img.shape[:1]:
                bg_img = cv2.resize(bg_img, (img.shape[1], img.shape[0]))
            # 按轮廓合并
            output_image = np.where(condition, img, bg_img)

            cv2.imshow('自拍抠图', output_image)
            if cv2.waitKey(1) > 0:
                break

    cap.release()

```

### 功能

- [人脸检测](../Face/)
- [面网格](../FaceMesh/)
- [手](../Hands/)
- [整体](../Holistic/)
- [物体空间](../Objectron/)
- [姿势](../Pose/)
- [姿势分类](../PoseClassification/)
- 自拍分割
