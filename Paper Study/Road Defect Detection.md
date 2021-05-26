缺陷的分类

| Damage Type | Detail                                     |
| :---------: | ------------------------------------------ |
|     D00     | Wheel-mark part(Longitudinal Linear Crack) |
|     D10     | Equal interval(Lateral Linear Crack)       |
|     D20     | Partial/Overall pavement(Alligator Crack)  |
|     D40     | Pothole                                    |



* 数据集
  * [RDD2020](https://data.mendeley.com/datasets/5ty2wb6gvg/1)目前最常用的RDD数据集
    * [RDD2020数据集介绍1](https://medium.com/geekculture/build-your-own-custom-road-damage-detector-ddcfcc405771)
    * [RDD2020数据集介绍2](http://downtime.data.mendeley.com/)
  * [gaps-datasets](https://www.tu-ilmenau.de/en/neurob/data-sets-code/gaps/)
* 三种主流实现算法
  * image processing：阈值分割、边缘检测、区域生长
  * machine learning（include deep learning）：如神经网络和支持向量机，仍依赖于图像处理，深度学习主要用到了卷积神经网络。处理问题的方法大概分为三种：图像分类（CNN网络）、目标检测方法（R-CNN、SSD、YOLO）、像素级分割
  * 3D image：利用3维数据进行裂纹检测是一个新的研究和应用领域
* 代码实现
  * [Transfer Learning-based Road Damage Detection for Multiple Countries](https://github.com/sekilab/RoadDamageDetector)
    * 论文来源：CVPR
    * 数据集：RDD2020
  * [Deep Learning Frameworks for Pavement Distress  Classification: A Comparative Analysis](https://github.com/titanmu/RoadCrackDetection)
    * 论文来源：CVPR
    * 数据集：RDD2020
  * [Improved Pixel-Level Pavement-Defect Segmentation Using a Deep Autoencoder](https://github.com/rytisss/RoadPavementSegmentation) 
    * 论文来源：MDPI出版的sensors(IF: 3.275)
    * 数据集：[CrackForest](https://github.com/cuilimeng/CrackForest-dataset),GAPs384,Crack500
  * [Unsupervised Pixel-level Road Defect Detection](https://github.com/andreYoo/Adversarial_IFTN)
    * 论文来源：CVPR
    * 数据集：GAPs384,Cracktree200,Crack500,CFD
  * [Deep Learning Frameworks for Pavement Distress Classification: A Comparative Analysis](https://github.com/titanmu/RoadCrackDetection)
    * 论文来源：未知
    * 数据集：RDD2020
  * [An Efficient and Scalable Deep Learning Approach for Road Damage Detection](https://github.com/mahdi65/roadDamageDetection2020)
    * 论文来源：CVPR
    * 数据集：RDD2020
  * [Road Damage Detection using Deep Ensemble Learning](https://github.com/kevaldoshi17/IEEE-Big-Data-2020)
    * 论文来源：CVPR
    * 数据集：RDD2020
  * [Road Damage Detection and Classification with Detectron2 and Faster R-CNN](https://github.com/iDataVisualizationLab/roaddamagedetector)
    * 论文来源：CVPR
    * 数据集：RDD2020
* 现存问题：
  * 在雨天光线不好的环境下或有水的路面，道路的裂纹检测可能会有一定的误差，鲁棒性并不高
  * 算法的移植性比较差（在不同的路面需要用测试不同的算法）
  * 实时性并不好

