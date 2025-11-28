# Technical Research
## Use-What-You-Have Makeup Coach - AI/ML Technologies & Implementation Guide

**Document Version:** 1.0  
**Date:** November 28, 2025  
**Prepared by:** Makeup Coach App Research Team

---

## Executive Summary

This document outlines the technical requirements and technology stack recommendations for building the Use-What-You-Have Makeup Coach application. We evaluate AI/ML frameworks, face detection APIs, skin analysis technologies, and cross-platform development options.

---

## 1. AI/ML Framework Comparison

### 1.1 TensorFlow vs PyTorch

| Criteria | TensorFlow | PyTorch | Recommendation |
|----------|------------|---------|----------------|
| **Learning Curve** | Moderate | Easier | PyTorch for prototyping |
| **Production Deployment** | Excellent | Good | TensorFlow for production |
| **Mobile Support** | TensorFlow Lite | PyTorch Mobile | TensorFlow Lite |
| **Research Adoption** | 30% | 70% | PyTorch for R&D |
| **Cloud Scalability** | Excellent | Good | TensorFlow |
| **Real-time Performance** | Excellent | Good | TensorFlow |
| **Community Size** | Large | Growing Fast | Both strong |

### 1.2 Framework Recommendation

**Hybrid Approach:**
- **PyTorch** for research and model development (faster iteration)
- **TensorFlow/TensorFlow Lite** for production deployment (better mobile optimization)
- **ONNX** for model interoperability between frameworks

### 1.3 Key Tools & Libraries

| Tool | Purpose | Integration |
|------|---------|-------------|
| TensorFlow Lite | Mobile ML inference | iOS/Android |
| TensorFlow.js | Web-based ML | Browser |
| Core ML | iOS-native ML | Apple devices |
| ML Kit | Cross-platform ML | Firebase |
| ONNX Runtime | Model portability | All platforms |

---

## 2. Face Detection & Landmark APIs

### 2.1 API Comparison

| API | Accuracy | Speed | Cost | Offline | Best For |
|-----|----------|-------|------|---------|----------|
| **MediaPipe** | 97.5% | 30ms | Free | Yes | Real-time mobile |
| **Apple ARKit** | 98%+ | 16ms | Free | Yes | iOS apps |
| **Google ML Kit** | 96% | 25ms | Free | Yes | Cross-platform |
| **AWS Rekognition** | 99%+ | 100ms | Pay | No | High accuracy |
| **Azure Face API** | 99%+ | 80ms | Pay | No | Enterprise |
| **Face++** | 98% | 50ms | Freemium | No | China market |

### 2.2 MediaPipe (Recommended for MVP)

**Why MediaPipe:**
- Open-source (Google)
- 468 facial landmarks
- Real-time performance (30ms)
- Works offline
- Cross-platform (iOS, Android, Web)
- Free to use

**Key Features:**
- Face mesh with 468 3D landmarks
- Iris tracking
- Face detection with bounding boxes
- Real-time video processing
- MobileNetV2 backbone (lightweight)

**Implementation:**
```python
import mediapipe as mp
mp_face_mesh = mp.solutions.face_mesh

with mp_face_mesh.FaceMesh(
    max_num_faces=1,
    refine_landmarks=True,
    min_detection_confidence=0.5,
    min_tracking_confidence=0.5
) as face_mesh:
    results = face_mesh.process(rgb_image)
```

### 2.3 Apple ARKit (iOS Enhancement)

**Why ARKit for iOS:**
- Native performance
- TrueDepth camera support
- 52 blendshapes for expression tracking
- Integrated with Core ML
- Best-in-class iOS experience

---

## 3. Skin Tone Analysis Technology

### 3.1 Skin Tone Classification Systems

| Scale | Categories | Use Case | Recommendation |
|-------|------------|----------|----------------|
| **Fitzpatrick Scale** | 6 types | Medical standard | Base classification |
| **Monk Skin Tone Scale** | 10 shades | Google AI standard | Inclusive AI |
| **ITA (Individual Typology Angle)** | Continuous | Scientific measurement | Technical backend |

### 3.2 Google Monk Skin Tone Scale (Recommended)

**Why Monk Scale:**
- Developed for AI/ML applications
- 10 shades (more inclusive than Fitzpatrick)
- Open-licensed by Google
- Designed to reduce AI bias
- Better representation of global skin tones

### 3.3 AI Skin Analysis Solutions

| Solution | Capabilities | Integration | Cost |
|----------|--------------|-------------|------|
| **Haut.AI** | 50+ skin metrics | API | Enterprise |
| **Perfect Corp AI** | Skin diagnostics | SDK | Enterprise |
| **Custom TensorFlow** | Flexible | Self-hosted | Development cost |
| **TrueSkin Dataset** | Training data | Research | Free |

### 3.4 Building Custom Skin Analysis

**Dataset Requirements:**
- Minimum 7,000+ diverse images
- 6-10 skin tone categories
- Various lighting conditions
- Multiple camera angles
- Diverse demographics

**Model Architecture:**
- CNN-based classification
- Transfer learning from MobileNetV2
- Multi-task learning (tone + concerns)
- Confidence scoring

---

## 4. Cross-Platform Development

### 4.1 Framework Comparison

| Framework | Performance | Dev Speed | ML Integration | Recommendation |
|-----------|-------------|-----------|----------------|----------------|
| **React Native** | Good | Fast | TensorFlow.js, ML Kit | Web team |
| **Flutter** | Excellent | Fast | TensorFlow Lite | Best overall |
| **Native (Swift/Kotlin)** | Best | Slow | Core ML, TF Lite | Premium experience |
| **Expo** | Good | Fastest | Limited | Quick MVP |

### 4.2 Recommended Stack: Flutter

**Why Flutter:**
- Single codebase for iOS, Android, Web
- Near-native performance
- Excellent camera integration
- TensorFlow Lite support
- Google's ML Kit integration
- Growing ecosystem

**Key Packages:**
```yaml
dependencies:
  camera: ^0.10.0
  tflite_flutter: ^0.10.0
  google_mlkit_face_detection: ^0.5.0
  image: ^4.0.0
  path_provider: ^2.0.0
```

### 4.3 Alternative: React Native

**Why React Native:**
- JavaScript/TypeScript (larger talent pool)
- Faster onboarding for web developers
- Good ML integration via react-native-tensorflow
- Expo for quick prototyping

---

## 5. Computer Vision Pipeline

### 5.1 Real-Time Processing Pipeline

```
[Camera Input]
    ↓
[Frame Capture (30 FPS)]
    ↓
[Face Detection (MediaPipe)]
    ↓
[Landmark Extraction (468 points)]
    ↓
[Skin Segmentation]
    ↓
[Skin Tone Analysis]
    ↓
[Feature Overlay/Tutorial]
    ↓
[Display Output]
```

### 5.2 Key Processing Features

| Feature | Technology | Purpose |
|---------|------------|--------|
| Face Detection | MediaPipe | Locate face in frame |
| Landmark Tracking | 468-point mesh | Map facial features |
| Skin Segmentation | U-Net / DeepLab | Isolate skin regions |
| Color Analysis | HSV/LAB color space | Determine skin tone |
| Makeup Overlay | OpenGL / Metal | AR visualization |
| Progress Comparison | Image diff algorithms | Before/after |

### 5.3 Performance Targets

| Metric | Target | Minimum |
|--------|--------|----------|
| Frame Rate | 30 FPS | 24 FPS |
| Detection Latency | <50ms | <100ms |
| Model Size | <20MB | <50MB |
| Battery Impact | <5%/hour | <10%/hour |
| Memory Usage | <200MB | <400MB |

---

## 6. Ingredient Database & APIs

### 6.1 Ingredient Data Sources

| Source | Data Type | Access | Use Case |
|--------|-----------|--------|----------|
| **INCI Database** | Ingredient names | Free | Standardization |
| **EWG Skin Deep** | Safety ratings | API | Safety scoring |
| **CosDNA** | Ingredient analysis | Scraping | Comedogenic ratings |
| **Paula's Choice** | Ingredient reviews | Manual | Expert insights |
| **Open Beauty Facts** | Product database | API | Product lookup |

### 6.2 Custom Ingredient Database Schema

```sql
CREATE TABLE ingredients (
    id UUID PRIMARY KEY,
    inci_name VARCHAR(255),
    common_name VARCHAR(255),
    function VARCHAR(100),
    safety_score INT (1-10),
    comedogenic_rating INT (0-5),
    irritation_potential VARCHAR(20),
    suitable_skin_types TEXT[],
    concerns_addressed TEXT[],
    concerns_caused TEXT[]
);
```

---

## 7. Product Recognition

### 7.1 Barcode/QR Scanning

| Library | Platform | Features |
|---------|----------|----------|
| ML Kit Barcode | Cross-platform | Fast, accurate |
| ZXing | Android | Open-source |
| AVFoundation | iOS | Native performance |

### 7.2 Visual Product Recognition

**Approach:** Custom image classification model
- Training data: Product images from major brands
- Architecture: MobileNetV2 transfer learning
- Output: Product category, brand, shade

---

## 8. Technical Architecture Overview

### 8.1 System Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    CLIENT LAYER                         │
├─────────────────────────────────────────────────────────┤
│  Flutter App (iOS/Android)  │  Web App (React/Flutter)  │
├─────────────────────────────────────────────────────────┤
│                    ML LAYER                             │
├─────────────────────────────────────────────────────────┤
│  TensorFlow Lite │ MediaPipe │ Core ML │ ML Kit        │
├─────────────────────────────────────────────────────────┤
│                    API LAYER                            │
├─────────────────────────────────────────────────────────┤
│  REST API │ GraphQL │ WebSocket (real-time)            │
├─────────────────────────────────────────────────────────┤
│                    BACKEND LAYER                        │
├─────────────────────────────────────────────────────────┤
│  Node.js/Python │ PostgreSQL │ Redis │ S3              │
└─────────────────────────────────────────────────────────┘
```

---

## 9. Recommendations Summary

### 9.1 MVP Technology Stack

| Component | Recommended Technology |
|-----------|------------------------|
| Mobile Framework | Flutter |
| Face Detection | MediaPipe Face Mesh |
| ML Framework | TensorFlow Lite |
| Skin Analysis | Custom model + Monk Scale |
| Backend | Node.js + PostgreSQL |
| Cloud | Firebase / AWS |
| CI/CD | GitHub Actions |

### 9.2 Development Phases

**Phase 1 (MVP):**
- MediaPipe face detection
- Basic skin tone classification
- Manual product entry
- Core tutorial system

**Phase 2 (Enhanced):**
- Advanced skin analysis
- Barcode scanning
- AI-powered recommendations
- Progress tracking

**Phase 3 (Advanced):**
- Visual product recognition
- Real-time AR overlay
- Personalized AI coaching
- Community features

---

## References

1. MediaPipe Documentation - Google
2. TensorFlow Lite Guide
3. Google Monk Skin Tone Scale
4. TrueSkin Dataset - arXiv
5. Flutter ML Integration Guide
6. Apple ARKit Face Tracking

---

*This document is part of the pre-development research phase for the Use-What-You-Have Makeup Coach application.*
