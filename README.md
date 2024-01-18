import pygame
import sys

# Initialize Pygame
pygame.init()

# Set up some constants
WIDTH, HEIGHT = 800, 600
BALL_RADIUS = 10
PADDLE_WIDTH, PADDLE_HEIGHT = 15, 80
BALL_SPEED = 2
PADDLE_SPEED = 2
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)

# Set up the ball and paddles
ball = pygame.Rect(WIDTH // 2, HEIGHT // 2, BALL_RADIUS * 2, BALL_RADIUS * 2)
paddle1 = pygame.Rect(0, HEIGHT // 2, PADDLE_WIDTH, PADDLE_HEIGHT)
paddle2 = pygame.Rect(WIDTH - PADDLE_WIDTH, HEIGHT // 2, PADDLE_WIDTH, PADDLE_HEIGHT)

# Set up the game window
screen = pygame.display.set_mode((WIDTH, HEIGHT))

def reset_ball():
    ball.center = (WIDTH // 2, HEIGHT // 2)

def move_paddle():
    keys = pygame.key.get_pressed()
    if keys[pygame.K_w]:
        paddle1.move_ip(0, -PADDLE_SPEED)
    if keys[pygame.K_s]:
        paddle1.move_ip(0, PADDLE_SPEED)
    if keys[pygame.K_UP]:
        paddle2.move_ip(0, -PADDLE_SPEED)
    if keys[pygame.K_DOWN]:
        paddle2.move_ip(0, PADDLE_SPEED)

    if paddle1.top <= 0:
        paddle1.top = 0
    if paddle1.bottom >= HEIGHT:
        paddle1.bottom = HEIGHT
    if paddle2.top <= 0:
        paddle2.top = 0
    if paddle2.bottom >= HEIGHT:
        paddle2.bottom = HEIGHT

def move_ball():
    global ball_speed_x, ball_speed_y

    ball.move_ip(ball_speed_x, ball_speed_y)

    if ball.left <= 0 or ball.colliderect(paddle1):
        ball_speed_x *= -1
    elif ball.right >= WIDTH or ball.colliderect(paddle2):
        ball_speed_x *= -1
    if ball.top <= 0 or ball.bottom >= HEIGHT:
        ball_speed_y *= -1

# Game loop
ball_speed_x = BALL_SPEED
ball_speed_y = BALL_SPEED
while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()

    screen.fill(BLACK)

    move_paddle()
    move_ball()

    pygame.draw.rect(screen, WHITE, paddle1)
    pygame.draw.rect(screen, WHITE, paddle2)
    pygame.draw.ellipse(screen, WHITE, ball)
    pygame.draw.aaline(screen, WHITE, (WIDTH // 2, 0), (WIDTH // 2, HEIGHT))

    pygame.display.flip()
    pygame.time.Clock().tick(60)
