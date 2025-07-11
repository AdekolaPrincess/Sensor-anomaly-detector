# LSTM-Based Sensor Anomaly Detector

Detecting potential machine faults using real-time sensor data with deep learning.

---

## 1.  Project Objective

This project aims to build a **supervised LSTM model** that detects **anomalies** in machine sensor readings, which may indicate unusual behavior or early signs of failure. By identifying faulty conditions early, maintenance can be scheduled proactively, improving safety and operational efficiency.

---

## 2.  Dataset

**Source:** [Machine Failure Prediction using Sensor Data (Kaggle)](https://www.kaggle.com/datasets/umerrtx/machine-failure-prediction-using-sensor-data/data)

The dataset consists of readings from various sensors installed on industrial machines, with a binary `fail` label:

- `footfall`: Number of people or objects passing by the machine  
- `tempMode`: Machine temperature setting  
- `AQ`: Air Quality index  
- `USS`: Ultrasonic sensor reading  
- `CS`: Current sensor (electrical usage)  
- `VOC`: Volatile Organic Compound levels  
- `RP`: Rotational position or RPM  
- `IP`: Input pressure  
- `Temperature`: Operating temperature  
- `fail`: Binary failure indicator (0 = normal, 1 = failure)

---

## 3.  Data Preprocessing & Cleaning

- Verified dataset integrity: **No missing values**
- Performed **exploratory data analysis (EDA)** to understand sensor behavior
- Split data into **train and test** early to simulate real-world conditions
- Applied **outlier treatment** only on training data  
    - Compared sensor columns to `fail` to identify **meaningful outliers**
    - Preserved outliers (e.g., extreme temperatures) that are tied to actual failures
- Applied **Yeo-Johnson transformation** to correct skewness  
    - Used same fitted transformer on test data to avoid data leakage
- Applied **median filtering** to reduce noise in training data only  
    - Prevents model confusion from random jitter, keeps real anomaly signals intact
- Scaled both train and test sets to bring features to a uniform scale

---

## 4.  Sequence Generation (Sliding Window)

LSTM networks require time-sequential input.  
To achieve this, flat tabular data was transformed into **overlapping windows of 10 time steps** using a sliding window method.

> This helps the model learn time-based dependencies between sensor readings and machine behavior.

---

## 5.  Model Architecture

A **supervised LSTM binary classifier** was built using Keras Functional API:


Input → LSTM(64) → Dropout(0.3) → LSTM(32) → Dropout(0.3) → Dense(1, activation='sigmoid')

--- 

## 6.  What is LSTM?

**LSTM (Long Short-Term Memory)** is a type of Recurrent Neural Network (RNN) that learns patterns over time.  
It is ideal for time-series problems because it remembers past inputs to make better decisions at each step.

> In this project, LSTM helps detect evolving sensor patterns that lead to machine failure.

---

## 7.  Model Performance

**Training Summary (after early stopping at 10 epochs):**
**Train Accuracy:** 88.0%
**Validation Accuracy:** 88.4%
**Validation Loss:** 0.3043
**Test Accuracy:** 87.0%



However, for anomaly detection, **precision and recall** are more important than plain accuracy:

| Label | Precision | Recall | F1-Score | Support |
|-------|-----------|--------|----------|---------|
| **0 (Normal)** | 0.86 | 0.88 | 0.87 | 94 |
| **1 (Failure)** | 0.87 | 0.85 | 0.86 | 86 |

>  This balanced performance confirms the model is reliable in detecting both normal and abnormal behavior.

---

## 8.  Applications
This model has real-world applications across industrial, manufacturing, and predictive maintenance domains:

- Factory Automation: Detect early signs of machinery wear or overload

- Predictive Maintenance: Reduce unplanned downtime by acting before failure

- IoT Systems: Embedded in edge devices to monitor sensor streams in real-time

- Smart Monitoring Dashboards: Integrate with visualization tools for real-time alerts

- Anomaly Surveillance: Used in health, energy, and robotics for time-based fault detection

## 9.  Contact
Made by Adekola Princess
Contact: adekolaprincess02@gmail.com
