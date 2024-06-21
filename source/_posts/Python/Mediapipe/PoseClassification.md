---
title: Python 媒体识别包 Mediapipe - 姿势分类（PoseClassification）
date: 2021-09-30 15:16:48
categories:
  - 学习笔记
tags:
  - Python
  - Mediapipe
  - OpenCV
---

### 姿势分类（PoseClassification）

<!-- more -->

- 模型训练

  > 官方详细说明：<https://mediapipe.page.link/pose_classification_basic>

  ```python
  import os
  import csv
  from BootstrapHelper import BootstrapHelper
  from FullBodyPoseEmbedder import FullBodyPoseEmbedder
  from PoseClassification import PoseClassifier

  # region Bootstrap images

  # images_in_folder 的必需结构：
  #
  #   fitness_poses_images_in/
  #     pushups_up/
  #       image_001.jpg
  #       image_002.jpg
  #       ...
  #     pushups_down/
  #       image_001.jpg
  #       image_002.jpg
  #       ...
  #     ...
  bootstrap_images_in_folder = 'mediapipe/images/pose/extended/train'

  # 引导图像和 CSV 的输出文件夹。
  bootstrap_images_out_folder = 'mediapipe/images/pose/extended/images_out'
  bootstrap_csvs_out_folder = 'mediapipe/images/pose/extended/csvs_out'

  # 初始化助手。
  bootstrap_helper = BootstrapHelper(
      images_in_folder=bootstrap_images_in_folder,
      images_out_folder=bootstrap_images_out_folder,
      csvs_out_folder=bootstrap_csvs_out_folder,
  )

  # 检查有多少姿势类和图像可用。
  bootstrap_helper.print_images_in_statistics()

  # 引导所有图像。
  # 将限制设置为一些小的数字以进行调试。
  bootstrap_helper.bootstrap(per_pose_class_limit=None)

  # 检查引导了多少图像。
  bootstrap_helper.print_images_out_statistics()

  # 没有检测到姿势的初始引导图像仍然保存在
  # 用于调试目的的文件夹（但不在 CSV 中）。 让我们移除它们。
  bootstrap_helper.align_images_and_csvs(print_removed_items=False)
  bootstrap_helper.print_images_out_statistics()

  # endregion Bootstrap images

  # region Manual filtration

  # # 请手动验证预测并删除姿势预测错误的样本（图像）。 检查是否要求您仅根据预测的地标对姿势进行分类。 如果你不能 - 删除它。
  # # 完成后对齐 CSV 和图像文件夹。
  # # 将 CSV 与过滤后的图像对齐。
  # bootstrap_helper.align_images_and_csvs(print_removed_items=False)
  # bootstrap_helper.print_images_out_statistics()

  # endregion Manual filtration

  # region Automatic filtration

  # 找出异常值。
  # 将姿势地标转换为嵌入。
  pose_embedder = FullBodyPoseEmbedder()

  # 根据姿势数据库对姿势进行分类。
  pose_classifier = PoseClassifier(
      pose_samples_folder=bootstrap_csvs_out_folder,
      pose_embedder=pose_embedder,
      top_n_by_max_distance=30,
      top_n_by_mean_distance=10)

  outliers = pose_classifier.find_pose_sample_outliers()
  print('Number of outliers: ', len(outliers))

  # 分析异常值。
  bootstrap_helper.analyze_outliers(outliers)

  # 删除所有异常值（如果您不想手动选择）。
  bootstrap_helper.remove_outliers(outliers)

  # 去除异常值后将 CSV 与图像对齐。
  bootstrap_helper.align_images_and_csvs(print_removed_items=False)
  bootstrap_helper.print_images_out_statistics()

  # endregion Automatic filtration

  # region Dump for the App


  def dump_for_the_app():
      pose_samples_folder = 'mediapipe/images/pose/extended/fitness_poses_csvs_out'
      pose_samples_csv_path = 'fitness_poses_csvs_out.csv'
      file_extension = 'csv'
      file_separator = ','

      # 文件夹中的每个文件代表一个姿势类。
      file_names = [name for name in os.listdir(
          pose_samples_folder) if name.endswith(file_extension)]

      with open(pose_samples_csv_path, 'w') as csv_out:
          csv_out_writer = csv.writer(
              csv_out, delimiter=file_separator, quoting=csv.QUOTE_MINIMAL)
          for file_name in file_names:
              # Use file name as pose class name.
              class_name = file_name[:-(len(file_extension) + 1)]

              # 一个文件行：`sample_00001,x1,y1,x2,y2,....`。
              with open(os.path.join(pose_samples_folder, file_name)) as csv_in:
                  csv_in_reader = csv.reader(csv_in, delimiter=file_separator)
                  for row in csv_in_reader:
                      row.insert(1, class_name)
                      csv_out_writer.writerow(row)


  # 将过滤后的姿势转储到 CSV 并下载。 如何在 ML Kit 示例应用程序中使用此 CSV。
  # dump_for_the_app()

  # endregion Dump for the App
  ```

- 姿势识别

  > 官方详细说明：<https://mediapipe.page.link/pose_classification_extended>

  ```python
  import cv2
  import numpy as np
  import tqdm
  from mediapipe.python.solutions import drawing_utils as mp_drawing
  from mediapipe.python.solutions import pose as mp_pose

  from EMADictSmoothing import EMADictSmoothing
  from FullBodyPoseEmbedder import FullBodyPoseEmbedder
  from PoseClassificationVisualizer import PoseClassificationVisualizer
  from RepetitionCounter import RepetitionCounter
  from PoseClassification import PoseClassifier

  # 指定您的视频名称和目标姿势类以计算重复次数。
  video_path = 'mediapipe/images/pose/extended/test/5.mp4'
  class_name = 'down'
  out_video_path = 'mediapipe/images/pose/extended/test_out/5_out.mp4'

  # 打开视频。
  video_cap = cv2.VideoCapture(video_path)

  # 获取一些视频参数以生成带分类的输出视频。
  video_n_frames = video_cap.get(cv2.CAP_PROP_FRAME_COUNT)
  video_fps = video_cap.get(cv2.CAP_PROP_FPS)
  video_width = int(video_cap.get(cv2.CAP_PROP_FRAME_WIDTH))
  video_height = int(video_cap.get(cv2.CAP_PROP_FRAME_HEIGHT))

  # 初始化跟踪器、分类器和计数器。
  # 在每个视频之前都这样做，因为它们都有状态。

  # 包含姿势类 CSV 的文件夹。 那应该是你使用的同一个文件夹
  # 构建分类器以输出 CSV。
  pose_samples_folder = 'mediapipe/images/pose/extended/csvs_out'

  # 初始化跟踪器。
  pose_tracker = mp_pose.Pose(static_image_mode=False)

  # 初始化嵌入器。
  pose_embedder = FullBodyPoseEmbedder()

  # 初始化分类器。
  # 确认您使用的参数与引导期间相同。
  pose_classifier = PoseClassifier(
      pose_samples_folder=pose_samples_folder,
      pose_embedder=pose_embedder,
      top_n_by_max_distance=30,
      top_n_by_mean_distance=10)

  # 取消注释以验证分类器使用的目标姿势并查找异常值。
  # outliers =pose_classifier.find_pose_sample_outliers()
  # print('姿势样本异常值的数量（考虑删除它们）：', len(outliers))

  # 初始化 EMA 平滑。
  pose_classification_filter = EMADictSmoothing(
      window_size=10,
      alpha=0.2)

  # 初始化计数器。
  repetition_counter = RepetitionCounter(
      class_name=class_name,
      enter_threshold=6,
      exit_threshold=4)

  # 初始化渲染器。
  pose_classification_visualizer = PoseClassificationVisualizer(
      class_name=class_name,
      plot_x_max=video_n_frames,
      # 如果与 `top_n_by_mean_distance` 相同，则图形看起来更好。
      plot_y_max=10)

  # 对视频运行分类。

  # 打开输出视频。
  out_video = cv2.VideoWriter(out_video_path, cv2.VideoWriter_fourcc(
      *'mp4v'), video_fps, (video_width, video_height))

  frame_idx = 0
  output_frame = None
  with tqdm.tqdm(total=video_n_frames, position=0, leave=True) as pbar:
      while True:
          # 获取视频的下一帧。
          success, input_frame = video_cap.read()
          if not success:
              break

          # 运行姿势跟踪器。
          input_frame = cv2.cvtColor(input_frame, cv2.COLOR_BGR2RGB)
          result = pose_tracker.process(image=input_frame)
          pose_landmarks = result.pose_landmarks

          # 绘制姿势预测。
          output_frame = input_frame.copy()
          if pose_landmarks is not None:
              mp_drawing.draw_landmarks(
                  image=output_frame,
                  landmark_list=pose_landmarks,
                  connections=mp_pose.POSE_CONNECTIONS)

          if pose_landmarks is not None:
              # 获取地标。
              frame_height, frame_width = output_frame.shape[0], output_frame.shape[1]
              pose_landmarks = np.array([[lmk.x * frame_width, lmk.y * frame_height, lmk.z * frame_width]
                                        for lmk in pose_landmarks.landmark], dtype=np.float32)
              assert pose_landmarks.shape == (
                  33, 3), '意外的地标形状： {}'.format(pose_landmarks.shape)

              # 对当前帧的姿势进行分类。
              pose_classification = pose_classifier(pose_landmarks)

              # 使用 EMA 进行平滑分类。
              pose_classification_filtered = pose_classification_filter(
                  pose_classification)

              # 计算重复次数。
              repetitions_count = repetition_counter(
                  pose_classification_filtered)
          else:
              # 没有姿势 => 当前帧没有分类。
              pose_classification = None

              # 仍然向过滤器添加空分类以保持未来帧的正确平滑。
              pose_classification_filtered = pose_classification_filter(dict())
              pose_classification_filtered = None

              # 不要假设那个人被“冻结”而更新计数器。 只是取最新的重复次数。
              repetitions_count = repetition_counter.n_repeats

          # 绘制分类图和重复计数器。
          output_frame = pose_classification_visualizer(
              frame=output_frame,
              pose_classification=pose_classification,
              pose_classification_filtered=pose_classification_filtered,
              repetitions_count=repetitions_count)

          # 保存输出帧。
          out_video.write(cv2.cvtColor(
              np.array(output_frame), cv2.COLOR_RGB2BGR))

          # 显示视频的中间帧以跟踪进度。
          cv2.imshow('姿势分类 - 俯卧撑(down)', np.array(output_frame))
          cv2.waitKey(1)

          frame_idx += 1
          pbar.update()

  # 关闭输出视频。
  out_video.release()

  # 释放MediaPipe资源。
  pose_tracker.close()

  cv2.waitKey()

  ```

### 其它功能

- [人脸检测](../Face/)
- [面网格](../FaceMesh/)
- [手](../Hands/)
- [整体](../Holistic/)
- [物体空间](../Objectron/)
- [姿势](../Pose/)
- 姿势分类
- [自拍分割](../SelfieSegmentation/)
