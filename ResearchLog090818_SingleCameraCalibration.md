# Research Log 090818 - Single Camera Calibration

---

Two important papers:

+ [A Flexible New Technique for Camera Calibration *Zhengyou Zhang*](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.220.534&rep=rep1&type=pdf)
+ [Decentering Distortion of Lenses *Duance C. Brown*](https://web.archive.org/web/20180312205006/https://www.asprs.org/wp-content/uploads/pers/1966journal/may/1966_may_444-462.pdf)

---

## Assumptions

We first focus on the no-distortion and no-skew camera model.

---

We can get a homography $\mathit{H}$ of each view of the chessboard. We  can write $\mathit{H}$ as column vectors
$$
\mathit{H}=
\begin{bmatrix}
\vec{\mathit{h}_{1}},&\vec{\mathit{h}_{2}},&\vec{\mathit{h}_{3}}
\end{bmatrix}\tag{1}
$$

which $\vec{\mathit{h}_{i}}\in\mathit{R}_{3\times1}\,,\,i\in1\sim3$.
$$
\mathit{H}=
\begin{bmatrix}
\vec{\mathit{h}_{1}},&\vec{\mathit{h}_{2}},&\vec{\mathit{h}_{3}}
\end{bmatrix}=s\cdot\mathit{M}\cdot\begin{bmatrix}
\vec{\mathit{r}_{1}},&\vec{\mathit{r}_{2}},&\vec{\mathit{t}}
\end{bmatrix}\tag{2}
$$

$$
\begin{cases}
\vec{h}_{1}=s\cdot M \cdot \vec{r}_{1}\\
\vec{h}_{2}=s\cdot M \cdot \vec{r}_{2}\\
\vec{h}_{3}=s\cdot M \cdot \vec{t}
\end{cases}\tag{3}
$$

The rotation vectors are orthogonal to each other. We extract the scale of the rotation vectors and combine with the $s$ which causes $\vec{r}_{1}$ and $\vec{2}$ to be orthonormal.

The dot product of the two orthonormal vector is zero. 
$$
\vec{r}_{1}^{T}\cdot\vec{r}_{2}=0\tag{4}
$$
We can use the equation to get the $1_{st}$ constrain:
$$
\begin{split}
\vec{r}_{1}^{T}\cdot\vec{r}_{2}
&=[\frac{1}{s}\vec{h}_{1}^{T}\cdot M^{-T}]\cdot[\frac{1}{s}\cdot M^{-1}\cdot\vec{h}_{2}]\\
&=\vec{h}_{1}^{T}\cdot M^{-T}\cdot M^{-1}\cdot\vec{h}_{2}\\
&=0
\end{split}\tag{5}
$$
We also known that  $\vec{r}_{1}^{T}\cdot \vec{r}_{1}=\vec{r}_{2}^{T}\cdot\vec{r}_{2}$,

so
$$
\vec{h}_{1}^{T}\cdot M^{-T}\cdot M^{-1}\cdot\vec{h}_{1}=\vec{h}_{2}^{T}\cdot M^{-T}\cdot M^{-1}\cdot\vec{h}_{2}. \tag{6}
$$


We define a new symbol $B$ which 


$$
\begin{split}
\mathit{B}&=M^{-T}\cdot M^{-1}\\
&=\begin{bmatrix}
\frac{1}{f_{x}^{2}}&0&\frac{-c_{x}}{f_{x}^{2}}\\
0&\frac{1}{f_{y}^{2}}&\frac{-c_{y}}{f_{y}^{2}}\\
\frac{-c_{x}}{f_{x}^{2}}&\frac{-c_{y}}{f_{y}^{2}}&\frac{c_{x}}{f_{x}^{2}}+\frac{c_{y}}{f_{y}^{2}}+1
\end{bmatrix}
\end{split}.\tag{7}
$$
$B$ is a symmetric matrix, so it can be used to rewrite equation (5) and (6) by this following property:
$$
\begin{split}
\vec{h}_{i}^{T}\cdot B \cdot \vec{h}_{j}&
=h_{j,i}(h_{i,1}B_{1,1}+h_{i,2}B_{2,1}+h_{i,3}B_{3,1})\\&\quad
+h_{j,2}(h_{i,1}B_{1,2}+h_{i,2}B_{2,2}+h_{i,3}B_{3,2})\\&\quad
+h_{j,3}(h_{i,1}B_{1,3}+h_{i,3}B_{2,3}+h_{i,3}B_{3,3})
\\&=
\begin{bmatrix}
h_{i,1}h_{j,1}\\(h_{i,1}h_{j,2}+h_{i,2}h_{j,1})
\\h_{i,2}h_{j,2}\\(h_{i,3}h_{j,1}+h_{i,1}h_{j,3})\\
(h_{i,3}h_{j,2}+h_{i,2}h_{j,3})\\ h_{i,3}h_{j,3}
\end{bmatrix}^{T}\cdot
\begin{bmatrix}
B_{11}\\B_{12}\\B_{22}\\B_{13}\\B_{23}\\B_{33}
\end{bmatrix}\\
&=\vec{v}_{i,j}^{T}\cdot\vec{b}
\end{split}.\tag{8}
$$

So equation (5) becomes
$$
\vec{v}_{1,2}^{T}.\vec{b}=0 \tag{9}
$$
and equation (6) can be reformed as 
$$
\begin{split}
\vec{h}_{1}^{T}\cdot M^{-T}\cdot M^{-1}\cdot\vec{h}_{1}-\vec{h}_{2}^{T}\cdot M^{-T}\cdot M^{-1}\cdot\vec{h}_{2}&=\vec{h}_{1}^{T}\cdot B \cdot\vec{h}_{1}-\vec{h}_{2}^{T}\cdot B\cdot\vec{h}_{2}\\
&=\{\vec{v}_{1,1}^{T}-\vec{v}_{2,2}^{T}\}\vec{b}\\
&=0
\end{split}\tag{10}
$$
With equation (9) and (10), we can get the solution of $\vec{b}$ and get the intrinsic coefficients:
$$
\begin{cases}
f_{x}=\sqrt{\frac{\lambda}{B_{11}}}\\
f_{y}=\sqrt{\frac{\lambda B_{11}}{B_{11}B_{22}-B_{12}^{2}}}\\
c_{x}=\sqrt{\frac{B_{13}f_{x}^{2}}{\lambda}}\\
c_{y}=\frac{B_{12}B_{13}-B_{11}B_{23}}{B_{11}B_{22}-B_{12}^{2}}\\
\lambda = \frac{1}{s}=B_{33}-\frac{(B_{13}^{2}+c_{y}(B_{12}B_{13}-B_{11}-B_{23}))}{B_{11}}
\end{cases}\tag{11}
$$

The extrinsics(rotation and translation) can be get by the following equations:
$$
\begin{cases}
\vec{r}_{1}=\lambda\cdot M^{-1}\cdot\vec{h}_{1}\\
\vec{r}_{2}=\lambda\cdot M^{-1}\cdot\vec{h}_{2}\\
\vec{r}_{3}=\vec{r}_{1}\times\vec{r}_{2}\\
\vec{t}= \lambda\cdot M^{-1}\cdot\vec{h}_{3}
\end{cases}\tag{12}
$$

---

If we consider the distortion parameters, we can use the formula to correct that
$$
\begin{bmatrix}
x_{p}\\y_{p}
\end{bmatrix}
=(1+k_{1}r^{2}+k_{2}r^{4}+k_{3}r^{6})
\begin{bmatrix}
x_{d}\\y_{d}
\end{bmatrix}+
\begin{bmatrix}
2p_{1}x_{d}y_{d}+p_{2}(r^{2}+2x_{d}^{2})\\
p_{1}(r^{2}+2y_{d}^{2}+2p_{2}x_{d}y_{d})
\end{bmatrix}\tag{13}
$$
