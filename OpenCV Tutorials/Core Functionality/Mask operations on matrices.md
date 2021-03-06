在矩阵上使用蒙版矩阵操作是非常简单的。做法是，我们根据蒙版矩阵（被认为是一个核心）的方式对图形的每一个像素点进行重新计算，最后获得计算后的值。这个蒙版值会通过相邻像素值（和本身像素值）的互相影响程度来进行调整，最后产生一个新的值。在数学层面上而言，我们通过我们特定的值做了个加权平均的计算。

测试用例

让我们来考虑一下图像对比度增强算法的问题。首先，我们想让图形的每一个像素的值通过以下公式来改变：

![](http://latex.codecogs.com/gif.latex?I(i,j)=5*I(i,j)-[I(i-1,j)+I(i+1,j)+I(i,j-1)+I(i,j+1)])

![](http://latex.codecogs.com/gif.latex?\iff\I(i,j)*M,\text{where}M=\bordermatrix{_i\backslash^j&-1&0&+1\cr-1&0&-1&0\cr0&-1&5&-1\cr+1&0&-1&0\cr})

第一个记号表示的是第一个公式，而第二个表示的是用蒙版实现的简化版本公式。公式中将蒙版矩阵放到中间以便使用蒙版（用大写字母表示，是第（0,0）个索引元素来处理你想计算的像素值，通过遮罩层矩阵值来对像素值进行加和。不用后者的记号，用一个大型矩阵也是可以表示这个公式，但是用一个记号来表示矩阵会方便阅读。

具体代码请参考原教程
