---
title: Python 媒体识别包 Mediapipe - 物体空间（Objectron）
date: 2021-09-30 15:16:07
categories:
  - 学习笔记
tags:
  - Python
  - Mediapipe
  - OpenCV
---

### 物体空间检测（Objectron）

<!-- more -->

```python
import cv2
import mediapipe as mp

mp_drawing = mp.solutions.drawing_utils
mp_objectron = mp.solutions.objectron

with mp_objectron.Objectron(
    static_image_mode=False,
    max_num_objects=5,
    min_detection_confidence=0.5,
    min_tracking_confidence=0.5,
    model_name='Shoe'
) as objectron:
    cap = cv2.VideoCapture(0)
    while cap.isOpened():
        success, img = cap.read()
        if success:
            results = objectron.process(cv2.cvtColor(img, cv2.COLOR_BGR2RGB))

            if results.detected_objects is not None:
                for obj in results.detected_objects:
                    mp_drawing.draw_landmarks(
                        img, obj.landmarks_2d, mp_objectron.BOX_CONNECTIONS)
                    mp_drawing.draw_axis(img, obj.rotation, obj.translation)

        cv2.imshow('物体空间识别', img)
        if cv2.waitKey(1) > 0:
            break

    cap.release()
```

### 其它功能

- [人脸检测](../Face/)
- [面网格](../FaceMesh/)
- [手](../Hands/)
- [整体](../Holistic/)
- 物体空间
- [姿势](../Pose/)
- [姿势分类](../PoseClassification/)
- [自拍分割](../SelfieSegmentation/)
