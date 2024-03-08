# PHYS 231 group research project Winter 2024
# Group: Henry Macrae-Sadek, Omar Sierra AJ Frazier, Jake Schaefer
# Majority of code taken from last years project Black-EtAl
# Lorenz System:
# 𝑥˙=𝑠(𝑦−𝑥) 
# 𝑦˙=𝑥(𝑟−𝑧)−𝑦 
# 𝑧˙=𝑥𝑦−𝑏𝑧 

import numpy as np
import matplotlib.pyplot as plt
import math


r= 28
b=8/3
def lorenz_system(x, y, z, r, b, s):
    x_dot = s * (y - x)
    y_dot = r * x - y - x * z
    z_dot = x * y - b * z
    return x_dot, y_dot, z_dot


ds = 0.1  # parameter step size
s = np.arange(5, 15, ds)  # parameter range
dt = 0.001  # time step
t = np.arange(0, 10, dt)  # time range

# initialize solution arrays
xs = np.empty(len(t) + 1)
ys = np.empty(len(t) + 1)
zs = np.empty(len(t) + 1)


# initial values x0,y0,z0 for the system
xs[0], ys[0], zs[0] = (1, 1, 1)

svalues=[]

for S in s:
  svalues.append(S)

  for i in range(len(t)):
        # approximate numerical solutions to system
        x_dot, y_dot, z_dot = lorenz_system(xs[i], ys[i], zs[i], S)
        xs[i + 1] = xs[i] + (x_dot * dt)
        ys[i + 1] = ys[i] + (y_dot * dt)
        zs[i + 1] = zs[i] + (z_dot * dt)
        #cataloging x,y,z values
        
        # calculate and save the peak values of the z solution
        
    # "use final values from one run as initial conditions for the next to stay near the attractor"
xs[0], ys[0], zs[0] = xs[i], ys[i], zs[i]

xarray=np.array(xs)
yarray=np.array(ys)
zarray=np.array(zs)

xdist=[]
ydist=[]
zdist=[]

for j in xarray:
    xdist.append( 1 / np.exp(xarray - i).sum() )
    ydist.append( 1 / np.exp(yarray - i).sum() )
    zdist.append( 1 / np.exp(zarray - i).sum() )
    
    
xdist = np.array(xdist)
ydist = np.array(ydist)
zdist = np.array(zdist)


print(xdist)




    

