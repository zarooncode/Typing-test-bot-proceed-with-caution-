# Typing-test-bot-proceed-with-caution-
This is a typing test auto bot typer that will type the test for you!
Coordinates may vary so follow code properly!
Inspiration from The Coding Sloth.
Use at your own risk, may get banned from certain websites for using this bot to type for you!

CODE:
```python
# modules/variables
import os
import time
import cv2
import keyboard
import pyautogui
import pytesseract

os.chdir(r"C:\Users\pc\Desktop\PYTHON\AI stuff and bots")
triggered = False

# functions
def pre_process(image_path): 
    image = cv2.imread(image_path)
    im_gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)
    _, im_black = cv2.threshold(im_gray, 0, 255, cv2.THRESH_BINARY, cv2.THRESH_OTSU)
    im_inverted = cv2.bitwise_not(im_black)
    kernel = cv2.getStructuringElement(cv2.MORPH_RECT, (2, 2))
    # im_dilated = cv2.dilate(im_inverted, kernel, iterations=1)
    return im_gray

def take_Screenshot():
    screenshot_path = "SPEED_TEST.png"
    screenshot = pyautogui.screenshot(region=(x,y,w,h)) # for the region add the coordinates and width nd height according to where the text in ur typing game is
    screenshot.save(screenshot_path)
    screenshot_processed = pre_process(screenshot_path)
    cv2.imwrite("Processed.png", screenshot_processed)
    return screenshot_processed

def extract_text(image):
    return pytesseract.image_to_string(image, config="--psm 6")

def clean_text(text):
    # Just replace line breaks with spaces
    return text.replace('\n', ' ').replace('\r', ' ')

def write_text(cleaned_text):
    pyautogui.write(cleaned_text, interval=0.05)

# main loop
while not triggered:
    if keyboard.is_pressed("alt") and keyboard.is_pressed("t"):
        triggered = True 
        print("taking image")
        processed_im = take_Screenshot()
        extracted_text = extract_text(processed_im)
        cleaned_text = clean_text(extracted_text)

        write_text(cleaned_text)
    
    time.sleep(0.05) 

if keyboard.is_pressed("esc"):
        print('escaped')
        triggered = False
