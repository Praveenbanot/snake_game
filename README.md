# snake_game
Step 1: Import Libraries
python
import turtle
import time
import random

turtle: A graphics library in Python that allows for drawing shapes and controlling a turtle (cursor) on the screen.
time: A library for handling time-related tasks, such as adding delays.
random: A library for generating random numbers, which is used to place food at random positions.
Step 2: Initialize Game Variables
python
delay = 0.2

# Score 
score = 0
high_score = 0

delay: Controls the speed of the game loop. A lower value means the game runs faster.
score: Tracks the current score of the player.
high_score: Keeps track of the highest score achieved in the game.
Step 3: Set Up the Screen
python
wn = turtle.Screen()
wn.title("Snake Game")
wn.bgcolor("White")
wn.setup(width=600, height=600)
wn.tracer(0)

wn: Creates a window (screen) for the game.
title(): Sets the title of the window to "Snake Game".
bgcolor(): Sets the background color of the window to white.
setup(): Defines the dimensions of the window (600 pixels wide and 600 pixels tall).
tracer(0): Disables automatic screen updates to improve performance, allowing manual control over when to update the screen.
Step 4: Create Snake Head
python
head = turtle.Turtle()
head.speed(0)
head.shape("square")
head.color("black")
head.penup()
head.goto(0, 0)
head.direction = "stop"

head: Creates a turtle object that represents the snake's head.
speed(0): Sets the drawing speed to maximum (no animation delay).
shape(): Sets the shape of the turtle to a square (the snake's head).
color(): Colors the snake's head black.
penup(): Prevents drawing lines when moving (the turtle won't leave a trail).
goto(0, 0): Places the snake's head at the center of the screen.
direction: Initializes the direction of movement as "stop".
Step 5: Create Snake Food
python
food = turtle.Turtle()
food.speed(0)
food.shape("circle")
food.color("Red")
food.penup()
food.goto(0, 100)

Similar to creating the snake head, this block creates a food object:
The shape is set to a circle and colored red.
It is initially positioned at (0, 100).
Step 6: Initialize Segments List
python
segments = []

This list will store segments of the snake's body as it grows longer after eating food.
Step 7: Create Score Display
python
pen = turtle.Turtle()
pen.speed(0)
pen.shape("square")
pen.color("black")
pen.penup()
pen.hideturtle()
pen.goto(0, 260)
pen.write("Score: 0  High Score: 0", align="center", font=("courier", 24, "normal"))

A new turtle object (pen) is created to display scores:
It is hidden (hideturtle()), so only text appears on-screen.
Positioned at (0, 260), it writes initial scores on-screen using write().
Step 8: Define Movement Functions
These functions change the direction of the snake based on user input:
python
def go_up():
    if head.direction != "down":
        head.direction = "up"

def go_down():
    if head.direction != "up":
        head.direction = "down"

def go_left():
    if head.direction != "right":
        head.direction = "left"

def go_right():
    if head.direction != "left":
        head.direction = "right"

Step 9: Define Movement Logic
python
def move():
    if head.direction == "up":
        y = head.ycor()
        head.sety(y + 20)

    if head.direction == "down":
        y = head.ycor()
        head.sety(y - 20)

    if head.direction == "left":
        x = head.xcor()
        head.setx(x - 20)

    if head.direction == "right":
        x = head.xcor()
        head.setx(x + 20)

The move function updates the position of the snake's head based on its current direction. Each movement changes its coordinates by 20 pixels.
Step 10: Keyboard Bindings
python
wn.listen()
wn.onkey(go_up, "u")
wn.onkey(go_down, "d")
wn.onkey(go_left, "l")
wn.onkey(go_right, "r")

These lines set up keyboard controls:
The listen() method allows Turtle to listen for key presses.
Each onkey method binds a specific key (u, d, l, r) to its corresponding movement function.
Step 11: Main Game Loop
python
try:
    while True:
        wn.update()

        # Check for a collision with the border
        if head.xcor() > 290 or head.xcor() < -290 or head.ycor() > 290 or head.ycor() < -290:
            time.sleep(1)
            head.goto(0, 0)
            head.direction = "stop"

            # Hide segments
            for segment in segments:
                segment.goto(1000, 1000)  # Move off-screen

            # Clear segment list
            segments.clear()

        # Check for a collision with food
        if head.distance(food) < 20:
            # Move food to random position
            x = random.randint(-290, 290)
            y = random.randint(-290, 290)
            food.goto(x, y)

            # Add a segment
            new_segment = turtle.Turtle()
            new_segment.speed(0)
            new_segment.shape("square")
            new_segment.color("black")
            new_segment.penup()
            segments.append(new_segment)

            # Increase score
            score += 10

            if score > high_score:
                high_score = score

            pen.clear()  
            pen.write("Score: {} High Score: {}".format(score, high_score), align="center", font=("courier", 24, "normal"))

        # Move end segments first in reverse order
        for index in range(len(segments) - 1, 0, -1):
            x = segments[index - 1].xcor()
            y = segments[index - 1].ycor()
            segments[index].goto(x, y)

        # Move segment zero to where the head is
        if len(segments) > 0:
            x = head.xcor()
            y = head.ycor()
            segments[0].goto(x, y)

        move()

        # Check for collision with body segments
        for segment in segments:
            if segment.distance(head) < 20:
                time.sleep(1)
                head.goto(0, 0)
                head.direction = "stop"

                # Hide segments
                for segment in segments:
                    segment.goto(1000, 1000) 

                # Clear segment list
                segments.clear()

        time.sleep(delay)

except turtle.Terminator:
    print("Game closed.")

Breakdown of Main Loop:
Update Screen: wn.update() refreshes the screen.
Collision with Borders:
If the snake hits any border (beyond ±290 on x or y), it resets its position and stops moving. All segments are hidden and cleared from memory.
Collision with Food:
If the snake's distance from food is less than 20, it indicates that it has eaten food. The food is repositioned randomly within bounds.
A new segment is added to the snake's body.
The score increases by 10, and if it exceeds high_score, it updates high_score.
The score display is updated by clearing previous text and writing new scores.
Move Segments:
The body segments are moved in reverse order so that they follow correctly behind the snake's head.
Self-Collision Check:
If any segment collides with the snake's head (distance less than 20), it resets as before and clears all body segments.
Final Step
python
wn.mainloop()

This line keeps the window open until it is closed by user action.
