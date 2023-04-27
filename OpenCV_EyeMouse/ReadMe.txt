Eye Controlled Mouse:

This program uses the Mediapipe library to detect facial landmarks and control the mouse cursor by tracking the movement of the user's left eye.


Requirements:

Python 3.x
OpenCV
Mediapipe
PyAutoGUI


Usage:

Connect a webcam to your computer.
Install the required libraries using pip.
Run the EyeMouse.py script.


How it Works:

The program uses a trained facial landmark detection model provided by Mediapipe to detect facial landmarks, and extracts the coordinates of the left eye landmarks (landmarks[145] and landmarks[159]). These coordinates are used to track the movement of the user's left eye.

The PyAutoGUI library is used to control the mouse cursor. The program calculates the screen coordinates of the user's left eye by multiplying the normalized landmark coordinates by the screen dimensions. If the user blinks their left eye (i.e., the distance between the upper and lower eyelids becomes very small), a mouse click is simulated.


Code Explanation:

Import the required libraries:
 
import cv2
import mediapipe as mp
import pyautogui


Initialize the webcam and the Mediapipe face mesh model: 

cam = cv2.VideoCapture(0)
face_mesh = mp.solutions.face_mesh.FaceMesh(refine_landmarks=True)


Get the screen dimensions using the PyAutoGUI library:
 
screen_w, screen_h = pyautogui.size()


Loop over the frames captured by the webcam:

while True:
    _, frame = cam.read()
	
	
Flip the frame horizontally to create a mirror effect:
 
frame = cv2.flip(frame, 1)


Convert the frame to RGB format:
 
rgb_frame = cv2.cvtColor(frame, cv2.COLOR_BGR2RGB)


Process the frame using the Mediapipe face mesh model:
 
output = face_mesh.process(rgb_frame)
landmark_points = output.multi_face_landmarks


If facial landmarks are detected, extract the coordinates of the left eye landmarks and calculate their screen coordinates:
 
if landmark_points:
    landmarks = landmark_points[0].landmark
    left = [landmarks[145], landmarks[159]]
    for landmark in left:
        screen_x = screen_w * landmark.x
        screen_y = screen_h * landmark.y
		
		
Use the PyAutoGUI library to move the mouse cursor to the coordinates of the left eye:
 
pyautogui.moveTo(screen_x, screen_y)


If the user blinks their left eye, simulate a mouse click:
 
if (left[0].y - left[1].y) < 0.004:
    pyautogui.click()
    pyautogui.sleep(1)
	
	
Display the frame with the landmarks drawn on it:

cv2.imshow('Eye Controlled Mouse', frame)
cv2.waitKey(1)