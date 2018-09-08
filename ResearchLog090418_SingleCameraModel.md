# Research Log 090418: Single camera model

---

## Lens distortions

There are two sources of lens distortions: radial and tangential distortion. The radial distortion is due to the shape of lens which means that vendor manufacture the spherical lens easily than the parabolic lens. The tangential distortion is risen by the assembly of the imager and the lens which is not align.

### Radial distortion

There are two sub-kind distortions of radial distortion. One is the barrel distortion, the other is the pincushion distortion. 

 ![Types of radial distortion](./ResearchLog090418_SingleCameraModel/lens-distortion-graphic.jpg)

The distortion is $0$ at the optical center of the imager for the radial distortion. The effect of distortions will increase along outside the radial direction. The distortion can be modeled by the **Taylor series expansion** around $\mathit{r}\,=\,0$ .
$$
\begin{cases}
\mathit{x}_{Cor} = \mathit{x}_{Ori}\cdot (1+\mathit{k}_{1}\mathit{r}^{2}+\mathit{k}_{2}\mathit{r}^{4}+\mathit{k}_{3}\mathit{r}^{6})\\
\mathit{y}_{Cor} = 
\mathit{y}_{Ori}\cdot(1+\mathit{k}_{1}\mathit{r}^{2}+\mathit{k}_{e}\mathit{r}^{4}+\mathit{k}_{3}\mathit{r}^{6})
\end{cases}\tag{1}
$$

+ $\mathit{x}_{Cor}$ : the correction point $x$ on the screen/monitor/display
+ $\mathit{y}_{Cor}$ : the correction point $y$ on the screen/monitor/display
+ $\mathit{x}_{Ori}$ :  the point projection on the imager
+ $\mathit{y}_{Ori}$ :  the point projection on the imager

With different cameras, the degree of the equation $(1)$ are also different. Typically,

+ Cheap web-cam: degree up to $2_{nd}$
+ Fish-eye lens: degree up to $3_{rd}$

### Tangential distortion

As description as introduction, the tangential distortion arises from the assembly. 

![Assembly of lens and CMOS cheap](./ResearchLog090418_SingleCameraModel/Screenshot from 2018-09-04 11-44-59.png) 

Tangential distortion can be modeled by the equations:
$$
\begin{cases}
\mathit{x}_{Cor} = \mathit{x}_{Ori} + \begin{bmatrix}2\mathit{p}_{1}\mathit{x}\mathit{y}+\mathit{p}_{2}(\mathit{r}^{2}+2\mathit{x}^{2})\end{bmatrix}\\
\mathit{y}_{Cor} = \mathit{y}_{Ori} + \begin{bmatrix}\mathit{p}_{1}(r^{2}+2\mathit{y}^{2})+2\mathit{p}_{2}\mathit{xy}\end{bmatrix}
\end{cases}\tag{2}
$$

### Summary

There are $5$ parameters which are typically bundled into  one distortion vector $5\times1 $

matrix.

---

## Extrinsic matrix

The pose of the camera can be formulated as a ==rotation matrix and translational vector.== 

### Rotation matrix

There we define the rotation angle $\psi,\phi,\,and\,\theta$ about specified axis $\mathit{x},\mathit{y}\,and\,\mathit{z}$ . The result of rotation matrix is the product of these rotation matrices $\mathit{\mathbf{R}_{x}}(\psi)$, $\mathit{\mathbf{R}_{y}}(\phi)$ and  $\mathit{\mathbf{R}_{z}}(\theta)$. 

+ $$
  \mathit{\mathbf{R}_{x}}(\psi) = 
  \begin{bmatrix}
  1&0&0\\
  0&\cos{\psi}&\sin{\psi}\\
  0&-\sin{\psi}&\cos{\psi}
  \end{bmatrix}\tag{3}
  $$

+ $$
  \mathit{\mathbf{R}_{y}}(\phi) = 
  \begin{bmatrix}
  \cos{\phi}&0&-\sin{\phi}\\
  0&1&0\\
  \sin{\phi}&0&\cos{\phi}
  \end{bmatrix}\tag{4}
  $$

+ $$
  \mathit{\mathbf{R}_{z}}(\theta) = 
  \begin{bmatrix}
  \cos{\theta}&\sin{\theta}&0\\
  -\sin{\theta}&\cos{\theta}&0\\
  0&0&1
  \end{bmatrix}\tag{5}
  $$




We define the rotation matrix as $\mathit{\mathbf{R}}= \mathit{\mathbf{R}}_{x}(\psi)\mathit{\mathbf{R}}_{y}(\phi)\mathit{\mathbf{R}}_{z}(\theta)\tag{6}$.

The rotation matrix has such property :
$$
\mathit{\mathbf{R}}^{T}\cdot\mathit{\mathbf{R}} = \mathit{\mathbf{R}}\cdot\mathit{\mathbf{R}}^{T} = \mathit{I}_{3}\tag{7}
$$
where $\mathit{I}_{3}$ is the identity matrix.

### Translation vector

The translation vector represents the shift from one coordinate system center to another system center. 

$$
\vec{\mathit{\mathbf{T}}} = \vec{\mathit{\mathbf{O}}}_{Obj} - \vec{\mathit{\mathbf{O}}}_{Camera}\tag{8}
$$

So a point of the object $\vec{\mathit{\mathbf{P}}}_{Obj}$ on the world coordinate translate to the camera coordinate which is denoted as $\vec{\mathit{\mathbf{P}}}_{Camera}$. 
$$
\vec{\mathit{\mathbf{P}}}_{Camera} =
\mathit{\mathbf{R}}
\cdot 
(\vec{\mathit{\mathbf{P}}}_{Obj}-\vec{\mathit{\mathbf{T}}}) \tag{9}
$$




