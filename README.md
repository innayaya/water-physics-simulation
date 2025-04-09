# water-physics-simulation
import numpy as np
import matplotlib.pyplot as plt
import matplotlib.animation as animation

# Simulation settings
WIDTH = 100  # Grid width
HEIGHT = 50  # Grid height
DAMPING = 0.99  # Damping factor to reduce oscillations

# Initialize wave height grids
current_wave = np.zeros((HEIGHT, WIDTH))
previous_wave = np.zeros((HEIGHT, WIDTH))

# Introduce an initial disturbance
def create_wave():
    cx, cy = WIDTH // 2, HEIGHT // 2  # Center of the grid
    current_wave[cy, cx] = 1  # Initial disturbance

# Update wave propagation
def update_wave():
    global current_wave, previous_wave
    next_wave = np.zeros((HEIGHT, WIDTH))
    for y in range(1, HEIGHT - 1):
        for x in range(1, WIDTH - 1):
            next_wave[y, x] = (
                (current_wave[y - 1, x] + current_wave[y + 1, x] +
                 current_wave[y, x - 1] + current_wave[y, x + 1]) / 2
                - previous_wave[y, x]
            ) * DAMPING
    previous_wave = np.copy(current_wave)
    current_wave[:] = next_wave[:]

# Animation function
def animate(i):
    update_wave()
    ax.clear()
    ax.set_xticks([])
    ax.set_yticks([])
    ax.imshow(current_wave, cmap='Blues', interpolation='bilinear')

# Initialize figure
fig, ax = plt.subplots()
create_wave()
ani = animation.FuncAnimation(fig, animate, frames=100, interval=50)
plt.show()
456
