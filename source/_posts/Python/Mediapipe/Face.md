---
title: Python 媒体识别包 Mediapipe - 人脸检测（Face）
date: 2021-09-30 15:14:41
categories:
  - 学习笔记
tags:
  - Python
  - Mediapipe
  - OpenCV
---

### 人脸检测（Face）

<!-- more -->

```python
import cv2
import mediapipe as mp
from mediapipe.python.solutions.drawing_utils import DrawingSpec

mp_face_detection = mp.solutions.face_detection
mp_drawing = mp.solutions.drawing_utils
# model_selection: 0 或 1。
# 0 选择最适合距离相机 2 米范围内的人脸的短距离模型，
# 1 表示最适合 5 米以内人脸的全频模型。
with mp_face_detection.FaceDetection(
    min_detection_confidence=0.5, model_selection=0
) as face_detection:
    cap = cv2.VideoCapture(0)

    while True:
        success, img = cap.read()
        if success:
            imgRGB = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)
            results = face_detection.process(imgRGB)

            if results.detections is None:
                continue
            for face in results.detections:
                color = DrawingSpec((255, 0, 255))
                mp_drawing.draw_detection(img, face, color, color)
                cv2.imshow('face', img)
        if cv2.waitKey(1) > 0:
            break

    cap.release()
```

### 其它功能

- 人脸检测
- [面网格](../FaceMesh/)
- [手](../Hands/)
- [整体](../Holistic/)
- [物体空间](../Objectron/)
- [姿势](../Pose/)
- [姿势分类](../PoseClassification/)
- [自拍分割](../SelfieSegmentation/)
