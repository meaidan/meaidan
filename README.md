import pygame
import sys
import random

# Initialize Pygame
pygame.init()

# Constants
WIDTH, HEIGHT = 800, 600
FPS = 60
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
GIRL_SPEED = 5
OBSTACLE_SPEED = 7

# Create the game window
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Girl Running Through a Shopping Mall")

# Load images
girl_image = pygame.image.load("girl.png")  # Replace "girl.png" with the actual image file
obstacle_image = pygame.image.load("obstacle.png")  # Replace "obstacle.png" with the actual image file

# Set up the game clock
clock = pygame.time.Clock()

# Girl's initial position
girl_x = WIDTH // 2
girl_y = HEIGHT - 100

# List to store obstacles
obstacles = []

# Function to draw the girl
def draw_girl(x, y):
    screen.blit(girl_image, (x, y))

# Function to draw an obstacle
def draw_obstacle(x, y):
    screen.blit(obstacle_image, (x, y))

# Game loop
while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()

    # Move the girl with arrow keys
    keys = pygame.key.get_pressed()
    if keys[pygame.K_LEFT] and girl_x > 0:
        girl_x -= GIRL_SPEED
    if keys[pygame.K_RIGHT] and girl_x < WIDTH - 50:
        girl_x += GIRL_SPEED

    # Create new obstacles randomly
    if random.randint(0, 100) < 5:
        obstacle_x = random.randint(0, WIDTH - 50)
        obstacle_y = -50
        obstacles.append([obstacle_x, obstacle_y])

    # Move and draw obstacles
    for obstacle in obstacles:
        obstacle[1] += OBSTACLE_SPEED
        draw_obstacle(obstacle[0], obstacle[1])

    # Check for collisions with obstacles
    for obstacle in obstacles:
        if (
            girl_x < obstacle[0] < girl_x + 50
            and girl_y < obstacle[1] < girl_y + 50
        ):
            print("Game Over!")
            pygame.quit()
            sys.exit()

    # Remove obstacles that are off-screen
    obstacles = [obstacle for obstacle in obstacles if obstacle[1] < HEIGHT]

    # Draw the girl
    draw_girl(girl_x, girl_y)

    # Update the display
    pygame.display.flip()

    # Set the frame rate
    clock.tick(FPS)

    # Fill the background
    screen.fill(WHITE)
