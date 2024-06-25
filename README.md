#baby snake
import time
import random
import tkinter as tk
CANVAS_WIDTH = 400
CANVAS_HEIGHT = 400
SIZE = 40
DELAY = 0.1

class Game:
    def __init__(self, canvas):
        self.canvas = canvas
        self.dx = 0
        self.dy = 0
        self.direction = 'Right'
        self.count = 0
        self.create_player()
        self.create_goal()
        self.canvas.bind_all("<KeyPress>", self.key_press)
        self.run_game()

    def create_player(self):
        self.player = self.canvas.create_rectangle(0, 0, SIZE, SIZE, fill="blue")

    def create_goal(self):
        self.goal_x = 360
        self.goal_y = 360
        self.goal = self.canvas.create_rectangle(self.goal_x, self.goal_y, self.goal_x + SIZE, self.goal_y + SIZE, fill="red")

    def key_press(self, event):
        if event.keysym == 'Left':
            self.direction = 'Left'
        elif event.keysym == 'Right':
            self.direction = 'Right'
        elif event.keysym == 'Up':
            self.direction = 'Up'
        elif event.keysym == 'Down':
            self.direction = 'Down'

    def move_goal(self):
        self.goal_x = random.randint(0, CANVAS_WIDTH // SIZE - 1) * SIZE
        self.goal_y = random.randint(0, CANVAS_HEIGHT // SIZE - 1) * SIZE
        self.canvas.moveto(self.goal, self.goal_x, self.goal_y)

    def run_game(self):
        while True:
            # Update the player's position
            if self.direction == 'Left':
                self.dx -= SIZE
            elif self.direction == 'Right':
                self.dx += SIZE
            elif self.direction == 'Up':
                self.dy -= SIZE
            elif self.direction == 'Down':
                self.dy += SIZE

            # Ensure player stays within bounds
            self.dx = max(0, min(self.dx, CANVAS_WIDTH - SIZE))
            self.dy = max(0, min(self.dy, CANVAS_HEIGHT - SIZE))
            self.canvas.moveto(self.player, self.dx, self.dy)

            # Check for out of bounds
            if self.dx < 0 or self.dx >= CANVAS_WIDTH or self.dy < 0 or self.dy >= CANVAS_HEIGHT:
                break

            # Detecting collisions
            if (self.dx == self.goal_x and self.dy == self.goal_y):
                self.move_goal()
                self.count += 1

            # Display points
            text_obj = self.canvas.create_text(10, 10, text=f"{self.count} points", anchor='nw')

            # Sleep
            time.sleep(DELAY)

            # Delete old points text
            self.canvas.delete(text_obj)

        # Show result
        result = f"Your Score is : {str(self.count)}"
        self.canvas.create_text(CANVAS_WIDTH/2, CANVAS_HEIGHT/2 - 10, text='Game Over', fill='red')
        self.canvas.create_text(CANVAS_WIDTH/2, CANVAS_HEIGHT/2 + 10, text=result, fill='blue')

def main():
    root = tk.Tk()
    root.title("Move the Player Game")
    canvas = tk.Canvas(root, width=CANVAS_WIDTH, height=CANVAS_HEIGHT)
    canvas.pack()
    game = Game(canvas)
    root.mainloop()

if __name__ == "__main__":
    main()
