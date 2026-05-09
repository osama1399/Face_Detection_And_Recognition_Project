# Face Detection & Recognition System using OpenCV

A lightweight real-time face detection application built with Python and OpenCV.  
This project captures live webcam video, detects human faces using the Haar Cascade Classifier, and highlights them with bounding boxes in real time.

---

# Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Tech Stack](#tech-stack)
- [System Requirements](#system-requirements)
- [Installation](#installation)
- [Project Structure](#project-structure)
- [Complete Source Code](#complete-source-code)
- [How to Run](#how-to-run)
- [Controls](#controls)
- [How Face Detection Works](#how-face-detection-works)
- [Code Explanation](#code-explanation)
- [Customization](#customization)
- [Troubleshooting](#troubleshooting)
- [Performance Tips](#performance-tips)
- [Limitations](#limitations)
- [Future Improvements](#future-improvements)
- [License](#license)
- [Credits](#credits)

---

# Overview

This project demonstrates the fundamentals of:

- Computer Vision
- Real-Time Video Processing
- Face Detection using Haar Cascades
- Webcam Integration with OpenCV

The application processes each webcam frame, converts it to grayscale, detects faces, and draws rectangles around them in real time.

---

# Features

- Real-time face detection
- Webcam video stream processing
- Haar Cascade based face detection
- Lightweight and fast performance
- Multiple face detection support
- Cross-platform compatibility
- Keyboard-controlled exit
- Easy customization and expansion

---

# Tech Stack

| Technology | Purpose |
|---|---|
| Python | Programming language |
| OpenCV | Computer vision library |
| Haar Cascade Classifier | Face detection algorithm |

---

# System Requirements

| Requirement | Minimum |
|---|---|
| Python | 3.7+ |
| RAM | 4 GB |
| Webcam | Built-in or external |
| Operating System | Windows / Linux / macOS |

---

# Installation

## Step 1: Create Project Directory

```bash
mkdir face_detection_project
cd face_detection_project
```

---

## Step 2: Create Virtual Environment

### Windows

```bash
python -m venv venv
venv\Scripts\activate
```

### Linux / macOS

```bash
python3 -m venv venv
source venv/bin/activate
```

---

## Step 3: Install OpenCV

```bash
pip install opencv-python
```

---

# Project Structure

```text
face_detection_project/
│
├── face_detector.py      # Main application
├── README.md             # Documentation
└── venv/                 # Virtual environment
```

---

# Complete Source Code

Create a file named `face_detector.py` and paste the following code:

```python
import cv2

# Load the Haar Cascade face detection model
face_cascade = cv2.CascadeClassifier(
    cv2.data.haarcascades + 'haarcascade_frontalface_default.xml'
)

# Open the default webcam
cap = cv2.VideoCapture(0)

# Check if webcam opened successfully
if not cap.isOpened():
    print("Error: Could not access webcam.")
    exit()

print("===================================")
print(" Face Detection System Started")
print(" Press 'q' to quit")
print("===================================")

while True:

    # Read frame from webcam
    success, frame = cap.read()

    # Verify frame capture
    if not success:
        print("Failed to capture frame")
        break

    # Convert frame to grayscale
    gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)

    # Detect faces
    faces = face_cascade.detectMultiScale(
        gray,
        scaleFactor=1.1,
        minNeighbors=4
    )

    # Draw rectangles around detected faces
    for (x, y, w, h) in faces:

        cv2.rectangle(
            frame,
            (x, y),
            (x + w, y + h),
            (255, 125, 0),
            2
        )

    # Display output window
    cv2.imshow("Face Detection System", frame)

    # Exit if 'q' key is pressed
    if cv2.waitKey(30) & 0xFF == ord('q'):
        break

# Release webcam resources
cap.release()

# Close all OpenCV windows
cv2.destroyAllWindows()

print("Application Closed")
```

---

# How to Run

Run the following command:

```bash
python face_detector.py
```

---

# Controls

| Key | Function |
|---|---|
| `q` | Quit application |
| `Ctrl + C` | Force terminate |

---

# How Face Detection Works

The system follows the pipeline below:

```text
Webcam Input
      ↓
Capture Video Frame
      ↓
Convert to Grayscale
      ↓
Run Haar Cascade Detection
      ↓
Detect Face Coordinates
      ↓
Draw Bounding Boxes
      ↓
Display Output Window
```

---

# Code Explanation

---

## 1. Import OpenCV

```python
import cv2
```

Imports the OpenCV library for computer vision operations.

---

## 2. Load Haar Cascade Model

```python
face_cascade = cv2.CascadeClassifier(
    cv2.data.haarcascades + 'haarcascade_frontalface_default.xml'
)
```

Loads the pre-trained Haar Cascade XML model for frontal face detection.

---

## 3. Open Webcam

```python
cap = cv2.VideoCapture(0)
```

- `0` represents the default webcam.
- Use `1`, `2`, etc. for external cameras.

---

## 4. Convert to Grayscale

```python
gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
```

Haar Cascade detection works better on grayscale images because:

- Lower computational complexity
- Faster processing
- Improved feature extraction

---

## 5. Detect Faces

```python
faces = face_cascade.detectMultiScale(
    gray,
    scaleFactor=1.1,
    minNeighbors=4
)
```

### Parameters

| Parameter | Purpose |
|---|---|
| `gray` | Input grayscale image |
| `scaleFactor` | Image scaling factor |
| `minNeighbors` | Detection confidence threshold |

---

## 6. Draw Bounding Boxes

```python
cv2.rectangle(
    frame,
    (x, y),
    (x + w, y + h),
    (255, 125, 0),
    2
)
```

### Rectangle Arguments

| Argument | Description |
|---|---|
| `(x, y)` | Top-left corner |
| `(x+w, y+h)` | Bottom-right corner |
| `(255,125,0)` | Rectangle color (BGR) |
| `2` | Border thickness |

---

# Customization

---

## Change Rectangle Color

```python
# Blue
(255, 0, 0)

# Green
(0, 255, 0)

# Red
(0, 0, 255)

# Yellow
(0, 255, 255)

# Purple
(255, 0, 255)
```

---

## Change Border Thickness

```python
# Thin border
1

# Medium border
2

# Thick border
5

# Filled rectangle
-1
```

---

## Increase Detection Sensitivity

```python
faces = face_cascade.detectMultiScale(
    gray,
    scaleFactor=1.05,
    minNeighbors=3
)
```

### Advantages

- Detects smaller faces
- More sensitive detection

### Disadvantages

- More false positives

---

## Improve Detection Accuracy

```python
faces = face_cascade.detectMultiScale(
    gray,
    scaleFactor=1.2,
    minNeighbors=6
)
```

### Advantages

- Fewer false detections
- Better accuracy

### Disadvantages

- May miss distant faces

---

# Add Face Counter

```python
face_count = len(faces)

cv2.putText(
    frame,
    f"Faces: {face_count}",
    (10, 30),
    cv2.FONT_HERSHEY_SIMPLEX,
    1,
    (0, 255, 0),
    2
)
```

---

# Add Screenshot Functionality

```python
if cv2.waitKey(30) & 0xFF == ord('s'):
    cv2.imwrite("screenshot.png", frame)
    print("Screenshot Saved")
```

---

# Troubleshooting

| Problem | Solution |
|---|---|
| `No module named cv2` | Run `pip install opencv-python` |
| Webcam not opening | Change `VideoCapture(0)` to `VideoCapture(1)` |
| Black screen | Check webcam permissions |
| No face detected | Improve lighting conditions |
| False detections | Increase `minNeighbors` |
| Laggy performance | Close background applications |

---

# Performance Tips

- Use adequate lighting
- Keep face clearly visible
- Avoid noisy backgrounds
- Use HD webcams
- Reduce webcam resolution for higher FPS
- Close camera-consuming applications

---

# Limitations

Haar Cascade detection has some limitations:

- Less accurate than deep learning models
- Sensitive to lighting conditions
- Can produce false positives
- Reduced accuracy at extreme face angles

---

# Future Improvements

Possible upgrades for this project:

- Deep Learning face detection
- Face recognition system
- Emotion detection
- Eye tracking
- Face mask detection
- GPU acceleration
- Multi-threaded video processing
- Attendance system integration

---

# Uninstall / Cleanup

Deactivate virtual environment:

```bash
deactivate
```

Delete project directory:

### Linux / macOS

```bash
cd ..
rm -rf face_detection_project
```

### Windows

```bash
cd ..
rmdir /s face_detection_project
```

---

# License

This project is open-source and free for educational purposes.

---

# Credits

- OpenCV Development Team
- Viola–Jones Face Detection Algorithm
- Python Community

---

# Version

```text
Version: 1.0.0
```

---

# Author

```text
Osama Khan
M.Sc. Engineering Cybernetics & Automation Engineering
University of Stuttgart & University of Bologna
```
