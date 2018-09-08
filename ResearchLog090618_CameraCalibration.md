# Research Log 0906 : Camera calibration

## Homography matrix

Considering that we want to map a point on a 3D planar into the imager of our camera can be expressed by the homography matrix $\mathbf{H}.$

+  Object point on world coordinate $\vec{Q}=\begin{bmatrix}\mathbf{X}\\\mathbf{Y}\\\mathbf{Z}\\1\end{bmatrix}$
+ Object point projection into the imager $\vec{q}=\begin{bmatrix}x\\y\\1\end{bmatrix}$

The relationship between the $\vec{q}$ and $\vec{Q}$ is that 
$$
\vec{q} = s\cdot\mathbf{H}\cdot\vec{Q} \tag{1}
$$

+ $s$ : scale factor

+ $\mathbf{H}$ : It is the product of the extrinsic and intrinsic matrix.

  + Extrinsic matrix: Rotation matrix and translational vector.

    + $$
      \mathbf{W} = \begin{bmatrix}
      \mathbf{R}&\vec{t}
      \end{bmatrix}\tag{2}
      $$

  + Intrinsic matrix:   $\mathbf{M}$

    + $$
      \mathbf{M} = \begin{bmatrix}
      \mathit{f}_{x}&\mathit{s}&\mathit{c}_{x}\\
      0&\mathit{f}_{x}&\mathit{c}_{y}\\
      0&0&1
      \end{bmatrix}\tag{3}
      $$





So the equation will become as 
$$
\vec{q} = \mathit{s} \cdot\mathbf{M}\cdot\mathbf{W}\cdot\vec{\mathbf{Q}} \tag{4}
$$

For now , we don't consider the skew parameter in the intrinsic matrix $\mathbf{M}$ which will let the matix become
$$
\mathbf{M} = \begin{bmatrix}
      \mathit{f}_{x}&0&\mathit{c}_{x}\\
      0&\mathit{f}_{x}&\mathit{c}_{y}\\
      0&0&1
\end{bmatrix}\tag{5}
$$

We can set the world coordinate on the chess board. Then the $\mathbf{Z}$ in $\vec{Q}$ is zero. 

![Screenshot from 2018-09-06 15-20-59](./ResearchLog090618_CameraCalibration/Screenshot from 2018-09-06 15-20-59.png)

Then the equation 4 will become
$$
\begin{split}
\vec{q} = \begin{bmatrix}
x\\y\\1
\end{bmatrix} &= \mathit{s}\cdot\mathit{M}_{3\times 3}\cdot
\begin{bmatrix}
\vec{r_{1}}&\vec{r_{2}}&\vec{r_{3}}&\vec{t}
\end{bmatrix}_{3\times4}\cdot
\begin{bmatrix}
\mathbf{X}\\ \mathbf{Y}\\ \mathbf{Z}\\1
\end{bmatrix}_{4\times1} = \mathit{s}\cdot\mathit{M}_{3\times 3}\cdot
\begin{bmatrix}
\vec{r_{1}}&\vec{r_{2}}&\vec{t}
\end{bmatrix}_{3\times3}\cdot
\begin{bmatrix}
\mathbf{X}\\ \mathbf{Y}\\1
\end{bmatrix}_{3\times1}\\
&=
\mathit{s}\cdot\mathit{M}_{3\times 3}\cdot
\mathbf{H_{Ori}}_{3\times 4}\cdot
\begin{bmatrix}
\mathbf{X}\\ \mathbf{Y}\\ \mathbf{Z}\\1
\end{bmatrix}_{4\times1} \qquad\qquad\qquad= 
\mathit{s}\cdot\mathit{M}_{3\times 3}\cdot
\mathbf{H_{New}}_{3\times 3}\cdot
\begin{bmatrix}
\mathbf{X}\\ \mathbf{Y}\\1
\end{bmatrix}_{3\times1}
\end{split}
$$
There are $6$ DOFs about $\mathbf{W}$.(Three are the rotation angles and three for translation vector.) For a chessboard image can give us at least eight constrains.(Each point can be expressed as $\begin{bmatrix}\mathbf{X}&\mathbf{Y}&1\end{bmatrix}^{T}$.)  We can get the homography matrix without knowing the intrinsic matrix. There are three common ways to avoid the outlies date effect.

+ **RANSAC**(random sampling with consensus)
+ **LMeDS**(least median of squares)
+ **RHO**(weighted RANSAC)

## Camera calibration





