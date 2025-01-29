# 🚗 Driver-Safety-Guard: AI-Powered Driver Monitoring System 🛡️

## 🔍 Overview
**Driver-Safety-Guard** is an **AI-powered driver monitoring system** that detects distracted driving behaviors using a **YOLOv8 model** and integrates with an **ESP32 microcontroller** to provide real-time alerts via LEDs and a buzzer. This project enhances road safety by identifying signs of **drowsiness, mobile phone usage, smoking, and other distractions.**

---

## 🎯 Key Features
✅ **Real-Time Detection** - Detects driver distractions using a camera and YOLOv8.  
✅ **ESP32 Integration** - Controls LEDs and a buzzer via HTTP requests.  
✅ **Multiple Alert Mechanisms** - Provides **visual and sound alerts** to warn drivers.  
✅ **Customizable Model** - Train the YOLOv8 model on new datasets for better accuracy.  
✅ **Cloud & Edge Deployment** - Works on local machines and can be adapted for cloud processing.  

---

## 🛠️ Hardware & Software Requirements
### 🔌 **Hardware**
- **ESP32** Development Board
- **Webcam / Camera Module**
- **LEDs (Green, Red, Orange, Blue)**
- **Buzzer**
- **Power Supply (ESP32-compatible)**

### 🖥 **Software**
- **Python 3.11+**
- **OpenCV** (for image processing)
- **Ultralytics YOLOv8** (for object detection)
- **ESP32 Web Server** (for communication)
- **Google Colab** (for training the model)

---

## 📂 Project Structure
```
📂 Driver-Safety-Guard/
 ├── 📁 Model_Training/
 │    ├── Main_Model_Training.ipynb  # Colab Notebook for YOLOv8 training
 │    ├── data.yaml  # Dataset configuration
 │    ├── train/  # Trained model weights
 ├── 📁 ESP32/
 │    ├── Main_Arduino_Code.ino  # ESP32 firmware for LED & Buzzer alerts
 ├── 📁 Model_Run/
 │    ├── Model_Run.py  # Runs the detection and communicates with ESP32
 ├── 📁 Media/
 │    ├── images/  # Sample detection images
 │    ├── presentation.pdf  # Full project presentation
 ├── README.md  # Project documentation
```

---

## 🏋️‍♂️ Model Training (YOLOv8)
### 1️⃣ **Mount Google Drive & Extract Dataset**
```python
from google.colab import drive
drive.mount('/content/drive')
```
```python
import zipfile, os
zip_file_path = "/content/drive/MyDrive/dataset.zip"
extracted_dir_path = "/content/extracted"
os.makedirs(extracted_dir_path, exist_ok=True)
with zipfile.ZipFile(zip_file_path, 'r') as zip_ref:
    zip_ref.extractall(extracted_dir_path)
```

### 2️⃣ **Train the Model**
```sh
!yolo task=detect mode=train model=yolov8m.pt data=data.yaml epochs=100 imgsz=640 plots=True
```

### 3️⃣ **Save the Trained Model**
```python
import shutil
source_path = "/content/extracted/runs/detect/train"
destination_path = "/content/drive/MyDrive/trained_model"
shutil.copytree(source_path, destination_path, dirs_exist_ok=True)
print("Training results saved.")
```

---

## 🔌 ESP32 Setup & Communication
The **ESP32** acts as a **web server**, receiving commands via HTTP requests from the AI model.

### **1️⃣ ESP32 Web Server Setup**
- Connect **ESP32 to WiFi**.
- Define endpoints (`/distracted`, `/drowsy`, `/mobileuse`, `/smoking`, `/off`).
- Control **LEDs & Buzzer** based on detected behavior.

### **2️⃣ ESP32 HTTP Endpoint Example**
```cpp
server.on("/distracted", HTTP_GET, [](){
    digitalWrite(greenLED, HIGH);
    tone(buzzer, 1000, 1000);
    server.send(200, "text/plain", "Distracted: Green LED On with Buzzer Beep");
});
```

---

## 🚀 Running the Model & ESP32 Integration
1️⃣ **Run the Detection Script** (Model_Run.py)
```sh
python Model_Run.py
```
2️⃣ **ESP32 will Respond to Detected States**
- **Drowsy → Red LED + Buzzer**
- **Mobile Use → Orange LED + Buzzer**
- **Smoking → Blue LED + Buzzer**
- **Distracted → Green LED + Buzzer**

---

## 📸 Sample Output
| Scenario | Detection Preview |
|----------|------------------|
| Drowsy Driving | ![Drowsy](https://github.com/user-attachments/assets/88d3ede5-74a6-4ce3-86a8-f3b24c1ce75a)
/drowsy.jpg) |
| Head Drop | ![Drowsy](Media/images/drowsy.jpg) |
| Mobile Usage | ![Mobile](Media/images/mobile.jpg) |
| Smoking | ![Smoking](Media/images/smoking.jpg) |

---

## 📜 License
This project is open-source and licensed under **MIT License**.

📌 **Project Presentation**: [View Full PDF](https://drive.google.com/file/d/1MdnVrYmAUnjPG4GHgdLDd13kZDKytUOF/view)  

🚀 **Stay Focused, Stay Safe!** 🚗💡

