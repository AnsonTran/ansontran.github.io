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

Translations are performed by adding a translation vector $t$ to a set of
points

Rotations counterclockwise by $\theta$ are done by multiplying each point by a
rotation matrix
* To rotate about the X axis
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
