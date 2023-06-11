
import pygame
import random

# Initialize Pygame
pygame.init()

# Set up the game window
window_width = 800
window_height = 600
window = pygame.display.set_mode((window_width, window_height))
pygame.display.set_caption("Car Game")

# Set up the colors
black = (0, 0, 0)
white = (255, 255, 255)
red = (255, 0, 0)

# Set up the car
car_width = 73
car_height = 82
car_img = pygame.image.load("car.png")
car_img = pygame.transform.scale(car_img, (car_width, car_height))
car_x = (window_width - car_width) // 2
car_y = window_height - car_height - 10
car_speed = 5

# Set up the obstacles
obstacle_width = 100
obstacle_height = 100
obstacle_img = pygame.Surface((obstacle_width, obstacle_height))
obstacle_img.fill(red)
obstacle_x = random.randint(0, window_width - obstacle_width)
obstacle_y = -obstacle_height
obstacle_speed = 3

clock = pygame.time.Clock()

game_over = False

def car(x, y):
    window.blit(car_img, (x, y))

def obstacle(x, y):
    window.blit(obstacle_img, (x, y))

def collision_detection(car_x, car_y, obstacle_x, obstacle_y):
    if car_x + car_width > obstacle_x and car_x < obstacle_x + obstacle_width and car_y + car_height > obstacle_y and car_y < obstacle_y + obstacle_height:
        return True
    return False

while not game_over:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            game_over = True

    keys = pygame.key.get_pressed()
    if keys[pygame.K_LEFT] and car_x > 0:
        car_x -= car_speed
    if keys[pygame.K_RIGHT] and car_x < window_width - car_width:
        car_x += car_speed

    window.fill(black)

    obstacle_y += obstacle_speed
    if obstacle_y > window_height:
        obstacle_x = random.randint(0, window_width - obstacle_width)
        obstacle_y = -obstacle_height

    if collision_detection(car_x, car_y, obstacle_x, obstacle_y):
        game_over = True

    car(car_x, car_y)
    obstacle(obstacle_x, obstacle_y)

    pygame.display.update()
    clock.tick(60)

pygame.quit()

