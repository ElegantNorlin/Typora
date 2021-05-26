---

---







YOLOV1:

* 将一幅图像分成SxS个网格(grid cell)，如果某个object的中心 落在这个网格中，则这个网格就负责预测这个object。 

* 每个网格要预测B个bounding box，每个bounding box除了要回归自身的位置之外，还要附带预测一个confidence值。 
  这个confidence代表了所预测的box中含有object的置信度和这个box预测的有多准两重信息，其值是这样计算的： ![img](https://gitee.com/wanwanzh/imagebed-windows/raw/master/pictures/20160317154752063) 

  其中如果有object落在一个grid cell里，第一项取1，否则取0。 第二项是预测的bounding box和实际的groundtruth之间的IoU值。

* 每个bounding box要预测(x, y, w, h)和confidence共5个值，每个网格还要预测一个类别信息，记为C类。则SxS个网格，每个网格要预测B个bounding box还要预测C个categories。输出就是S x S x (5*B+C)的一个tensor。 
  注意：class信息是针对每个网格的，confidence信息是针对每个bounding box的。

* 

