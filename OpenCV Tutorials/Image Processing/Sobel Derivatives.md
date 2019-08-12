Goal

In this tutorial you will learn how to:

* Use the OpenCV function Sobel() to calculate the derivatives from an image.
* Use the OpenCV function Scharr() to calculate a more accurate derivative for a kernel of size 3⋅3

Theory

> Note
>
> The explanation below belongs to the book Learning OpenCV by Bradski and Kaehler.

1.In the last two tutorials we have seen applicative examples of convolutions. One of the most important convolutions is the computation of derivatives in an image (or an approximation to them).
2.Why may be important the calculus of the derivatives in an image? Let's imagine we want to detect the edges present in the image. For instance:

![](https://docs.opencv.org/4.1.0/Sobel_Derivatives_Tutorial_Theory_0.jpg)

You can easily notice that in an edge, the pixel intensity changes in a notorious way. A good way to express changes is by using derivatives. A high change in gradient indicates a major change in the image.

3.To be more graphical, let's assume we have a 1D-image. An edge is shown by the "jump" in intensity in the plot below:

![](https://docs.opencv.org/4.1.0/Sobel_Derivatives_Tutorial_Theory_Intensity_Function.jpg)

4.The edge "jump" can be seen more easily if we take the first derivative (actually, here appears as a maximum)

![](https://docs.opencv.org/4.1.0/Sobel_Derivatives_Tutorial_Theory_dIntensity_Function.jpg)

5.So, from the explanation above, we can deduce that a method to detect edges in an image can be performed by locating pixel locations where the gradient is higher than its neighbors (or to generalize, higher than a threshold).

6.More detailed explanation, please refer to Learning OpenCV by Bradski and Kaehler
Sobel Operator

1.The Sobel Operator is a discrete differentiation operator. It computes an approximation of the gradient of an image intensity function.

2.The Sobel Operator combines Gaussian smoothing and differentiation.

Formulation

Assuming that the image to be operated is I:

1.We calculate two derivatives:

    a.Horizontal changes: This is computed by convolving I with a kernel Gx with odd size. For example for a kernel size of 3, Gx would be computed as:
    
    ![](http://latex.codecogs.com/gif.latex?G_%7Bx%7D%20%3D%20%5Cbegin%7Bbmatrix%7D%20-1%20%26%200%20%26%20+1%20%5C%5C%20-2%20%26%200%20%26%20+2%20%5C%5C%20-1%20%26%200%20%26%20+1%20%5Cend%7Bbmatrix%7D%20*%20I)

    b.Vertical changes: This is computed by convolving I with a kernel Gy with odd size. For example for a kernel size of 3, Gy would be computed as:
    
    ![](http://latex.codecogs.com/gif.latex?G_%7By%7D%20%3D%20%5Cbegin%7Bbmatrix%7D%20-1%20%26%20-2%20%26%20-1%20%5C%5C%200%20%26%200%20%26%200%20%5C%5C%20+1%20%26%20+2%20%26%20+1%20%5Cend%7Bbmatrix%7D%20*%20I)

At each point of the image we calculate an approximation of the gradient in that point by combining both results above:

![](http://latex.codecogs.com/gif.latex?G%20%3D%20%5Csqrt%7B%20G_%7Bx%7D%5E%7B2%7D%20+%20G_%7By%7D%5E%7B2%7D%20%7D)

2.Although sometimes the following simpler equation is used:

![](http://latex.codecogs.com/gif.download?G%20%3D%20%7CG_%7Bx%7D%7C%20+%20%7CG_%7By%7D%7C)

> Note
> 
> When the size of the kernel is 3, the Sobel kernel shown above may produce noticeable inaccuracies (after all, Sobel is only an approximation of the derivative). OpenCV addresses this inaccuracy for kernels of size 3 by using the Scharr() function. This is as fast but more accurate than the standard Sobel function. It implements the following kernels:
>
> ![](http://latex.codecogs.com/gif.download?G_%7Bx%7D%20%3D%20%5Cbegin%7Bbmatrix%7D%20-3%20%26%200%20%26%20+3%20%5C%5C%20-10%20%26%200%20%26%20+10%20%5C%5C%20-3%20%26%200%20%26%20+3%20%5Cend%7Bbmatrix%7D)
>
> ![](http://latex.codecogs.com/gif.download?G_%7By%7D%20%3D%20%5Cbegin%7Bbmatrix%7D%20-3%20%26%20-10%20%26%20-3%20%5C%5C%200%20%26%200%20%26%200%20%5C%5C%20+3%20%26%20+10%20%26%20+3%20%5Cend%7Bbmatrix%7D)
> 
> You can check out more information of this function in the OpenCV reference - Scharr() . Also, in the sample code below, you will notice that above the code for Sobel() function there is also code for the Scharr() function commented. Uncommenting it (and obviously commenting the Sobel stuff) should give you an idea of how this function works.