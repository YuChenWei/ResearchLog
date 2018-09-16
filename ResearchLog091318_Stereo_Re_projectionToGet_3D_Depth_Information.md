# ResearchLog 091318 - Stereo re-projection to get 3D depth information 

# Re-projection matrix $Q$

Review that the triangulation, we know that

$$
\begin{cases}
	Z = \frac{Tf}{d}\\
	X = \frac{u-c_{x}}{f_{x}}\\
	Y = \frac{v-c_{y}}{f_{y}}
\end{cases}.\tag{1}
$$

So we can define a re-projection matrix $Q$ as

$$
Q=\begin{bmatrix}
1&0&0&-c_{x}\\
0&1&0&-c_{y}\\
0&0&0&f\\
0&0&\frac{-1}{T}&\frac{c_{x}-c_{x}'}{T}
\end{bmatrix}\tag{2}
$$

If the principal rays at infinity, then $c_{x}=c_{x}'$ and the term in the lower-right corner is $0$. A point on the left image is $(x,y,d,1)^{T}$ can be applied the $Q$ matrix to get the 3D location information

$$
Q\cdot
\begin{bmatrix}
x\\y\\d\\1
\end{bmatrix}=
\begin{bmatrix}
X\\Y\\Z\\W
\end{bmatrix}\tag{3}
$$

The three-dimensional coordinates are $(\frac{X}{W},\frac{Y}{W},\frac{Z}{W})^{T}​$.