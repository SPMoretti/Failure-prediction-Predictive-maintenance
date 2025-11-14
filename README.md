# 🧠 Real-Time Monitoring and Anomaly Detection for Turbomachinery Data

## 📋 Overview
This Python application performs **real-time condition monitoring** and **anomaly detection** on sensor data from a **turbomachine**.

It is designed to process high-frequency operational signals—like vibration, temperature —captured during the turbomachine’s operation.  
Using a **data-driven statistical approach**, the system models the machine’s *normal behavior* and continuously monitors new data to detect potential **faults, abnormal transients, or efficiency losses**.

---

## ⚙️ Main Features

- 📥 **Reads sensor data** directly from an Excel file.
- 🧹 **Preprocesses data** by selecting relevant variables and scaling them using `StandardScaler`.
- 🧭 **Computes a centroid** representing the normal operating condition of the turbomachine.
- 🧠 **Performs real-time monitoring** with:
  - A **moving window** of recent samples.
  - **Dynamic centroid updates** every *X* samples.
  - **Adaptive thresholds** based on distance statistics.
- 🚨 **Detects anomalies** by measuring deviations from the centroid in real time.
- 📈 **Visualizes** normal and anomalous data points over time with threshold indication.
- 💾 **Exports results** for further analysis and visualization.

---

## 🧩 Methodology

### 1. **Data Input**
The application reads operational variables from an Excel file containing time-series data of a turbomachine.

The first 1200 samples (by default) are used to define the initial “normal behavior window”.

---

### 2. **Preprocessing**
Selected columns are extracted and standardized using `StandardScaler`, transforming all signals into comparable numerical ranges.  
Missing or incomplete samples are automatically removed.

---

### 3. **Centroid Calculation**
A **centroid** is computed as the mean vector of all signals in the initial window.  
This centroid represents the machine’s normal state in a multi-dimensional feature space.

Each new data point is later compared to this centroid to assess how much it deviates from expected behavior.

---

### 4. **Real-Time Monitoring and Anomaly Detection**
The algorithm continuously receives new sensor readings and performs the following steps for each new sample:

1. **Standardize** the new sample using the same scaler.
2. **Compute the Euclidean distance** between the new sample and the centroid.
3. **Compare the distance** to a dynamic threshold calculated from the 99th percentile of recent distances.
4. **Flag anomalies**:
   - If the distance exceeds the threshold, an anomaly counter increases.
   - After several consecutive anomalies (e.g., 10), a **confirmed anomaly** is reported.
5. **Periodically update** the centroid and threshold after *X* samples to adapt to gradual system changes.

---
