# Transformations

There are three types of transformations we can apply to a shape:
* Rigid body
  * Preserves distance, angles, and orientation
  * E.g., Translation and Rotation
* Conformal
  * Preserves angles and orientations
  * E.g., Translation, Rotation, and Uniform Scaling
* Affine
  * Preserves parallelism. Lines remain lines.
  * Translation, Rotation, Scaling, Shear, and Reflection

## Affine Transformations
Affine transformations are described as an operation

\[F(p)=Ap+t\]

where A is a linear transformation, t is a translation vector

* Translations are described by the translation vector t
* Rotations counterclockwise by $\theta$ can be described by rotation matrices
  around a given axis
  * Rotation about the x axis
    \[
    \begin{bmatrix}
    1 & 0            & 0\\
    0 & \cos(\theta) & -\sin(\theta)\\
    0 & \sin(\theta) & \cos(\theta)
    \end{bmatrix}
    \]
  * To rotate about the Y axis:
    \[
    \begin{bmatrix}
    \cos(\theta)  & 0 & \sin(\theta)\\
    0             & 1 & 0\\
    -\sin(\theta) & 0 & \cos(\theta)
    \end{bmatrix}
    \]
  * To rotate about the Z axis:
    \[
    \begin{bmatrix}
    \cos(\theta) & -\sin(\theta) & 0\\
    \sin(\theta) & \cos(\theta)  & 0\\
    0            & 0             & 1
    \end{bmatrix}
    \]
* Scaling by scalars $a,b,c$ is performed with A as described below. To get
  uniform scaling, set $a=b=c$
  \[
  \begin{bmatrix}
  a & 0 & 0\\
  0 & b & 0\\
  0 & 0 & c
  \end{bmatrix}
  \]
* Shearing by $h_x, h_y, h_z$ is described with shearing matrices:
  * Shearing about the x axis
    \[
    \begin{bmatrix}
    1   & 0 & 0\\
    h_y & 1 & 0\\
    h_z & 0 & 1
    \]
  * Shearing about the y axis
    \[
    \begin{bmatrix}
    1 & h_x & 0\\
    0 & 1   & 0\\
    0 & h_z & 1
    \]
  * Shearing about the z axis
    \[
    \begin{bmatrix}
    1 & 0 & h_x\\
    0 & 1 & h_y\\
    0 & 0 & 1
    \]
* Reflection about the y axis:
  \[
  \begin{bmatrix}
  -1 & 0 & 0\\
  0  & 1 & 0\\
  0  & 0 & -1
  \end{bmatrix}
  \]

An affine transformation $q=F(p)=Ap+t$ also satisfies the following:
* Inverse of an affine transformation is also affine, assuming it exists
  * This means that under an affine transformation, there is an inverse
    transformation that will undo the original transformation
* Lines and parallelism are preserved under affine transformation
* Given a closed region, the area under an affine transformation is scaled by
  $det(A)$
* A composition of affine transformations is still affine

## Homogeneous Coordinates
Something important to note, is that the order of transformations matter. We
cannot guarantee that a rotate then scale transformation, would be equal to a
scale then rotate transformation.

Suppose we want to chain multiple affine transformations $A_1,A_2,A_3$
together. First, we apply A1, then A2, then A3.

\[A_3(A_2(A_1*p + t_1) + t_2) + t_3\]

Since the ordering of transformations is important, we would have to store each
transformation matrix and each translation vector. The transformation process
becomes convoluted!

Homogeneous coordinates simplify this by encoding both transformation and
translation into one (D+1)x(D+1) matrix.

\[
\begin{bmatrix}
A        & t\\
0 \ldots & 1
\end{bmatrix}
\]

To tranform a point, we augment a point $p$ with a 1:
\[
\begin{bmatrix}
p_x\\
p_y\\
\vdots\\
1
\end{bmatrix} = 
\begin{bmatrix}
p_x'\\
p_y'\\
\vdots\\
p_w
\end{bmatrix}
\]

We can recover the original point by dividing each component by $p_w$ and then
tossing out the last element. Note that there are now many ways to express a
point in homogeneous coordinates. All points

\[
\begin{bmatrix}
\alpha p\\
\alpha
\]

with $\alpha \not = 0$ express the same homogeneous point.

Now, homogeneous transformations can be expressed as:
\[A_3*A_2*A_1*p\]

Also, we can calculate the matrix $A_4=A_3*A_2*A_1$, which encodes all three
transformations into one matrix!

Note however, that geometric operations can't be applied to homogeneous
coordinates in the same way as in cartesian coordinates.

For example:
* In cartesian coordinates: $(1,1) + (1,1) = (2,2)$
* In homogeneous coordinates: $(1,1,1) + (1,1,1) = (2,2,2) = (1,1,1) = (1,1)$
* We can solve this problem when adding two homogeneous coordinates by setting
  the w component of either vector to be 0 first

## Hierarchical Transforms


