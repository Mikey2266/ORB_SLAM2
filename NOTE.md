SLAM.TrackMonocular -> Tracking::GrabImageMonocular -> Tracking::Track -> Tracking::MonocularInitialization -> Tracking::TrackLocalMap

### System::TrackMonocular
* 如果定位模式被激活，关闭局部建图，只定位；如果定位模式被关闭，之前建立的局部地图被release
* 返回：世界坐标系到该帧相机坐标系的变换矩阵
### Tracking::Track
* 初始化 -> 相机位姿跟踪 -> 局部地图跟踪 -> 关键帧处理 -> 姿态保存
* 2种模式：纯追踪模式；追踪时有回环检测和局部建图
* 3种位姿计算方式：
  * 运动模型跟踪：根据上一帧的位姿和速度（位姿变换关系）来计算当前帧的位姿，匹配方式使用上一帧特征点投影到当前帧的方式进行匹配
  * 参考关键帧跟踪：匹配方式使用关键帧BOW向量与当前帧进行匹配
  * 重定位：匹配方式是将不包含前2种已匹配过的的局部地图点投影到当前帧进行匹配，主要应对跟踪丢失
* 5种状态：SYSTME_NOT_READY；NO_IMAGES_YET；NOT_INITIALIZED；OK；LOST
### Tracking::StereoInitialization -> Frame::ComputeStereoMatches
* SAD算法：用于像素块匹配，将2个像素块对应数值之差的绝对值求和来评估相似度
* 亚像素算法
### Tracking::MonocularInitialization
* 连续2帧计算基础矩阵和单应性矩阵，得到2帧的运动初始位姿
### Tracking::TrackLocalMap
* 局部地图包括：局部地图关键帧、局部地图地图点
* 局部地图关键帧：
  * 能够观测到当前帧地图点的关键帧作为局部地图点关键帧
  * 如果局部地图关键帧数量不够的话将这些关键帧的共视关键帧、子关键帧、父关键帧也加到局部关键帧中，直到满足数量要求
  * 将能看到当前帧最多地图点的关键帧设为参考关键帧
* 局部地图跟踪的目的：得到当前帧的初始位姿之后，根据局部地图中的地图点和当前帧进行匹配，进一步优化当前帧位姿和地图点
* 更新局部地图点时，需要将之前的局部地图点数据清空