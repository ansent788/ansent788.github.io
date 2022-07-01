---
title: Python 媒体识别包 Mediapipe - 面网格（FaceMesh）
date: 2021-09-30 15:14:59
categories:
  - 学习笔记
tags:
  - Python
  - Mediapipe
  - OpenCV
---

### 面部网格（FaceMesh）

<!-- more -->

```python
import cv2
import mediapipe as mp

mp_drawing = mp.solutions.drawing_utils
mp_drawing_styles = mp.solutions.drawing_styles
mp_face_mesh = mp.solutions.face_mesh

with mp_face_mesh.FaceMesh(
    static_image_mode=False,
    max_num_faces=20,
    min_detection_confidence=0.7,
    min_tracking_confidence=0.7
) as face_mesh:

    drawing_spec = mp_drawing.DrawingSpec((255, 0, 255), 1, 1)
    cap = cv2.VideoCapture(0)

    while True:
        success, img = cap.read()
        if success:
            results = face_mesh.process(cv2.cvtColor(img, cv2.COLOR_BGR2RGB))

            if results.multi_face_landmarks is None:
                continue

            for face in results.multi_face_landmarks:
                print("face: ", face)
                mp_drawing.draw_landmarks(
                    image=img,
                    landmark_list=face,
                    connections=mp_face_mesh.FACEMESH_TESSELATION,
                    landmark_drawing_spec=None,
                    connection_drawing_spec=mp_drawing_styles.get_default_face_mesh_tesselation_style()
                )
                mp_drawing.draw_landmarks(
                    image=img,
                    landmark_list=face,
                    connections=mp_face_mesh.FACEMESH_CONTOURS,
                    landmark_drawing_spec=None,
                    connection_drawing_spec=mp_drawing_styles.get_default_face_mesh_contours_style()
                )
            cv2.imshow('face mash', img)

            if cv2.waitKey(1) > 0:
                break

    cap.release()
```

### 其它功能

- [人脸检测](../Face/)
- 面网格
- [手](../Hands/)
- [整体](../Holistic/)
- [物体空间](../Objectron/)
- [姿势](../Pose/)
- [姿势分类](../PoseClassification/)
- [自拍分割](../SelfieSegmentation/)
