# Research Log 090318 - Single camera model

---

## Goals: 
The goals of camera calibration are finding the the model of camera's geometry and distortion model of lens. These are called as the intrinsic parameters of camera.

---

## Camera geometry model - Pinhole model

![ResearchLog090318_1](../ResearchLog/ResearchLog090318_SingleCameraModel/ResearchLog090318_1.png)

The simplest model of camera is pinhole model. The model views the focus of  the camera lens as a pinhole on a plane. The ray pass through the hole directly and image on the *image plane*.

+ $\mathit{f}$ : the focal length from pinhole aperture to the screen 

+ $\mathit{Z}$ : the distance from the camera to the object

+ $\mathit{X}$ : the length of the object

+ $\mathit{x}$ : the length of the object's image on image plane 

We can use the similarity of triangular to describe the relationship.
$$
-\mathit{x} = \mathit{f}\frac{\mathit{X}}{\mathit{Z}}
$$

We can swap the positions of image  and the pinhole plane.

![Screenshot from 2018-09-03 15-11-28](/home/chenwei/Documents/ResearchLog/ResearchLog090318_SingleCameraModel/Screenshot from 2018-09-03 15-11-28.png)

The point in the pinhole is called as the center of projection. Every ray will leave from the object and pass through the center of projection. The point at the intersection of the optical axis and the image plane is referred to as the principle point. The distance between the center of the projection is same as the the focal length.

+ $\vec{Q}=(\mathit{X_{Camera}},\mathit{Y_{Camera}},\mathit{Z_{Camera}})^{T}$: some points of the object in the world coordinate 
+ $\vec{\mathit{q}}=(\mathit{x_{Image}},\mathit{y_{Image}},0)^{T}$

Typically, the center of the imager chip(CMOS) isn't usually on  the optical axis. Due to this, we need to add two more parameters $c_{x},c_{y}$ to model these displacement (away from the optical axis) of the center of coordinates on the projection screen.

$$
\begin{cases}
\mathit{x}_{image} = \mathit{f}_{x}\cdot\frac{\mathit{X}_{Camera}}{\mathit{Z}_{Camera}}+\mathit{c}_{\mathit{x}}\\
\mathit{y}_{image} = \mathit{f}_{y}\cdot\frac{\mathit{Y}_{Camera}}{\mathit{Z}_{Camera}}+\mathit{c}_{\mathit{y}}
\end{cases}
$$

For a moder CMOS camera not a tape camera, the focal length should be redefined. 

For example, $f_{\mathit{x}}=\mathit{f}\cdot\mathit{S_{x}}$.

+  $\mathit{f}$  :  the physical focal length (**Unit: mm**)
+ $\mathit{S_{x}}$ : the size of the individual imager elements (**Unit: pixels/mm**)
+ $\mathit{f_{x}}$  : the imager focal length (**Unit: pixels**)

 One should notice that we ==can't== get the real value of $\mathit{S_{x}}$, $\mathit{S_{y}}$, $\mathit{f_{x}}$ and $\mathit{f_{y}}$ by the camera calibration method.



The relationship can be rewritten by the matrix operation==(homogeneous coordinates)==. The homogeneous coordinates associated with a point in a projective space of dimensions==$\mathit{n}$== is common expressed as ==$\mathit{n+1}$== dimensional vector. The vector on the image plane only have two components $\mathit{x},\mathit{y}$  which can be added a new parameter $\mathit{\omega}$ . Finally the vector on the image plane which be described by the homogeneous coordinates become $\vec{\mathit{q}}=(\mathit{x},\mathit{y},\mathit{\omega})^{T}$.
$$
\vec{\mathit{q}} = \mathit{M}\cdot\vec{\mathit{Q}}
$$

+ $$\vec{\mathit{q}} = 
  \begin{bmatrix}
  \mathit{x}_{screen}\\
  \mathit{y}_{screen}\\
  \mathit{\omega}
  \end{bmatrix}$$

+ $$\mathit{M}
  =\begin{bmatrix}
  \mathit{f}_{x}&0&\mathit{c}_{x}\\
  0&\mathit{f}_{y}&\mathit{c}_{y}\\
  0&0&1
  \end{bmatrix}$$ ( This matrix is called as ==intrinsic matrix==)

+ $$\vec{\mathit{Q}} = \begin{bmatrix}
  \mathit{X}_{Camera}\\
  \mathit{Y}_{Camera}\\
  \mathit{Z}_{Camera}
  \end{bmatrix}$$
