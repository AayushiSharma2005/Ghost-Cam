# 👻 Ghost-Cam: Real-Time Human De-Identification for Privacy-Preserving Behavioral Monitoring

## 📌 Overview

Ghost-Cam is a real-time computer vision system designed for **privacy-preserving human activity monitoring**. It detects and analyzes human behavior while completely removing identity information from video frames.

Instead of storing raw video, the system converts humans into **anonymous skeletal representations (17 keypoints)** and removes all visible human pixels using background inpainting. This ensures behavioral understanding without exposing personal identity.

---

## 🚀 Key Features

- 🧍 Real-time human pose estimation using YOLOv8 Pose (17 keypoints)
- 🧼 Complete human removal using background inpainting (Telea algorithm)
- 🧠 Behavioral analysis without biometric storage
- ⚠️ Fall detection using multi-signal temporal logic
- 🚧 Zone breach detection using coordinate-based tracking
- ✋ Reaching gesture detection for safety monitoring
- 👥 Crowd density estimation using skeleton count
- 🔐 Privacy-preserving design (no raw frame storage)
- 📉 ~99% reduction in storage using JSON-based telemetry

---

## 🧠 System Pipeline

1. Video Frame Input  
2. Parallel Processing  
   - Skeleton Extraction (YOLOv8 Pose)  
   - Human Removal (Background Inpainting)  
3. Frame Synchronization  
4. Behavioral Analysis Engine  
5. Ghost Stream Generation  
6. JSON Metadata Output  

---

## 🧩 Core Modules

### 1. Pose Estimation
- Model: YOLOv8 Pose (Ultralytics)
- Output: 17 skeletal keypoints per detected person

### 2. De-Identification Engine
- Background plate generated using median frame approach
- Human removal using:
  - YOLO-based segmentation mask
  - Motion-based difference mask
- Telea inpainting for seamless background reconstruction

### 3. Fall Detection
Uses multi-signal temporal analysis:
- Body aspect ratio change
- Hip centroid drop
- Rapid posture change
- Head-to-hip alignment
- Temporal buffer for stability and noise reduction

### 4. Zone Breach Detection
- Based on hip centroid position
- Detects entry into restricted regions defined in frame coordinates

### 5. Reaching Detection
- Detects wrist movement above shoulder threshold
- Used for unsafe or restricted access monitoring

### 6. Crowd Monitoring
- Counts valid detected persons using keypoints confidence
- Generates alerts when threshold is exceeded

---

## 📊 Dataset

- UR Fall Detection Dataset
  - 30 ADL (normal activities)
  - 40 fall sequences
  - RGB videos (480×640 resolution, 20 FPS)
  - Multi-camera viewpoints
  - Ground truth labels included

---

## ⚙️ Tech Stack

- Python 3.x  
- YOLOv8n-Pose (Ultralytics)  
- OpenCV (cv2)  
- NumPy  
- Matplotlib  
- JSON for metadata output  
- Kaggle CPU environment  

---

## 📈 Performance

| Metric | Value |
|--------|------|
| Average FPS | 9–10 FPS (CPU) |
| YOLO Inference | 6–8 ms/frame |
| Inpainting Time | 40–60 ms/frame |
| Total Latency | 46–68 ms/frame |
| Storage Reduction | ~99% |
| Output Video Size | 8–12 MB |
| JSON Metadata | ~200 KB per sequence |

---

## 🎯 Fall Detection Performance

| Sequence | Sensitivity | Specificity | Accuracy | F1 Score |
|----------|------------|-------------|----------|----------|
| Fall Cases | High (up to 100%) | High (up to 100%) | ~93–96% | ~0.90–0.95 |
| ADL Cases | — | 98–100% | High stability | — |

---

## 🧾 Output Format

Each frame generates structured metadata:

```json
{
  "frame_id": 1,
  "timestamp": "00:00:01",
  "person_count": 2,
  "fall_detected": false,
  "zone_breach": false,
  "reaching": "right",
  "keypoints": [[x1,y1], [x2,y2], ...],
  "events": ["zone_breach"]
}
