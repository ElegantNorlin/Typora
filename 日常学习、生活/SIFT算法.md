

### SIFT算子的核心步骤：

* 建立尺度空间，即建立图像金字塔。

  * 高斯金字塔：金字塔的每一层都有相同数量的不同参数下的高斯滤波图，每一层的尺寸相同，越往上尺寸越小。

    高斯金字塔的构建：

    * 对图像做高斯平滑
    * 对图像做降采样

* 在尺度空间中检测极值点，也就是特征点（角点）

  * 从图像金字塔生成DOG金字塔，将每一层的高斯模糊图像两两相减得到DOG图，这样就得到了DOG金字塔。

* 特征点方向赋值，完成此步骤后，每个特征点有三个主要信息：位置、尺度、方向

  ![img](https://gitee.com/wanwanzh/imagebed/raw/master/pictures/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI5MDUxMTA3,size_16,color_FFFFFF,t_70.png)

* 计算特征点的描述子

