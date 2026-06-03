# 🚀 Distributed ROI-Based Intelligent Video Analytics Platform

## 📌 Project Overview

The Distributed ROI-Based Intelligent Video Analytics Platform is a scalable, event-driven surveillance and monitoring solution designed to analyze multiple video streams in real time. The system combines Deep Learning, Distributed Messaging, Edge Video Processing, and Region-of-Interest (ROI) Analytics to deliver accurate, low-latency, and resource-efficient monitoring across diverse environments.

The platform leverages custom-trained YOLOv7 Object Detection models, YOLOv7 Instance Segmentation, RabbitMQ message queues, Redis in-memory storage, and OpenCV-based video processing pipelines to identify and analyze critical events from multiple camera feeds.

Unlike traditional surveillance systems that process every detected object indiscriminately, this architecture introduces intelligent ROI validation, ensuring that only detections occurring within predefined operational zones are considered valid. This significantly reduces false positives, minimizes storage consumption, and improves event relevance.

---

# 🎯 Business Problem

Modern surveillance infrastructures generate enormous amounts of video data. Conventional video analytics systems often process every frame and every detection regardless of contextual importance.

This approach creates several challenges:

* Excessive computational overhead
* High storage requirements
* Increased false alarms
* Inefficient resource utilization
* Difficulty scaling across multiple camera streams

Organizations operating industrial facilities, manufacturing plants, smart cities, and transportation systems require a more intelligent mechanism for processing only meaningful events.

This project addresses these challenges through:

* Region-Aware Detection
* Event-Driven Architecture
* Distributed Processing
* Real-Time Analytics

---

# 🎯 Project Goals

### Primary Objectives

* Process multiple live camera streams simultaneously.
* Perform real-time object detection using a custom-trained YOLOv7 model.
* Execute person segmentation using YOLOv7 Segmentation.
* Apply dynamic ROI-based validation.
* Reduce false detections and unnecessary alerts.
* Enable distributed communication using RabbitMQ.
* Store validated events in Redis for high-speed retrieval.
* Support horizontal scalability for enterprise deployments.

---

# 🧠 AI Model Development Lifecycle

## Dataset Engineering

A custom dataset was curated from multiple surveillance scenarios covering industrial, traffic, and workplace safety environments.

### Target Classes

| Class ID | Object Class |
| -------- | ------------ |
| 0        | Person       |
| 1        | Car          |
| 2        | Bike         |
| 3        | Helmet       |
| 4        | Jacket       |
| 5        | Fire         |
| 6        | Smoke        |

---

## Data Annotation

Images were annotated using LabelImg.

Annotation Process:

* Bounding Box Creation
* Class Assignment
* Quality Validation
* Dataset Balancing

The final dataset was divided into:

* Training Set
* Validation Set
* Test Set

---

## Model Training

A custom YOLOv7 architecture was trained using transfer learning techniques.

### Training Features

* Data Augmentation
* Mosaic Augmentation
* Random Scaling
* Random Flipping
* Hyperparameter Optimization

### Training Output

The best-performing weights file:

best.pt

was selected based on:

* mAP Score
* Precision
* Recall
* Validation Loss

---

# 🏗️ System Architecture

## High-Level Architecture

Camera Streams
│
▼
Frame Producer Layer
│
▼
RabbitMQ Exchange
│
┌──────┴─────────┐
│                │
▼                ▼

Person Queue   Object Queue

│                │
▼                ▼

YOLOv7 Seg    YOLOv7 Detect

│                │
└──────┬─────────┘
▼

Detection Exchange

```
    │
```

┌──────┼──────────┬─────────┐
▼      ▼          ▼

Vehicle Fire     PPE
Queue   Queue    Queue

```
    │
    ▼
```

ROI Validation Layer

```
    │
    ▼
```

Redis Event Store

---

# ⚙️ Core Components

## 1. Camera Configuration Manager

Responsible for:

* Camera registration
* Stream initialization
* Metadata management
* Dynamic camera updates

Configuration includes:

* Camera ID
* Stream URL
* Location
* Detection Labels
* ROI Coordinates

---

## 2. Redis Configuration Layer

Redis serves as:

### Configuration Cache

Stores:

* Camera configurations
* ROI settings
* Processing rules

### Event Storage

Stores:

* Detection results
* Segmentation outputs
* Alert history

Benefits:

* Low latency retrieval
* Reduced disk I/O
* Centralized management

---

## 3. Multi-Camera Processing Engine

The system supports concurrent processing using:

### Multiprocessing

Distributes cameras across CPU cores.

### Multithreading

Handles frame acquisition independently.

Advantages:

* High throughput
* Better CPU utilization
* Improved scalability

---

# 📹 Frame Acquisition Pipeline

Each camera continuously streams video frames.

Processing Steps:

1. Capture Frame
2. Resize Frame
3. Encode Frame
4. Publish to RabbitMQ

The encoding stage converts frames into Base64 format for seamless message transportation across services.

---

# 📨 Distributed Messaging System

RabbitMQ acts as the backbone of inter-service communication.

### Benefits

* Decoupled Services
* Fault Tolerance
* Load Balancing
* Asynchronous Processing
* Horizontal Scaling

Each processing module operates independently while communicating through dedicated exchanges and queues.

---

# 🤖 Object Detection Pipeline

The Object Detection Service consumes frames from the Object Queue.

### Detection Workflow

Frame Received
→ Decode Frame
→ Preprocess
→ YOLOv7 Inference
→ Post Processing
→ Publish Results

Outputs:

* Class Label
* Bounding Box
* Confidence Score
* Timestamp

Supported Classes:

* Car
* Bike
* Helmet
* Jacket
* Fire
* Smoke

---

# 👤 Person Segmentation Pipeline

Person-related frames are routed to the Segmentation Service.

### Segmentation Workflow

Frame Received
→ YOLOv7 Segmentation
→ Mask Generation
→ ROI Mapping
→ Event Generation

Outputs:

* Person Mask
* Segmented Region
* Pixel Coordinates
* Detection Confidence

---

# 🔄 Intelligent Detection Routing

After inference, detections are dynamically routed.

### Vehicle Analytics Queue

Monitors:

* Cars
* Bikes

Applications:

* Traffic Monitoring
* Parking Analytics
* Restricted Zone Detection

---

### Fire & Smoke Queue

Monitors:

* Fire
* Smoke

Applications:

* Industrial Safety
* Hazard Detection
* Emergency Response

---

### PPE Compliance Queue

Monitors:

* Helmet
* Jacket

Applications:

* Construction Sites
* Manufacturing Plants
* Safety Compliance Audits

---

# 📍 ROI-Based Event Validation

ROI validation is the most critical layer of the platform.

Instead of storing every detection, the system evaluates whether an object's centroid falls within a predefined operational region.

### Validation Logic

Detection Generated
│
▼
Check ROI Boundary
│
┌──────┴──────┐
│             │
▼             ▼

Inside ROI   Outside ROI

│             │
▼             ▼

Store Event  Ignore Event

---

## Benefits

### Accuracy Improvement

Only relevant detections are processed.

### Storage Optimization

Reduces unnecessary event storage.

### Alert Reliability

Minimizes false alarms.

### Resource Efficiency

Reduces CPU and memory utilization.

---

# 💾 Event Storage Architecture

Validated events are stored in Redis.

Each event contains:

* Event ID
* Camera ID
* Timestamp
* Detection Type
* Confidence Score
* ROI Status
* Bounding Box
* Encoded Snapshot

---

# 📊 Analytics Modules

## Vehicle Analytics

Features:

* Vehicle Detection
* Vehicle Classification
* Zone Monitoring

---

## Fire Analytics

Features:

* Fire Detection
* Smoke Detection
* Early Warning Generation

---

## PPE Compliance Analytics

Features:

* Helmet Monitoring
* Safety Jacket Monitoring
* Compliance Reporting

---

# 🚀 Scalability Features

The architecture supports horizontal scaling through:

* Additional RabbitMQ Consumers
* Distributed Detection Services
* Multi-GPU Inference Nodes
* Kubernetes Deployment

The system can scale from:

Single Camera → Enterprise Surveillance Networks

without architectural changes.

---

# 🛠 Technology Stack

| Technology          | Purpose                    |
| ------------------- | -------------------------- |
| Python              | Backend Development        |
| OpenCV              | Video Processing           |
| YOLOv7              | Object Detection           |
| YOLOv7 Segmentation | Person Segmentation        |
| RabbitMQ            | Message Broker             |
| Redis               | Event Storage & Cache      |
| LabelImg            | Annotation Tool            |
| Multiprocessing     | Parallel Execution         |
| Multithreading      | Concurrent Stream Handling |

---

# 🌍 Real-World Use Cases

### Industrial Safety Monitoring

* Helmet Compliance
* Safety Jacket Compliance
* Worker Presence Detection

### Smart Surveillance

* Intrusion Detection
* Restricted Zone Monitoring
* Human Activity Analysis

### Traffic Analytics

* Vehicle Monitoring
* Traffic Density Analysis
* Parking Management

### Fire Safety Systems

* Fire Detection
* Smoke Monitoring
* Early Incident Response

---

# 🔮 Future Roadmap

Planned Enhancements:

* DeepSORT Multi-Object Tracking
* Vehicle Counting
* Crowd Analytics
* Real-Time Dashboard
* Grafana Monitoring
* Docker Containerization
* Kubernetes Orchestration
* Edge Deployment
* Cloud Integration (AWS/Azure/GCP)
* AI-Based Alert Prioritization

---

# 🏆 Key Achievements

* Built a fully distributed video analytics pipeline.
* Trained custom YOLOv7 models for domain-specific detection.
* Implemented ROI-aware event filtering.
* Reduced false detections significantly.
* Designed a scalable RabbitMQ-driven architecture.
* Enabled real-time processing of multiple camera streams.
* Developed modular microservice-style processing components.

---

# 🎯 Conclusion

This project demonstrates a production-grade Intelligent Video Analytics Platform capable of handling large-scale surveillance workloads through distributed processing and AI-powered event detection.

By integrating custom YOLOv7 models, RabbitMQ messaging infrastructure, Redis-based storage, and ROI-aware filtering mechanisms, the platform delivers accurate, scalable, and resource-efficient monitoring suitable for industrial safety, smart city deployments, traffic analytics, and security surveillance.

The architecture emphasizes modularity, scalability, and operational efficiency, making it adaptable for both edge and cloud-based deployments.
