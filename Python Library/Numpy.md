### numpy的ndarray对象

***

**np.array()生成一个ndarray数组，ndarray在程序中的别名是：array。np.array()输出成[]形式，元素由空格分割。**

#### **ndarray对象的属性**

> * **ndarray.ndim  秩，即轴的数量或维度的量**
>
> * **ndarray.shape  ndarray对象的尺度，对于矩阵，n行m列**
>
> * **ndarray.size  ndarray对象元素的个数，相当于  .shape中n*m的值**
>
> * **ndarray.dtype  ndarray对象的元素类型**
>
> * **ndarray.itemsize  ndarray对象中每个元素的大小，以字节为单位**

#### **ndarray对象的创建方法**

> * **从Python中的列表、元组等类型创建ndarray数组**
>
> * **使用numpy中的函数创建ndarray数组，如：arange,ones,zeros等。**
>
>   > * **np.arange(n)  返回ndarray类型，元素从0到n-1**
>   > * **np.ones(shape)  根据shape生成一个全1数组，shape是元组类型**
>   > * **np.zeros(shape)  根据shape生成一个全0数组，shape是元组类型**
>   > * **np.full(shape,val)  根据shape生成一个数组，每一个元素值都是val**
>   > * **np.eye(n) 创建一个正方的n*n单位矩阵，对角线为1，其余为0**
>
>   
>
>   > * **np.ones_like(a)  根据数组a的形状生成一个全1的数组**
>   > * **np.zeros_like(a)  根据数组a的形状生成一个全0的数组**
>   > * **np.full(a,val)  根据数组a的形状生成一个数组，每个元素的值都是val**
>
>   
>
>   > * **np.linspace()  根据起止数据等间隔地填充数据，形成数组**
>   > * **np.concatenate()  将两个或多个数组合并成一个新的数组**
>
>   
>
> * **从字节流（raw bytes）中创建ndarray数组**
>
> * **从文件中读取特定格式，创建ndarray数组**

#### ndarray数组的变换

**对于创建后的ndarray数组，可以对其进行维度变换和元素类型变换。**

> * ndarray数组的维度变换
>
>   > * **.reshape(shape)  不改变数组元素，返回一个shape形状的数组，原数组不变**
>   >
>   > * **.resize(shape)  与.reshape()功能一致，但修改原数组**
>   >
>   > * **.swapaxes(ax1,ax2)  将数组n个维度中的两个维度进行调换**
>   >
>   > * **.flatten()  对数组进行降维，返回折叠后的一维数组，原数组不变**
>   >
>   
> * ndarray数组的类型变换
>
>   > * **new_ndarray =  a.astype(newtype)  astype()方法一定会创建新的数组（原始数据的一个拷贝），即使两个类型不一致**
>
> * ndarray数组转换成python列表
>
>   > * **list  =  ndarray.tolist()将ndarray数组转换成python原生的list列表**

#### ndarray数组的运算

**数组与标量之间的运算：数组与标量之间的运算作用于数组的每一个元素**

**numpy一元函数**

**对ndarray中的数据执行元素级运算的函数**

> * **np.abs(x)   np.fabs(x)  计算数组各元素的绝对值**
> * **np.sqrt(x)  计算数组各元素的平方根**
> * **np.square(x)  计算数组各元素的平方**
> * **np.log(x)   np.log10(x)   np.log2(x)  计算数组各元素的自然对数、10底对数和2底对数**
> * **np.ceil(x)  np.floor(x)  计算数组各元素的ceiling值（不超过元素的整数值）或floor值（小于这个元素的最大整数值）**
> * **np.rint(x)  计算数组各元素的四舍五入值**
> * **np.modf(x)  将数组各元素的小数和整数部分以两个独立的数组形式返回**
> * **np.cos(x)  np.cosh(x)  np.sin(x)  np.sinh(x)  np.tan(x)  np.tanh(x)  计算数组各元素的普通型和双曲型三角函数**
> * **np.exp(x)  计算数组各元素的指数值**
> * **np.sign(x)  计算数组各元素的符号值，1(+),0,-1(-)**

**numpy二元函数**

**numpy的设计理念是希望我们把数组当作一个数值来使用**

> *  **\+   -   *   /   \*\*  两个数组各元素进行对象运算**
>
> * **np.maximum(x,y)   np.fmax()  np.minimum(x,y)  np.fmin()  元素级的最大值/最小值计算**
> * **np.mod(x,y)  元素级的模运算**
> * **np.copysign(x,y)  将数组y中各元素值的符号赋值给数组x对应元素**
> * \>  <  >=  <=  ==  !=  算术比较，产生布尔型数组

