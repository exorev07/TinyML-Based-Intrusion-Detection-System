**TinyML-based Intrusion Detection System for Electric Vehicles**

📌**Project Overview**

This project presents an offline, low cost, real-time Intrusion Detection System (IDS) designed 
to secure the Controller Area Network (CAN) bus in Electric Vehicles. 
By using TinyML, the system identifies cyber-attacks such as DoS and Fuzzy attacks directly on resource-constrained edge hardware.

🛠️**Technical Implementation**

1. Hardware Testbed: A realistic bus emulation environment was engineered
   using ESP8266 nodes for data streaming and an ESP32 for real-time detection.
2. Data Engineering: The pipeline processed a dataset of 3.5 million CAN frames,
   transforming raw telemetry into structured, multi-ID frames optimized for on-device inference.
3. Edge AI Framework: The project utilized TensorFlow Lite(TFLite)
   to deploy a quantization-friendly MLP model, effectively managing embedded memory and latency constraints.


📊 **Performance & Metrics**

1. XGBoost Benchmark: Achieved approximately 92.4% test accuracy during initial model validation.
2. On-Device Accuracy: The optimized MLP model maintained 84.12% accuracy while running in real-time on the ESP32 hardware.
3. Operational Efficiency: The implementation ensures low-latency detection with a minimal hardware footprint, suitable for automotive environments.
