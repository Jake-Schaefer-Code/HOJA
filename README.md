# Group: Henry Macrae-Sadek, Omar Sierra AJ Frazier, Jake Schaefer
# Majority of code taken from last years project Black-EtAl
# Lorenz System:
# 𝑥˙=𝑠(𝑦−𝑥) 
# 𝑦˙=𝑥(𝑟−𝑧)−𝑦 
# 𝑧˙=𝑥𝑦−𝑏𝑧


import numpy as np
import matplotlib.pyplot as plt
from scipy.stats import entropy

def lorenz_system(x, y, z, s, r=28, b=8/3):
    x_dot = s * (y - x)
    y_dot = x * (r - z) - y
    z_dot = x * y - b * z
    return x_dot, y_dot, z_dot

ds = 0.1  # parameter step size
s_values = np.arange(10, 15, ds)  # Prandtl number range
dt = 0.01  # time step
t = np.arange(0, 2, dt)  # time range

entropies_x = []
entropies_y = []
entropies_z = []

for S in s_values:
    xs, ys, zs = [1.0], [1.0], [1.0]  # Initial conditions

    for i in range(len(t) - 1):
        x_dot, y_dot, z_dot = lorenz_system(xs[-1], ys[-1], zs[-1], S)
        xs.append(xs[-1] + x_dot * dt)
        ys.append(ys[-1] + y_dot * dt)
        zs.append(zs[-1] + z_dot * dt)

    plt.figure(figsize=(12, 4))
    plt.subplot(131)
    plt.plot(xs, ys)
    plt.title(f'Lorenz Attractor for s={S:.1f}\nX-Y Plane')
    plt.xlabel('X')
    plt.ylabel('Y')

    plt.subplot(132)
    plt.plot(xs, zs)
    plt.title('X-Z Plane')
    plt.xlabel('X')
    plt.ylabel('Z')

    plt.subplot(133)
    plt.plot(ys, zs)
    plt.title('Y-Z Plane')
    plt.xlabel('Y')
    plt.ylabel('Z')

    plt.tight_layout()
    plt.show()

    # Compute histograms as probability distributions for Shannon entropy
    prob_x, _ = np.histogram(xs, bins=30, density=True)
    prob_y, _ = np.histogram(ys, bins=30, density=True)
    prob_z, _ = np.histogram(zs, bins=30, density=True)

    # Compute Shannon entropy
    entropy_x = entropy(prob_x, base=2)
    entropy_y = entropy(prob_y, base=2)
    entropy_z = entropy(prob_z, base=2)

    entropies_x.append(entropy_x)
    entropies_y.append(entropy_y)
    entropies_z.append(entropy_z)

plt.figure(figsize=(12, 4))
plt.subplot(131)
plt.scatter(s_values, entropies_x)
plt.title('Shannon Entropy of X')
plt.xlabel('s')
plt.ylabel('Entropy')

plt.subplot(132)
plt.scatter(s_values, entropies_y)
plt.title('Shannon Entropy of Y')
plt.xlabel('s')

plt.subplot(133)
plt.scatter(s_values, entropies_z)
plt.title('Shannon Entropy of Z')
plt.xlabel('s')

plt.tight_layout()
plt.show()
