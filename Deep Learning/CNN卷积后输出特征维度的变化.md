---

---





### CNN卷积操作后特征维度的变化

* 输入维度：![W_{1} \times H_{1}\times D_{1}](https://gitee.com/wanwanzh/imagebed-windows/raw/master/pictures/gif.latex)，分别代表输入样本的长宽高

* 卷积操作的超参数：
  * 卷积核个数：![K](https://gitee.com/wanwanzh/imagebed-windows/raw/master/pictures/gif.latex) 
  * 卷积核大小：![F\times F](https://gitee.com/wanwanzh/imagebed-windows/raw/master/pictures/gif.latex2) 
  * 滑动步长（Stride）：![S](https://gitee.com/wanwanzh/imagebed-windows/raw/master/pictures/gif.latex3) 
  * 填充（Padding）：![P](https://gitee.com/wanwanzh/imagebed-windows/raw/master/pictures/gif.latex4)
  
* 则输出的维度为![](https://gitee.com/wanwanzh/imagebed-windows/raw/master/pictures/gif.latex5)，其中

  ![image-20210520211331736](https://gitee.com/wanwanzh/imagebed-windows/raw/master/pictures/image-20210520211331736.png) 

* 由于CNN的参数共享机制，每个卷积核的参数个数为![F*F*D_{1}](https://gitee.com/wanwanzh/imagebed-windows/raw/master/pictures/gif.latex10) ，共有![(F*F*D_{1})*K](https://gitee.com/wanwanzh/imagebed-windows/raw/master/pictures/gif.latex11) 个权重和![K](https://gitee.com/wanwanzh/imagebed-windows/raw/master/pictures/gif.latex12) 个偏置

* 若想要卷积后得到的矩阵长宽与卷积前保持一致，则当S=1时：

  * 卷积核为3时 padding 选择1
  * 卷积核为5时 padding 选择2
  * 卷积核为7时 padding 选择3

