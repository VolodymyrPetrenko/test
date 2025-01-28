import pyautogui
import time
import random

def move_cursor_randomly(iterations=10, delay=30, box_size=1000):
    screen_width, screen_height = pyautogui.size()
    start_x = (screen_width - box_size) // 2
    start_y = (screen_height - box_size) // 2

    for i in range(iterations):
        random_x = random.randint(start_x, start_x + box_size)
        random_y = random.randint(start_y, start_y + box_size)
        speed = random.uniform(0.5, 3.0)
        pyautogui.moveTo(random_x, random_y, duration=speed)

        time.sleep(delay)

if __name__ == "__main__":
    time.sleep(3)
    move_cursor_randomly(iterations=200, delay=60, box_size=100)
