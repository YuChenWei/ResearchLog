# Research Log 091218 - Stereo Calibration and Rectification

---
+ Stereo calibration: The process of computing the geometrical relationship between the two cameras in space.
+ Stereo rectification: The process of ==correcting== the individual images so the two images are row-aligned.

---

## Stereo calibration

Stereo calibration depends on finding the rotation matrix $R$ and translation vector $\vec{T}$ between the two cameras.

From the proof of essential matrix we know that 

$$
\begin{cases}
\begin{split}
\vec{p}_{l}&=E_{extrinsic,left}\times \vec{P}\\
			&=\begin{bmatrix}
			R_{l}&\vec{T_{l}}\\
			0&1
			\end{bmatrix}\times \vec{P}
\end{split}\\
\begin{split}
\vec{p}_{r}&=E_{extrinsic,right}\times \vec{P}\\
			&=\begin{bmatrix}
			R_{r}&\vec{T_{r}}\\
			0&1
			\end{bmatrix}\times \vec{P}
\end{split}
\end{cases}\tag{1}
$$



$$
\begin{split}
\vec{p}_{r}&=E_{extrinsic,right}E_{extrinsic,right}^{-1}\vec{p}_{l}\\
&=\begin{bmatrix}
			R_{r}&\vec{T_{r}}\\
			0&1
  \end{bmatrix}\times
  \begin{bmatrix}
			R_{l}&\vec{T_{l}}\\
			0&1
  \end{bmatrix}^{-1}\vec{p}_{l}\\
&=\begin{bmatrix}
			R_{r}&\vec{T_{r}}\\
			0&1
\end{bmatrix}\times
\begin{bmatrix}
    R_{l}^{-1}&-R_{l}^{-1}\vec{T_{l}}\\
    0&1
\end{bmatrix}\vec{p}_{l}\qquad(Inverse\,of\,affine\,transform)\\
&=\begin{bmatrix}
R_{r}R_{l}^{-1}&\vec{T}_{r}-R_{r}R_{l}^{-1}\vec{T}_{l}\\
0&1
\end{bmatrix}\vec{p}_{l}\\
&=\begin{bmatrix}
R_{New}&\vec{T}_{New}\\
0&1
\end{bmatrix}\vec{p}_{l}
\end{split}\tag{2}
$$
where
$$
\begin{cases}
R_{New} = R_{r}R_{l}^{-1}\\
\begin{split}
\vec{T}_{New} &= \vec{T}_{r}-R_{r}R_{l}^{-1}\vec{T}_{l}\\
			&= \vec{T}_{r}-R_{New}\vec{T}_{l}
\end{split}
\end{cases}\tag{3}
$$
One can use the ==Levenberg-Marquardt iterative algorithm== to find the solution of $R_{New}$ and $\vec{T}_{New}$.

---

## Stereo rectification 

The goal of stereo rectification is that we re-project the image planes of two cameras so image rows are perfectly aligned. The row aligned images of two cameras will make that the **[stereo correspondence/matching](https://en.wikipedia.org/wiki/Correspondence_problem)** be more reliable. 

There are common ways to rectify our image pairs.

+ Hartley's algorithm 
  + Using the fundamental matrix of a pair of two uncalibrated stereo cameras to rectify. 
  + The algorithm is suitable for the on-line "Structure from motion" but the result have lots of distortion.
  + It will loss the scale information of objects.
+ Bouguet's algorithm
  + Using the rotation matrix and translation vector of two calibrated stereo cameras to rectify. 

---

### Uncalibrated stereo images pair rectification - Hartley's algorithm

Hartley's method attempts to find homographies that map the epipoles to infinity and minimize the disparities between the two stereo images - it does this by matching points between two image pairs. We don't need to calculate the intrinsic matrices of two cameras due to this informations are implicitly contained in the fundamental matrices.

---

### Calibrated stereo images pair rectification - Bouguet's algorithm

From the equation (3), we have the $R_{New}$ and $\vec{T}_{New}$ between the two camera. We can use these to rectify. Bouguet's approach is to minimize the amount of change reprojection produces for each of the two images(minize the resulting reprojection distions) and maximizing common viewing area. Bouguet's algorithm doesn't be addressed but the Matlab toolbox apply it to implement stereo rectify.  $R_{New}$ means that the rotation from left camera coordinate to right camera coordinate. It splits $R_{New}$ to two half parts. We call these new rotation matrices as $r_{r}$  and $r_{l}$ for the left and right cameras. So their principal rays will become parallel to the vector sum of their original principal ray vector. ==Draw a picture my self.== After these rotations the image plane of two cameras are coplanar. 



---

## References

+ [In Defense of the Eight-Point Algorithm *Richard I. Hartley*](https://www.cse.unr.edu/~bebis/CS485/Handouts/hartley.pdf)
+ [Revisiting Hartley's Normalized Eight-Point Algorithm *Wojciech Chojnacki, Michael J. Brooks, Anton van den Hengel, and Darren Gawley*](https://cs.adelaide.edu.au/~wojtek/papers/pami-nals2.pdf)
+ [Eight-point algorithm](https://en.wikipedia.org/wiki/Eight-point_algorithm)
+ [VFX Class note](https://www.csie.ntu.edu.tw/~cyy/courses/vfx/05spring/lectures/scribe/08scribe2.pdf)
+ [Visual Odometry Part 1: The First 30 Years and Fundamentals](https://www.ifi.uzh.ch/dam/jcr:5759a719-55db-4930-8051-4cc534f812b1/VO_Part_I_Scaramuzza.pdf)
+ [Image rectification](https://en.wikipedia.org/wiki/Image_rectification)
+ [Theory and Practice of Projective Rectification *Richard I. Hartley*](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.6.1107&rep=rep1&type=pdf)
+ [Bouguet's algorithm](http://www.vision.caltech.edu/bouguetj/calib_doc/)