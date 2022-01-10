# IMU gravity canceller on axes rotations

### An algorithm that isolates the gravity vector, given any R3 rotation, for later cancel with the IMU measurements.

``` python

# Libraries used for convenience

import sympy as sym
import numpy as np
from IPython.display import display, Markdown, Latex, HTML
import scipy as sp
import scipy.linalg
import matplotlib.pyplot as plt
from matplotlib import animation
from mpl_toolkits.mplot3d.art3d import Poly3DCollection

# ------------------------------------------------------------

# Axes rotation angles
Rx= np.deg2rad(45)
Ry= np.deg2rad(0)
Rz= np.deg2rad(0)

# Gravity vector
g = sym.Matrix([[0],
                [0],
                [-9.8]])

# Assembling the rotation matrices (the plane rotates, not the vector) 
# therefore, this is not the usual rotation matrix of a vector in R3 (it is equivalent to rotating a vector by an opposite angle in a given axis) 
# e.g., rotating a vector 45º on the x-axis is different from rotating the plane and keeping the vector stationary, that is, rotating the vector -45º 

# x-axis rotation
Mx = sym.Matrix([[1,             0,          0],
                 [0,    np.cos(Rx), np.sin(Rx)],
                 [0, -1*np.sin(Rx), np.cos(Rx)]])
# y-axis rotation
My = sym.Matrix([[np.cos(Ry), 0, -1*np.sin(Ry)],
                 [         0, 1,             0],
                 [np.sin(Ry), 0,    np.cos(Ry)]])
# z-axis rotation
Mz = sym.Matrix([[   np.cos(Rz), np.sin(Rz), 0],
                 [-1*np.sin(Rz), np.cos(Rz), 0],
                 [            0,          0, 1]])

# Return the rotation of vector g in R3 
Mx@My@Mz@g

# Just subtract the resulting vector from the accelerometer readings 
# this will isolate gravity (we will only have the projection of the acceleration vectors on the XY plane)

```
