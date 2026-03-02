# 🚗 Advanced Lane Detection using Computer Vision

A complete lane detection pipeline built using classical Computer Vision techniques in Python and OpenCV.  
This system processes road videos in real-time and detects lane boundaries, estimates curvature, and calculates vehicle position relative to lane center.

---

## 📌 Project Overview

This project implements an end-to-end Advanced Lane Detection system designed to work under varying lighting conditions and curved roads.

The pipeline includes:

- Camera calibration and distortion correction
- Multi-channel gradient and color thresholding
- Perspective (Bird’s Eye View) transformation
- Lane pixel detection using sliding windows
- Polynomial curve fitting
- Radius of curvature estimation (in meters)
- Vehicle offset estimation from lane center
- Temporal smoothing and sanity validation
- Real-time visualization on video

---

## 🧠 Processing Pipeline

### 1️⃣ Camera Calibration & Undistortion
Camera calibration parameters are loaded from precomputed pickle files.  
Each frame is undistorted before further processing to remove lens distortion.

---

### 2️⃣ Gradient & Color Thresholding

To achieve robust lane detection, multiple techniques are combined:

- Sobel X and Sobel Y gradients
- Gradient magnitude threshold
- Gradient direction threshold
- R channel threshold (RGB)
- S channel threshold (HLS)
- V channel threshold (HSV)

Combining multiple color spaces improves performance under shadows, brightness variations, and faded lane markings.

The result is a binary image highlighting potential lane pixels.

---

### 3️⃣ Perspective Transform (Bird’s Eye View)

The binary image is transformed into a top-down view using a perspective warp matrix.

This simplifies curve detection by making lane lines approximately vertical and parallel.

---

### 4️⃣ Lane Detection Strategy

Two detection approaches are used:

#### 🔹 Sliding Window Search
- Used during initial detection
- Uses histogram-based window scanning
- Dynamically recenters windows based on detected pixels

#### 🔹 Search Around Previous Fit
- Used when lanes are already detected
- Searches within a margin around previous polynomial fit
- Faster and more stable

---

### 5️⃣ Polynomial Curve Fitting

A second-order polynomial is fitted separately for left and right lanes:

x = Ay² + By + C

This allows smooth curve modeling even on curved roads.

---

### 6️⃣ Curvature & Vehicle Position Estimation

The system computes:

- Radius of curvature (in meters)
- Distance of vehicle from lane center

Pixel-to-meter conversion factors are applied for real-world measurement.

---

### 7️⃣ Temporal Smoothing & Sanity Checks

To improve stability across frames:

- Exponential filtering is applied
- Previous fits are averaged
- Horizontal lane distance is validated
- Curvature ratio is checked
- Wrong detections are rejected

This prevents sudden jumps and false lane detections.

---

### 8️⃣ Lane Overlay & Visualization

Detected lanes are:

- Drawn as a filled region
- Warped back to original perspective
- Overlaid onto the original frame

The final output displays:
- Radius of curvature
- Vehicle position offset
- Lane lock status

---
