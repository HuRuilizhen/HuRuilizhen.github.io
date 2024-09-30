---
layout: post
title: "Mathematical Concepts and Spatial Transformation"
description: "A brief summary of the mathematical concepts and spatial transformation in robotics."
date: 2024-09-25
feature_image: images/robotics.jpg
tags: ['robotics', 'mathematics']
---

## Representation of Position

In robotics, the representation of the position of a robot is a fundamental problem. There are different ways to represent the position of a robot, including coordinate frames, rotation matrices, and quaternions.

<!--more-->

---

### Coordinate Frame

A coordinate frame is a set of three mutually perpendicular lines that intersect at a single point. The coordinate frame is used to define the position and orientation of a robot in 3D space.

The coordinate frame can be represented by a set of three orthonormal vectors $\hat{x}$, $\hat{y}$, and $\hat{z}$, which are mutually perpendicular and of unit length. The position of a point $p$ in the coordinate frame can be represented by a vector $\overrightarrow{p}$, which can be written as:

$$\overrightarrow{p}=x\hat{x}+y\hat{y}+z\hat{z}$$

where $x$, $y$, and $z$ are the coordinates of the point $p$ in the coordinate frame.

### Cartesian Coordinate Frame

A Cartesian coordinate frame is a coordinate frame that uses three mutually perpendicular lines to define the position and orientation of a robot in 3D space. The three lines are labeled as $x$, $y$, and $z$.

The Cartesian coordinate frame is the most commonly used coordinate frame in robotics. The position of a point $p$ in the Cartesian coordinate frame can be represented by a vector $\overrightarrow{p}$, which can be written as:

$$\overrightarrow{p}=x\hat{i}+y\hat{j}+z\hat{k}$$

where $\hat{i}$, $\hat{j}$, and $\hat{k}$ are the unit vectors of the $x$, $y$, and $z$ axes, respectively.

### Reference Frame and Mobile Frame

**Reference Frame**:

- Definition: A reference frame is a fixed coordinate system used as a standard for all measurements and calculations. It is considered stationary, and all other coordinate systems are defined relative to it.

- Usage: In fields such as robotics, computer graphics, and physics, the reference frame provides a consistent basis for describing the position and orientation of objects. For example, in robotics, the **world coordinate system** is a common reference frame.

**Mobile Frame**:

- Definition: A mobile frame is a coordinate system attached to a moving or rotating object. As the object moves and rotates, this coordinate system moves and rotates with it.

- Usage: The mobile frame allows us to describe the position and orientation of an object relative to itself. This is useful for understanding how the object is moving in its own context. For instance, in a robotic arm, a mobile frame can be defined at the end effector to describe its orientation and position relative to the base of the arm.


---

### Coordinates

Coordinates are a set of numbers that define the position of a point in 3D space. The coordinates of a point can be represented in different ways, including Cartesian coordinates, cylindrical coordinates, and spherical coordinates.

In the Cartesian coordinate system, the coordinates of a point $p$ can be represented by a vector $\overrightarrow{p}$, which can be written as:

$$\overrightarrow{p}=x\hat{i}+y\hat{j}+z\hat{k}$$

In the cylindrical coordinate system, the coordinates of a point $p$ can be represented by a vector $\overrightarrow{p}$, which can be written as:

$$\overrightarrow{p}=r\hat{\rho}+\theta\hat{\theta}+z\hat{k}$$

where $r$ is the radial distance from the origin, $\theta$ is the angular displacement from the x-axis, and $z$ is the height of the point above the xy-plane.

In the spherical coordinate system, the coordinates of a point $p$ can be represented by a vector $\overrightarrow{p}$, which can be written as:

$$\overrightarrow{p}=r\hat{\rho}+\theta\hat{\theta}+\phi\hat{\phi}$$

where $r$ is the radial distance from the origin, $\theta$ is the angular displacement from the x-axis, and $\phi$ is the angular displacement from the z-axis.

---

### Rotation Matrix

A rotation matrix is a square matrix that defines the rotation of a coordinate frame. The rotation matrix is used to transform the coordinates of a point from one coordinate frame to another.

The rotation matrix can be represented by a 3x3 matrix $\mathbf{R}$, which can be written as:

$$
    \mathbf{R}=
    \begin{bmatrix}
    r_{1,1} & r_{1,2} & r_{1,3} \\
    r_{2,1} & r_{2,2} & r_{2,3} \\
    r_{3,1} & r_{3,2} & r_{3,3}
    \end{bmatrix}
$$

The rotation matrix can be used to transform the coordinates of a point $p$ from one coordinate frame to another. The transformed coordinates can be represented by a vector $\overrightarrow{p'}$, which can be written as:

$$\overrightarrow{p'}=\mathbf{R}\overrightarrow{p}$$

Properties of the rotation matrix:

- **Orthogonality**: A rotation matrix is an orthogonal matrix, meaning its row and column vectors are unit vectors and are mutually orthogonal:

$$
    \mathbf{R}^T\mathbf{R}=\mathbf{I}
$$

$$
    \mathbf{R}^{-1}\mathbf{R}=\mathbf{I}
$$

$$
    \mathbf{R}^{T}=\mathbf{R}^{-1}
$$

- **Determinant of 1**: This ensures that the rotation does not change the volume of the object being rotated:

$$
    \det(\mathbf{R})=1
$$

- **Composition of Rotations**: The product of two rotation matrices is also a rotation matrix. Set $\mathbf{R}^1$ and $\mathbf{R}^2$ as rotation matrices, then the product of $\mathbf{R}^1$ and $\mathbf{R}^2$ is a rotation matrix $\mathbf{R}$:

$$
    \mathbf{R}=\mathbf{R}^1\mathbf{R}^2
$$

- **Rotation About an Axis**: In three-dimensional space, any rotation can be viewed as a rotation about some axis. For any given rotation matrix, it is possible to determine the corresponding axis and angle of rotation.

#### Examples

Pure rotation about x-axis:

$$
    \mathbf{R}_x(\theta)=\begin{bmatrix} 1 & 0 & 0 \\ 0 & \cos(\theta) & -\sin(\theta) \\ 0 & \sin(\theta) & \cos(\theta) \end{bmatrix}
$$

Pure rotation about y-axis:

$$
    \mathbf{R}_y(\theta)=\begin{bmatrix} \cos(\theta) & 0 & \sin(\theta) \\ 0 & 1 & 0 \\ -\sin(\theta) & 0 & \cos(\theta) \end{bmatrix}
$$

Pure rotation about z-axis:

$$
    \mathbf{R}_z(\theta)=\begin{bmatrix} \cos(\theta) & -\sin(\theta) & 0 \\ \sin(\theta) & \cos(\theta) & 0 \\ 0 & 0 & 1 \end{bmatrix}
$$

Pure rotation about x-axis of a mobile frame followed a pure rotation about z-axis of the reference frame:

$$
    \mathbf{B} = \mathbf{R}_z (\alpha) \mathbf{R}_x (\beta)
$$

---

### Coordinate Transformation

Coordinate transformation is the process of transforming the coordinates of a point from one coordinate frame to another. There are different ways to perform coordinate transformation, including rotation matrices, homogeneous matrices, and quaternions.

The coordinate transformation can be represented by a 4x4 matrix $\mathbf{T}$, which can be written as:

$$
\mathbf{T}=\begin{bmatrix} \mathbf{R} & \overrightarrow{t} \\ \overrightarrow{0} & 1 \end{bmatrix}
$$

where $\mathbf{R}$ is the rotation matrix, and $\overrightarrow{t}$ is the translation vector. 

The inverse of the coordinate transformation can be obtained by using the inverse of the homogeneous matrix \(\mathbf{T}\):

$$
\mathbf{T}^{-1}=\begin{bmatrix} \mathbf{R}^{T} & -\mathbf{R}^{T}\overrightarrow{t} \\ \overrightarrow{0} & 1 \end{bmatrix}
$$

The transformed coordinates can be represented by a vector \(\overrightarrow{p'}\), which can be written as:

$$
\overrightarrow{p'}=\mathbf{T}\overrightarrow{p}
$$

The coordinate transformation can also be represented by a quaternion $\mathbf{q}$, which can be written as:

$$
\mathbf{q}=w+x\hat{i}+y\hat{j}+z\hat{k}
$$

where $w$, $x$, $y$, and $z$ are the components of the quaternion. 

The transformed coordinates can be represented by a vector $\overrightarrow{p'}$, which can be written as:

$$
\overrightarrow{p'}=\mathbf{q}\otimes\overrightarrow{p}\otimes\mathbf{q}^{-1} 
$$

where $\otimes$ is the quaternion multiplication operator.

---