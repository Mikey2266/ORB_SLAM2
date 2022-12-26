SLAM.TrackMonocular -> Tracking::GrabImageMonocular -> Tracking::Track -> Tracking::MonocularInitialization

### System::TrackMonocular
* 如果定位模式被激活，关闭局部建图，只定位；如果定位模式被关闭，之前建立的局部地图被release
* 返回：世界坐标系到该帧相机坐标系的变换矩阵
### Tracking::Track
* 初始化 -> 相机位姿跟踪 -> 局部地图跟踪 -> 关键帧处理 -> 姿态保存
* 2种模式：纯追踪模式；追踪时有回环检测和局部建图
* 3种位姿计算方式：
  * 运动模型跟踪：匹配方式使用上一帧特征点投影到当前帧的方式进行匹配
  * 参考关键帧跟踪：匹配方式使用关键帧BOW向量与当前帧进行匹配
  * 重定位：匹配方式是将不包含前2种已匹配过的的局部地图点投影到当前帧进行匹配
* 5种状态：SYSTME_NOT_READY；NO_IMAGES_YET；NOT_INITIALIZED；OK；LOST
### Frame::ComputeStereoMatches
* SAD算法：用于像素块匹配，将2个像素块对应数值之差的绝对值求和来评估相似度
* 亚像素算法