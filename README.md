import turtle
import time
import pygame
import os

# Initialize Pygame
pygame.mixer.init()

# Load the sound files
hit_sound = pygame.mixer.Sound("tennis.mp3")
score_sound = pygame.mixer.Sound("scored.mp3")

# creating a Screen for the game with name
wn = turtle.Screen()
wn.title("Pong by @BerserkAhmet")
wn.bgcolor("black")
wn.setup(width=800, height=600)
wn.tracer(0)

# score of the paddles
score_a = 0
score_b = 0

# Paddle A
paddle_a = turtle.Turtle()
paddle_a.speed(0)
paddle_a.shape("square")
# ozgerted figurani
paddle_a.shapesize(stretch_wid=5, stretch_len=1)
paddle_a.color("white")
paddle_a.penup()
paddle_a.goto(-350, 0)

# Paddle B
paddle_b = turtle.Turtle()
paddle_b.speed(0)
paddle_b.shape("square")
# ozgerted figurani
paddle_b.shapesize(stretch_wid=5, stretch_len=1)
paddle_b.color("white")
paddle_b.penup()
paddle_b.goto(350, 0)

# Ball
ball = turtle.Turtle()
ball.speed(0.1)
ball.shape("square")
ball.color("white")
ball.penup()
ball.goto(0, 0)
ball.dx = 2.5  # Decrease the speed in the x-direction
ball.dy = -2.5  # Decrease the speed in the y-direction

# Pen
pen = turtle.Turtle()
pen.speed(0)
pen.color("white")
pen.penup()
pen.hideturtle()
pen.goto(0, 260)


pen.write("Player A: 0  Player B: 0".format(name1="Player A",name2="Player B"), align='center', font=("Courier", 24, "normal"))

# functions for paddle_a moving
def paddle_a_up():
    y = paddle_a.ycor()
    y += 40
    paddle_a.sety(y)

def paddle_a_down():
    y = paddle_a.ycor()
    y -= 40
    paddle_a.sety(y)

# keyboard setup for paddle_a
wn.listen()
wn.onkeypress(paddle_a_up, 'w')
wn.onkeypress(paddle_a_down, 's')

# functions for moving of paddle_b
def paddle_b_up():
    y = paddle_b.ycor()
    y += 40
    paddle_b.sety(y)

def paddle_b_down():
    y = paddle_b.ycor()
    y -= 40
    paddle_b.sety(y)

# keyboard setup for paddle_b
wn.listen()
wn.onkeypress(paddle_b_up, 'Up')
wn.onkeypress(paddle_b_down, 'Down')

# main game loop
while True:
    wn.update()

    # ball moving
    ball.setx(ball.xcor() + ball.dx)
    ball.sety(ball.ycor() + ball.dy)

    # border checking
    if ball.ycor() > 290 or ball.ycor() < -290:
        ball.dy *= -1
        hit_sound.play()

    if ball.xcor() > 390:  # Check the right border
        ball.goto(0, 0)
        ball.dx *= -1
        score_a += 1
        pen.clear()
        pen.write("Player A: {}  Player B: {}".format(score_a, score_b), align='center',font=("Courier", 24, "normal"))
        score_sound.play()

    if ball.xcor() < -390:  # Check the left border
        ball.goto(0, 0)
        ball.dx *= -1
        score_b += 1
        pen.clear()
        pen.write("Player A: {}  Player B: {}".format(score_a, score_b,), align='center',font=("Courier", 24, "normal"))
        score_sound.play()

    # paddle and ball collisions
    if (ball.xcor() > 340 and ball.xcor() < 350) and (ball.ycor() < paddle_b.ycor() + 40 and ball.ycor() > paddle_b.ycor() - 40):
        ball.setx(340)
        ball.dx *= -1

    if (ball.xcor() < -340 and ball.xcor() > -350) and (ball.ycor() < paddle_a.ycor() + 50 and ball.ycor() > paddle_a.ycor() - 50):
        ball.setx(-340)
        ball.dx *= -1

    # Introduce a small delay to stabilize the loop
    time.sleep(0.01)
