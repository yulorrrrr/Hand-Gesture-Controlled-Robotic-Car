# AI-Based Hand Gesture Controlled Robotic Car

A deep learning and embedded systems project that recognizes hand gestures in real time using a custom Convolutional Neural Network (CNN) and controls a robotic car through Raspberry Pi and Arduino communication.

The system performs live camera inference on a Raspberry Pi and translates gestures into robot movement commands.

---

## Project Overview

This project combines:

- Computer Vision
- Deep Learning (PyTorch CNN)
- Embedded AI
- Raspberry Pi
- Arduino motor control
- Real-time inference

The model recognizes five hand gestures and converts them into robotic actions.  

Current workflow:

Camera → CNN → Gesture Prediction → Raspberry Pi → Arduino → Motor Control  

Current Status:

✅ Dataset collection complete  
✅ CNN model training complete  
✅ Real-time gesture recognition complete  
✅ Arduino software  
🔄 Raspberry Pi integration ongoing  
🔄 Robot hardware integration ongoing

---

## System Architecture

```text
Pi Camera
    │
    ▼
┌────────────────────┐
│ Raspberry Pi       │
│ Real-time CNN      │
│ Gesture Recognition│
└────────────────────┘
    │
    ▼
 Gesture Command

┌────────────────────┐
│ Arduino            │
│ Motor Controller   │
└────────────────────┘
    │
    ▼

┌────────────────────┐
│ Robotic Car        │
└────────────────────┘
```

---

## Supported Gestures

| Gesture | Car Action |
|----------|------------|
| ✋ Up | Forward |
| 🫳 Down | Backward |
| 🫲 Left | Turn Left |
| 🫱 Right | Turn Right |
| ✊ Fist | Stop |

---

## Dataset

link for dowanload: https://drive.google.com/file/d/1XIfKAD9617ZEVZF2Kdq29gbn-D-zA4Yl/view?usp=drive_link

Custom dataset collected using self-recorded videos.

Frames extracted from videos using [Video Frame Extractor](https://frame-extractor.com/en) and manually organized into gesture classes.

Classes:

- Down
- Fist
- Left
- Right
- Up

Image preprocessing:

- Resize: 128×128
- RGB images
- Normalization
- Data augmentation

Training augmentation:

```python
transforms.ColorJitter(
    brightness=0.3,
    contrast=0.3
)
```

---

## Project Structure

```text
AI-Based Hand Gesture Controlled Robotic Car/

├── Dataset/
│   ├── Train/     # 800 images for each class
│   │   ├── Down
│   │   ├── Fist
│   │   ├── Left
│   │   ├── Right
│   │   └── Up
│   │
│   └── Test/      # 200 images for each class
│       ├── Down
│       ├── Fist
│       ├── Left
│       ├── Right
│       └── Up
│
├── model/
│   └── model.pth
│
├── src/
│   ├── Image_Classification.py
│   ├── Gesture_Recognition_pi.py
│   ├── Model_def.py
│   ├── Real_Time_Test.py
│   └── Dataset_Reshuffling.py
│
└── README.md
```

---

## CNN Model Architecture

```text
Input (3×128×128)

Conv2d(3→16)
BatchNorm
ReLU
MaxPool

↓

Conv2d(16→32)
BatchNorm
ReLU
MaxPool

↓

Conv2d(32→64)
BatchNorm
ReLU
MaxPool

↓

Flatten

16384

↓

Linear(16384→256)

↓

Dropout(0.5)

↓

Linear(256→64)

↓

Linear(64→5)
```

---

## Training Configuration

| Parameter | Value |
|------------|--------|
| Optimizer | Adam |
| Learning Rate | 0.0005 |
| Batch Size | 8 |
| Epochs | 20 |
| Loss | CrossEntropyLoss |

---

## Real-Time Inference Features

The Raspberry Pi implementation includes:

### Center ROI extraction

To reduce background noise, only the center region of the image is used:

```python
roi = get_center_roi(frame)
```

---

### Confidence threshold

Low-confidence predictions become:

```python
"No Gesture"
```

to avoid accidental robot movement.

---

### Temporal smoothing

Predictions are stabilized using:

```python
deque()
Counter()
```

This prevents frame-by-frame fluctuations.

---

## Hardware

- Raspberry Pi 5
- Pi Camera
- Arduino Uno
- Motor Driver
- Robotic Chassis
- Battery Pack

---

## Installation

Install dependencies:

```bash
pip install torch torchvision matplotlib torchsummary opencv-python pillow picamera2
```

---
