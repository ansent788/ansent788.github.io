---
title: Python 媒体识别包 Mediapipe - 整体（Holistic）
date: 2021-09-30 15:15:54
categories:
  - 学习笔记
tags:
  - Python
  - Mediapipe
  - OpenCV
---

### 整体识别（Holistic）

<!-- more -->

```python
import typing
import cv2
import mediapipe as mp
from mediapipe.python.solutions.drawing_utils import DrawingSpec

mp_drawing = mp.solutions.drawing_utils
mp_drawing_styles = mp.solutions.drawing_styles
mp_holistic = mp.solutions.holistic

def hiddenPoseFace(landmarks: typing.NamedTuple) -> typing.Any:
    for idx in range(11):
        landmarks.pose_landmarks.landmark[idx].visibility = 0

with mp_holistic.Holistic(
    min_detection_confidence=0.5,
    min_tracking_confidence=0.5
) as holistic:
    cap = cv2.VideoCapture(0)
    while True:
        success, img = cap.read()
        if success:
            img = cv2.flip(img, 1)
            results = holistic.process(cv2.cvtColor(img, cv2.COLOR_BGR2RGB))

            hiddenPoseFace(results)

            mp_drawing.draw_landmarks(
                img, results.pose_landmarks, mp_holistic.POSE_CONNECTIONS,
                landmark_drawing_spec=DrawingSpec((0, 255, 0), 4, 4)
            )

            mp_drawing.draw_landmarks(
                img, results.face_landmarks, mp_holistic.FACEMESH_CONTOURS,
                landmark_drawing_spec=None,
                connection_drawing_spec=mp_drawing_styles.get_default_face_mesh_contours_style()
            )
            mp_drawing.draw_landmarks(
                img, results.face_landmarks, mp_holistic.FACEMESH_TESSELATION,
                landmark_drawing_spec=None,
                connection_drawing_spec=mp_drawing_styles.get_default_face_mesh_tesselation_style()
            )

            cv2.imshow('综合识别', img)
            if cv2.waitKey(1) > 0:
                break
```

### 其它功能

- [人脸检测](../Face/)
- [面网格](../FaceMesh/)
- [手](../Hands/)
- 整体
- [物体空间](../Objectron/)
- [姿势](../Pose/)
- [姿势分类](../PoseClassification/)
- [自拍分割](../SelfieSegmentation/)
