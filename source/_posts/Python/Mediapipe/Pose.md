---
title: Python 媒体识别包 Mediapipe - 姿势（Pose）
date: 2021-09-30 15:16:21
categories:
  - 学习笔记
tags:
  - Python
  - Mediapipe
  - OpenCV
---

### 姿势检测（Pose）

<!-- more -->

```python
import cv2
import mediapipe as mp
import numpy as np

mp_drawing = mp.solutions.drawing_utils
mp_drawing_styles = mp.solutions.drawing_styles
mp_pose = mp.solutions.pose

cap = cv2.VideoCapture(0)

with mp_pose.Pose(
    min_detection_confidence=0.5,
    min_tracking_confidence=0.5
) as pose:
    while cap. isOpened():
        success, img = cap.read()
        if success:
            img = cv2.flip(img, 1)
            imgRGB = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)
            results = pose.process(imgRGB)

            if results.pose_landmarks is None:
                continue

            mp_drawing.draw_landmarks(
                img, results.pose_landmarks, mp_pose.POSE_CONNECTIONS, mp_drawing_styles.get_default_pose_landmarks_style())

            cv2.imshow('Pose', img)
            if cv2.waitKey(1) > 0:
                break

    cap.release()
```

### 其它功能

- [人脸检测](../Face/)
- [面网格](../FaceMesh/)
- [手](../Hands/)
- [整体](../Holistic/)
- [物体空间](../Objectron/)
- 姿势
- [姿势分类](../PoseClassification/)
- [自拍分割](../SelfieSegmentation/)
