# TinyML-Based Intrusion Detection System for CAN Bus

A lightweight intrusion detection system for CAN bus networks, optimized for deployment on ESP32 microcontrollers using TensorFlow Lite for Microcontrollers (TinyML).

## 📋 Project Overview

This project implements a neural network-based intrusion detection system capable of detecting three types of CAN bus attacks:
- **Attack_Free** (Normal traffic)
- **DoS** (Denial of Service attacks)
- **Fuzzy** (Fuzzing attacks)

The model is trained on a large CAN bus dataset and deployed on resource-constrained ESP32 hardware for real-time attack detection.

## 🎯 Model Performance

- **Accuracy**: ~87-96% (depending on optimization strategy)
- **Model Size**: 48-140 KB (TFLite format)
- **Inference Time**: Real-time on ESP32
- **Features**: 10-25 features extracted from CAN frames

## 📁 Project Structure

```
├── 01_Dataset/                          # Dataset preparation scripts
│   └── dataset_prep.ipynb               # Data preprocessing notebook
│
├── 02_Model/                            # Model training and artifacts
│   ├── 01_IDS_Train_Model.ipynb         # Model training notebook
│   ├── 02_confusion_matrix.png          # Model evaluation visualization
│   └── 03_training_history.png          # Training metrics
│
├── 04_Reciever_ESP32_Code/              # ESP32 firmware
│   └── 04_Reciever_ESP32_Code.ino       # ESP32 intrusion detection code
│
├── 05_Transmitter_ESP8266_Code/         # ESP8266 CAN transmitter
│   └── 05_Transmitter_ESP8266_Code.ino
│
└── hardware_pin_connections.png         # Hardware wiring diagram
```

## 🚀 Getting Started

### Prerequisites

**Hardware:**
- ESP32 DevKit
- ESP8266 (optional - for CAN message transmission)
- CAN transceiver module (MCP2515)

**Software:**
- Python 3.8+
- TensorFlow 2.x
- Arduino IDE
- TensorFlowLite_ESP32 library

### Installation

1. **Clone the repository:**
   ```bash
   git clone https://github.com/akshita24101/TinyML-Based-Intrusion-Detection-and-Battery-Health-for-EVs.git
   cd TinyML-Based-Intrusion-Detection-and-Battery-Health-for-EVs
   ```

2. **Install Python dependencies:**
   ```bash
   pip install tensorflow pandas numpy scikit-learn matplotlib seaborn
   ```

3. **Install Arduino libraries:**
   - TensorFlowLite_ESP32
   - SPI (built-in)

### Training the Model

1. Place your CAN dataset in `01_Dataset/` (CSV format with columns: CAN_ID, DLC, DATA[0-7], Label)
2. Run the training notebook: `02_Model/01_IDS_Train_Model.ipynb`
3. The model will be saved as TFLite and converted to C header file

### Deploying to ESP32

1. Train the model using the notebook to generate `model_data.h` (C header file with TFLite model)
2. Copy `model_data.h` to `04_Reciever_ESP32_Code/` folder
3. Open `04_Reciever_ESP32_Code.ino` in Arduino IDE
4. Select ESP32 board and upload
5. Send CAN data via Serial Monitor (115200 baud) in format: `CAN_ID(decimal),DLC,DATA[0-7](hex)`

Example input:
```
0165000,8,0e,e8,7f,00,00,00,07,9e
```

The ESP32 will respond with the detected attack type and confidence score.

## 📊 Model Architecture

- **Input**: 10-25 features from CAN frames
- **Architecture**: 128→64→32 or 256→128→64→32 (optimized versions)
- **Output**: 3 classes (Attack_Free, DoS, Fuzzy)
- **Regularization**: L2, Dropout, BatchNormalization
- **Optimization**: Class weights for imbalanced data

## 🔬 Features Used

**Basic Features (10):**
- CAN_ID (numeric)
- DLC (Data Length Code)
- DATA[0-7] (8 data bytes)

**Enhanced Features (15 additional):**
- Statistical: mean, std, max, min, sum, range, variance
- Pattern detection: zero_count, byte differences
- Signature bytes: first_byte, last_byte

## 📈 Results

### Classification Report (87% Model)
```
              precision    recall  f1-score   support

 Attack_Free       0.89      0.96      0.92    450000
         DoS       0.91      0.68      0.78    135000
       Fuzzy       0.76      0.73      0.74    117000

    accuracy                           0.87    702000
```

### Optimized Model Performance
- Attack_Free: 98%+ accuracy
- DoS: 85-90% accuracy
- Fuzzy: 85-90% accuracy
- Overall: 92-96% accuracy

## 🛠️ Hardware Setup

Refer to `hardware_pin_connections.png` for detailed wiring diagram.

**ESP32 Pin Connections:**
- GPIO 5 → MCP2515 CS
- GPIO 18 → MCP2515 SCK
- GPIO 19 → MCP2515 MISO
- GPIO 23 → MCP2515 MOSI

## 📝 Usage Example

```python
# Send CAN data to ESP32 via serial
# Format: CAN_ID(decimal),DLC,D0,D1,D2,D3,D4,D5,D6,D7(all hex)

# Normal traffic example
0165000,8,0e,e8,7f,00,00,00,07,9e

# ESP32 Response:
# Prediction: Attack_Free
# Confidence: 99.98%
# Inference time: 12ms
```

## 🤝 Contributing

Contributions are welcome! Please feel free to submit issues or pull requests.

## 📄 License

This project is open-source and available under the MIT License.

## 👥 Authors

- Akshita - [akshita24101](https://github.com/akshita24101)
- Ekansh - [exorev07](https://github.com/exorev07)

## 🙏 Acknowledgments

- CAN bus dataset providers
- TensorFlow Lite for Microcontrollers team
- ESP32 community

## 📧 Contact

For questions or collaborations, please open an issue or contact the authors.

---

**Note:** The following files are excluded from this repository (see `.gitignore`):
- Dataset files (`.csv`) - Too large for GitHub (206+ MB)
- Model files (`.h5`, `.keras`, `.tflite`) - Large binary files
- `model_data.h` - Generated C header file with TFLite model (~49 KB)

Please train the model using your own CAN bus dataset to generate these files.
