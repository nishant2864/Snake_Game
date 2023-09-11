# CodeAlpha_Snake_Game
# Creating a simple Snake game using Python and the Turtle module is a great project for beginners. The Turtle module provides a simple way to create graphics and is perfect for developing games with a graphical user interface. 

import turtle
import time
import random

# Set up the screen
screen = turtle.Screen()
screen.title("Snake Game")
screen.setup(width=600, height=600)
screen.bgcolor("black")
screen.tracer(0)

# Initialize score
score = 0

# Snake and food
snake = [turtle.Turtle()]
snake[0].shape("square")
snake[0].color("lime green")
snake[0].penup()
snake[0].goto(0, 0)

food = turtle.Turtle()
food.shape("circle")
food.color("red")
food.penup()
food.goto(random.randint(-290, 290), random.randint(-290, 290))

# Score display
score_display = turtle.Turtle()
score_display.speed(0)
score_display.color("white")
score_display.penup()
score_display.hideturtle()
score_display.goto(0, 260)
score_display.write("Score: 0", align="center", font=("Courier", 24, "normal"))

# Functions for snake movement
def go_up():
    if snake[0].heading() != 270:
        snake[0].setheading(90)

def go_down():
    if snake[0].heading() != 90:
        snake[0].setheading(270)

def go_left():
    if snake[0].heading() != 0:
        snake[0].setheading(180)

def go_right():
    if snake[0].heading() != 180:
        snake[0].setheading(0)

def is_collision():
    # Check if snake collides with the screen boundaries
    if (
        snake[0].xcor() > 290
        or snake[0].xcor() < -290
        or snake[0].ycor() > 290
        or snake[0].ycor() < -290
    ):
        return True

    # Check if snake collides with itself
    for segment in snake[1:]:
        if snake[0].distance(segment) < 20:
            return True

    return False

# Keyboard bindings
screen.listen()
screen.onkeypress(go_up, "Up")
screen.onkeypress(go_down, "Down")
screen.onkeypress(go_left, "Left")
screen.onkeypress(go_right, "Right")

# Update the score display
def update_score():
    score_display.clear()
    score_display.write(f"Score: {score}", align="center", font=("Courier", 24, "normal"))

# Main game loop
delay = 0.1
while True:
    screen.update()

    # Move the snake
    for i in range(len(snake) - 1, 0, -1):
        x = snake[i - 1].xcor()
        y = snake[i - 1].ycor()
        snake[i].goto(x, y)

    snake[0].forward(20)

    # Check for collision
    if is_collision():
        break

    # Update food position if eaten and increase the score
    if snake[0].distance(food) < 20:
        food.goto(random.randint(-290, 290), random.randint(-290, 290))
        new_segment = turtle.Turtle()
        new_segment.shape("square")
        new_segment.color("lime green")
        new_segment.penup()
        snake.append(new_segment)
        score += 10
        update_score()

    time.sleep(delay)

# Game over
turtle.goto(0, 0)
turtle.color("white")
turtle.write("Game Over", align="center", font=("Courier", 24, "normal"))
turtle.hideturtle()
screen.mainloop()
