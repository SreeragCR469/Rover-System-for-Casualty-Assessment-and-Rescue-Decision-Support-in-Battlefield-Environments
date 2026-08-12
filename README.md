# Rover System for Casualty Assessment and Rescue Decision Support in Battlefield Environments

An intelligent rover system that combines computer vision, machine learning, environmental sensing, and IoT communication to assess casualties and surrounding hazards in dangerous environments and provide structured information for remote rescue decision support.

## Overview

Rescue operations in battlefield and disaster environments can expose personnel to hazards such as fire, smoke, toxic gases, debris, and unstable surroundings. Directly assessing casualties in such environments can therefore be dangerous and time-consuming.

This project presents an intelligent reconnaissance rover designed to remotely assess potential casualties while simultaneously monitoring environmental hazards.

The system combines:

- Computer vision for human and posture detection
- Machine learning for fall detection
- Temporal analysis for casualty mobility assessment
- Environmental sensing for hazard classification
- ESP32-based rover locomotion and sensor control
- Raspberry Pi-based edge AI processing
- MQTT-based communication
- A real-time dashboard for situational awareness

The system is designed to assist human operators rather than replace human decision-making. The final output is a structured casualty and environmental assessment that can support rescue deployment decisions.

---

## Key Features

- Real-time human detection and pose estimation
- Fall detection using MediaPipe Pose and Random Forest
- Posture classification for standing, sitting, and lying states
- 15-frame fall confirmation to reduce transient false detections
- 60-second temporal stability analysis
- Stationary vs. moving casualty assessment
- Temperature, gas, and flame monitoring
- Four-level environmental hazard classification:
  - SAFE
  - WARNING
  - DANGER
  - ABORT ZONE
- GPS-based casualty location reporting
- MQTT-based sensor communication
- Real-time Flask dashboard
- Edge inference on Raspberry Pi 4
- ESP32-based rover locomotion and environmental sensing

---

## System Architecture

The rover uses a dual-processor architecture.

### Raspberry Pi 4

The Raspberry Pi acts as the main AI and vision-processing platform. It performs:

- Camera acquisition
- MediaPipe Pose landmark extraction
- Feature extraction
- Machine learning-based fall detection
- Posture classification
- Temporal stability analysis
- Casualty assessment
- Dashboard operation

### ESP32

The ESP32 operates independently for:

- Rover locomotion
- Environmental sensor acquisition
- Hazard classification
- Motor control

The two subsystems communicate through wireless and serial interfaces, allowing the computationally intensive vision pipeline to remain independent from the real-time sensing and motor-control tasks.

---

## Hardware

### Main Components

- Raspberry Pi 4 Model B
- Raspberry Pi Camera Module
- ESP32 microcontroller
- MQ-2 gas sensor
- KY-026 infrared flame sensor
- DHT22 temperature and humidity sensor
- GPS module
- L298N dual H-bridge motor driver
- Four-wheel differential-drive chassis
- DC geared motors

The Raspberry Pi camera provides a 640 × 480 video stream for the computer vision pipeline. The ESP32 interfaces with the environmental sensors and controls the rover motors through the L298N motor driver.

---

## Software and Technologies

- Python
- C/C++ for ESP32
- Raspberry Pi OS
- MediaPipe Pose
- Scikit-learn
- Random Forest
- OpenCV
- Flask
- MQTT
- Mosquitto MQTT Broker
- GPS
- Jupyter/Google Colab for model development

---

## Computer Vision Pipeline

The computer vision subsystem uses the Raspberry Pi Camera to detect people and extract body landmarks using MediaPipe Pose.

MediaPipe provides 33 three-dimensional body landmarks for each detected person.

Instead of directly processing raw images using a computationally expensive deep neural network, the system extracts a compact six-feature representation from the pose landmarks.

### Extracted Features

1. Body angle
2. Body symmetry
3. Bounding-box aspect ratio
4. Head-to-ground distance
5. Wrist-to-hip distance
6. Landmark visibility

The six-dimensional feature vector is then provided to lightweight machine learning classifiers.

This feature-based approach reduces computational requirements and enables real-time inference on the Raspberry Pi without requiring a GPU.

---

## Machine Learning Model

Six supervised classifiers were evaluated:

- Random Forest
- Gradient Boosting
- Support Vector Machine (RBF)
- Multi-Layer Perceptron
- Soft Voting Ensemble
- Stacking Ensemble

The classifiers were evaluated using the same extracted features and train/test split.

Because missing a genuine casualty is considered more critical than generating an isolated false alarm, FALL-class recall was considered an important evaluation criterion along with accuracy, precision, F1 score, and inference latency.

### Final Model

Random Forest was selected for deployment because it provides a strong balance between:

- Fall detection performance
- Precision
- Accuracy
- Inference speed
- Computational requirements

The final Random Forest model uses 200 estimators and was designed for deployment on the resource-constrained Raspberry Pi platform.

---

## Temporal Stability Analysis

A single frame classified as a fall is not considered sufficient evidence of a casualty.

To reduce transient misclassifications, the system requires 15 consecutive FALL classifications before registering a fall event.

After a confirmed fall, the system observes the detected subject for 60 seconds.

Eight body landmarks are tracked during this period:

- Head
- Left shoulder
- Right shoulder
- Left hip
- Right hip
- Left knee
- Right knee
- Left ankle

The maximum standard deviation of the tracked landmark coordinates is used as the movement metric.

### Classification

```text
σmax ≤ 0.015  →  STATIONARY
σmax > 0.015  →  MOVING
