## 图像的开运算和闭运算

**腐蚀**和**膨胀**是对像素值大的部分而言的，即高亮白部分而不是黑色部分；以下图片前景物体为高亮像素，背景为低亮像素。

**膨胀(dilated)**是图像中的高亮部分进行膨胀，领域扩张，效果图拥有比原图更大的高亮区域；操作的时候表现为相邻区域用极大值代替，高亮区域增加。

**腐蚀(eroded)**是图像中的高亮部分被腐蚀掉，领域缩减，效果图拥有比原图更小的高亮区域；操作的时候表现为相邻区域用极小值代替,高亮区域减少。

### 开运算

开运算的步骤是先先腐蚀后膨胀

* 开运算能够除去孤立的小点，毛刺和小桥，而总的位置和形状不便。
* 开运算是一个基于几何运算的滤波器。
* 结构元素大小的不同将导致滤波效果的不同。
* 不同的结构元素的选择导致了不同的分割，即提取出不同的特征。

### 闭运算

闭运算是先膨胀后腐蚀

* 闭运算能够填平前景物体内的小裂缝，而总的位置和形状不变。
* 闭运算是通过填充图像的凹角来滤波图像的。
* 结构元素大小的不同将导致滤波效果的不同。
* 不同结构元素的选择导致了不同的分割。

#### Python Code

```python
import cv2
# 读取图片
img = cv2.imread('./image.png',0)
# 定义核结构元素
kernel = cv2.getStructuringElement(cv2.MORPH_RECT,(3, 3))
# 腐蚀图像
eroded = cv2.erode(img,kernel)
# 显示腐蚀后的图像
cv2.imshow("Eroded Image",eroded)
#膨胀图像
dilated = cv2.dilate(img,kernel)
#显示膨胀后的图像
cv2.imshow("Dilated Image",dilated);
# 开运算
opened = cv2.morphologyEx(img, cv2.MORPH_OPEN, kernel)
# 显示开运算后的图像
cv2.imshow("Open", opened)
#闭运算
closed = cv2.morphologyEx(img, cv2.MORPH_CLOSE, kernel)
#显示闭运算后的图像
cv2.imshow("Close",closed);
```





