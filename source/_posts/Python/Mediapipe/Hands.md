---
title: Python 媒体识别包 Mediapipe - 手（Hands）
date: 2021-09-30 15:15:42
categories:
  - 学习笔记
tags:
  - Python
  - Mediapipe
  - OpenCV
---

### 手部识别（Hands）

<!-- more -->

1. 编写手部识别类

   ```python
    import typing
    from cv2 import COLOR_BGR2RGB, FILLED, circle, cvtColor
    from mediapipe.python.solutions.drawing_utils import draw_landmarks
    from mediapipe.python.solutions.hands import HAND_CONNECTIONS, Hands
    import numpy as np

    class handDetector():
        def __init__(self, static_image_mode=False, max_num_hands=2, min_detection_confidence=0.5, min_tracking_confidence=0.5) -> typing.Any:
            """
            处理 RGB 图像并返回每只检测到的手的手部标志和惯用性。

            Args:
                static_image_mode: 是否将输入图像视为一批静态且可能不相关的图像，或视频流。
                max_num_hands: 要检测的最大手数。
                min_detection_confidence: 手部检测成功的最小置信值 ([0.0, 1.0])。
                min_tracking_confidence: 要被视为成功跟踪的手部标志的最小置信值 ([0.0, 1.0])。

            Raises:
                RuntimeError: 如果底层图形抛出任何错误。
                ValueError: 如果输入图像不是三通道 RGB。
            """

            self.hands = Hands(
                static_image_mode,
                max_num_hands,
                min_detection_confidence,
                min_tracking_confidence
            )

        def drawHands(self, img: np.ndarray) -> typing.Any:
            """
            返回手部骨骼识别后绘制图片

            Args:
                img: 表示为 numpy ndarray 的 RGB 图像。

            Return:
                绘制手部骨骼后的 numpy ndarray 的 RGB 图像
            """
            results = self.findHands(img)
            if results.multi_handedness is not None:
                for handLms in results.multi_hand_landmarks:
                    draw_landmarks(img, handLms, HAND_CONNECTIONS)
                    for id, lm in enumerate(handLms.landmark):
                        h, w, c = img.shape
                        cx, cy = int(lm.x * w), int(lm.y * h)
                        circle(img, (cx, cy), 2, (0, 255, 0), FILLED)
            return img

        def findHands(self, img: np.ndarray) -> typing.Any:
            """
            返回手部识别骨骼数据

            Args:
                img: 表示为 numpy ndarray 的 RGB 图像。

            Return:
                具有两个字段的 NamedTuple 对象：
                “multi_hand_landmarks”字段包含检测到的每只手上的手部标志
                “multi_handedness”字段包含检测到的手的惯用手（左手与右手）
            """
            imgRGB = cvtColor(img, COLOR_BGR2RGB)
            return self.hands.process(imgRGB)
   ```

2. 识别视频中的手

   ```python
    from time import time
    from Hands import handDetector
    from cv2 import FONT_HERSHEY_PLAIN, VideoCapture, imshow, putText, waitKey

    # 打开视频链接
    cap = VideoCapture(0)

    # 创建识别器，并开始识别
    hands = handDetector(
        static_image_mode=False,
        max_num_hands=2,
        min_detection_confidence=0.8,
        min_tracking_confidence=0.8
    )
    start = 0
    end = 0
    while True:
        start = time()
        success, img = cap.read()
        if success:
            img = hands.drawHands(img)
            end = time()
            fps = 1 / (end - start)
            putText(img, f'FPS:{int(fps)}', (10, 70),
                    FONT_HERSHEY_PLAIN, 3, (0, 255, 0), 3)
            imshow('hand', img)
            if waitKey(1) > 0:
                break

    cap.release()
   ```

### 其它功能

- [人脸检测](../Face/)
- [面网格](../FaceMesh/)
- 手
- [整体](../Holistic/)
- [物体空间](../Objectron/)
- [姿势](../Pose/)
- [姿势分类](../PoseClassification/)
- [自拍分割](../SelfieSegmentation/)
