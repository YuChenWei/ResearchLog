# Research Log 0910 - Stereo Imaging

---

There are four steps to get the depth information from stereo imaging.:

+ Removing the radial and tangential lens distortion(get the undistortion image)
+ Rectifying the images which gives the row aligned images
+ Correspondence the images and output the disparity map
+ Using the triangulation to reprojection and get the depth information

---

## Triangulation

### Assumptions:

+ Undistorted, aligned two cameras
+ Image planes of two cameras are coplanar.
+ The optical axes of two cameras are parallel.
+ The focal lengths of two cameras are same: $f_{l}=f_{t}$.

### Goals

If there are an object point $\mathit{\vec{P}}$ on the camera coordinate, then the point on the image planes are $\mathit{\vec{p}}_{l}$ and $\mathit{\vec{p}}_{r}$ for each image coordinates. The horizontal components of $\mathit{\vec{p}}_{l}$ and $\mathit{\vec{p}}_{r}$ are $\mathit{x}_{l}$ and $\mathit{x}_{r}$ respected to each image coordinates. 

![Screenshot from 2018-09-10 14-32-05](./ResearchLog091018_StereoImage/Screenshot from 2018-09-10 14-32-05.png)

From triangulation, we can use the similar triangles to get the depth information $Z$.
$$
\begin{split}
\frac{T-\{(\mathit{x}_{l}-\frac{w}{2})+(\frac{w}{2}-\mathit{x}_{r}\})}{Z-f} &=
\frac{T-\{\mathit{x}_{l}-\mathit{x}_{r}\}}{Z-f}\\
&=\frac{T}{Z}
\end{split}\tag{1}
$$
We can get that
$$
Z=\frac{f\cdot T}{\mathit{x}_{l}-\mathit{x}_{r}}.\tag{2}
$$
Now we define the the ==disparty== as
$$
D=\mathit{x}_{l}-\mathit{x}_{r}.\tag{3}
$$

## Epipolar geometry

In real world, the two image planes are not coplanar. So we need to define a new geometry to describe the relationship.

There are few terminologies about the epipolar geometry:

+ Epipole: The intersection points of the two image planes and the  line form by the two principle points $O_{l},O_{r}$ . e.c $\mathit{\vec{e}}_{l},\mathit{\vec{e}}_{r}$ 
+ Epipolar line: The line cross through the points of projection $\vec{p}_{l},\vec{p}_{r}$ and the corresponding epipoles $\mathit{\vec{e}}_{l},\mathit{\vec{e}}_{r}$ . 
+ Epipolar plane: The plane form by the three point the point on the object $\vec{P}$  and the two principal points $O_{l},O_{r}$.

![Screenshot from 2018-09-10 15-50-56](./ResearchLog091018_StereoImage/Screenshot from 2018-09-10 15-50-56.png)

$\vec{p}_{l}$ ($\vec{p}_{r}$) is a projection point on the image plane from a possible collection of the line formed by $\vec{P}$ and $\vec{p}_{l}'$( $\vec{p}_{r}'$), So we can't get the depth information from a single camera due to the ill conditioned. 

### Essential matrix

The essential matrix is defined as the coordinates transform of two cameras. As the extrinsic matrix, we can use the rotation matrix and translation vector to describe the transformation.

Recall the extrinsic matrix of the two cameras,
$$
\begin{cases}
\vec{p}_{l}
=
\begin{bmatrix}
R_{l}|T_{l}
\end{bmatrix}\vec{P}=E_{extrinsic,left}\vec{P}\\
\vec{p}_{r}
=
\begin{bmatrix}
R_{r}|T_{r}
\end{bmatrix}\vec{P}=E_{extrinsic,right}\vec{P}
\end{cases}\tag{4}
$$

$$
\begin{split}
\vec{p}_{r}&=E_{extrinsic,right}E_{extrinsic,left}^{-1}\vec{p}_{l}
&=\begin{bmatrix}
R_{New}|T_{New}
\end{bmatrix}\vec{p}_{l}
\end{split}\tag{5}
$$

![Screenshot from 2018-09-10 16-36-46](./ResearchLog091018_StereoImage/Screenshot from 2018-09-10 16-36-46.png)

Equation (5) can be rewritten as this new equation
$$
\vec{p}_{r}=R_{New}\cdot(\vec{p}_{l}-\vec{T}). \tag{6}
$$
The relationship of $\vec{T}_{New}$ and $\vec{T}$ is that
$$
\vec{T}_{New} = -R\vec{T}.\tag{7}
$$
Now recall the high-school mathematic, a line passing through two points $\vec{x}_{1}$ and $\vec{x}_{2}$ with a normal vector $\vec{n}$ of the plane which the line lies on has the property
$$
(\vec{x}_{1}-\vec{x}_{2})\cdot\vec{n}=0.\tag{8}
$$
$\vec{p}_{l}\times\vec{T}$ can be viewed as the normal vector of the epipolar plane then 
$$
\begin{split}
(\vec{p}_{l}-\vec{T})\cdot(\vec{p}_{l}\times\vec{T})&=(\vec{p}_{l}-\vec{T})^{H}(\vec{p}_{l}\times\vec{T})\\
&=(R_{New}^{-1}\vec{p}_{r})^{H}(\vec{p}_{l}\times\vec{T})\\
&=(R_{New}^{-1}\vec{p}_{r})^{H}\begin{bmatrix}T\end{bmatrix}_{\times}^{H}\vec{p}_{l}\quad\mathit{(cross\,product)}\\
&=(R_{New}^{-1}\vec{p}_{r})^{H}
\begin{bmatrix}
0&t_{3}&-t_{2}\\
-t_{3}&0&t_{1}\\
t_{2}&-t_{1}&0
\end{bmatrix}\vec{p}_{l}\\
&=\vec{p}_{r}^{H}R_{New}^{-H}\begin{bmatrix}T\end{bmatrix}_{\times}^{H}\vec{p}_{l}\\
&=\vec{p}_{r}^{H}E_{essential}\vec{p}_{l}\\
&=0
\end{split}.\tag{9}
$$

$$
det([T]_{\times}^{H})=0 \tag{10}
$$

From the equation (10), we know that $[T]_{\times}^{H}$ is a rank-deficient matrix($[T]_{x}^{H}\in\mathcal{R}_{3},rank([T]_{\times}^{H})=2$). This means that the solution set of equation is a line. 

---

## Fundamental matrix

The essential matrix is the relationship of the two camera coordinates transformation and the fundamental matrix is used to described the transformation between the image planes. This means that we need to consider the intrinsic matrices of two camera. Recall that $\vec{q}$(the object point on the image coordinate) and $\vec{p}$(the object point on the camera coordinate) can be formulated as

$$
\begin{cases}
\vec{q}_{l}= M_{Intrinsic}\vec{p}_{l}\\
\vec{q}_{r}= M_{Intrinsic}\vec{p}_{r}\\
\end{cases}\tag{11}
$$

 or
$$
\begin{cases}
\vec{p}_{l}= M_{Intrinsic}^{-1}\vec{q}_{l}\\
\vec{p}_{r}= M_{Intrinsic}^{-1}\vec{q}_{r}\\
\end{cases}.\tag{12}
$$
Substitute equation (11) to (9) and we get that

$$
\begin{split}
\vec{p}_{r}^{H}E_{essential}\vec{p}_{l}&=(M_{Intrinsic}^{-1}\vec{q}_{r})^{H}E_{essential}(M_{Intrinsic}^{-1}\vec{q}_{l})\\
&=\vec{q}_{r}^{H}M_{Intrinsic}^{-H}E_{essential}M_{Intrinsic}\vec{q}_{l}\\
&=\vec{q}_{r}^{H}F_{fundamental}\vec{q}_{l}\\
&=0
\end{split}
$$

---

## References

+ [Wiki of essential matrix]()
+ [CS231A Course Notes 4: Stereo Systems and Structure from Motion]()

---

## Notice

Need to check the properties of essential matrix and fundamental matrix likes $\mathit{D.O.F}$, ... etc for oral defense. 