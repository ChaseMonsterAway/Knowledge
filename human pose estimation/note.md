# Self-supervised Keypoint Correspondences for Multi-Person Pose Estimation and Tracking in Videos

1. 使用前一帧的信息来补全当前帧中的关键点信息
2. 为了实现多人的追踪，使用关键点进行匹配来区分不同的人
3. 输入为单人图片，通过将单人图片$I_1$进行warp得到部分缺失或者变化的对应图片$I_2$，以这两组图片作为输入，通过一个backbone进行特征提取，分别得到$F_1$和$F_2$，根据关键点的位置在$F_1$上截取local map（3*3），对$F_2$进行卷积操作，得到一个响应图$R_1$。进行特征融合$<F_1, F_2, R_1>$，接一个后处理网络得到refine的map——$R_2$。

