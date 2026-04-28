# UE5 GestureRecorder Plugin

[cite_start]**GestureRecorder** is a lightweight, independent Unreal Engine 5 (UE5) plugin designed to track the joint movements of a Character's Skeletal Mesh and record them in real-time as high-quality CSV data for Machine Learning (AI) model training[cite: 8].

[cite_start]This plugin is highly optimized for the Data Collection phase of an AI gesture recognition pipeline (MediaPipe -> OSC -> UE5 -> ONNX)[cite: 3, 4].

## Key Features

* **Independent & Lightweight:** Operates using only default UE5 modules without heavy third-party dependencies (e.g., OSC or NNE), making it easily portable to any project.
* [cite_start]**Real-time AI Preprocessing:** Extracts relative 3D coordinates using the **Pelvis as the origin (0,0,0)**, rather than relying on screen pixels or absolute world coordinates[cite: 6].
* [cite_start]**Auto-Scale Normalization:** Automatically converts Unreal Engine's default scale (cm) into the AI training standard (m) by applying a 0.01 multiplier[cite: 6].
* **2D & 3D Support:**
  * `UGestureRecorderComponent`: Extracts 3D space coordinates (X, Y, Z).
  * `UGestureRecorderComponent2D`: Maps Unreal's Y and Z axes to 2D X and Y coordinates to seamlessly match webcam and MediaPipe environments.
* [cite_start]**Auto-Labeling & File Management:** Automatically appends the `GestureName` (string) and `GestureCount` (integer) to the end of the CSV header for easy data labeling[cite: 11]. [cite_start]Safely saves the recorded data to an absolute path creating a unique, timestamp-based filename (e.g., `GestureData_20260427_143000.csv`)[cite: 11].

## Installation

1. Navigate to the `Plugins` folder inside your UE5 project directory. (Create the folder if it does not exist).
2. Copy or clone this repository into the `Plugins` folder (e.g., `Plugins/GestureRecorder/`).
3. Right-click your project's `.uproject` file and select **"Generate Visual Studio project files"**.
4. Open the `.sln` file in Visual Studio and **Build** the project, or simply launch the Unreal Editor to compile the plugin via Live Coding.

## How to Use

### 1. Attaching the Component
* Open the Character Blueprint you want to record data from.
* Click the `+ Add` button and attach the **Gesture Recorder Component** (or the 2D version).
* [cite_start]On `BeginPlay`, the component will automatically find and assign the parent actor's `SkeletalMeshComponent` as the target[cite: 9].

### 2. Configuration (Details Panel)
* [cite_start]**Root Bone Name:** The reference origin bone (Default: `pelvis`)[cite: 10].
* [cite_start]**Target Bone Names:** An array of bones to extract coordinates from (e.g., `upperarm_r`, `lowerarm_r`, `hand_r`, `head`)[cite: 7].
* **Normalize Scale:** The ratio used for meter conversion (Default: `0.01`).
* **Record Interval:** The frame rate for recording (Default: `0.0333` / approx. 30 FPS).
* **Base File Name:** The prefix for the output CSV file (e.g., `GestureData`).

### 3. Blueprint Nodes
* `StartRecording()`: Begins the data recording process.
* `StopRecording()`: Ends the recording and generates the CSV file in the `Saved/GestureData/` directory.
* `UpdateGestureInfo(String Name, Int Count)`: Real-time update of the current gesture's name and its repetition count (ground truth label).

## Output Data Format (CSV)

The generated CSV files are structured and ready to be used directly by AI engineers for ONNX model training.

**[Header Example - 3D Version]**
```csv
shoulder_r_X,shoulder_r_Y,shoulder_r_Z,elbow_r_X,elbow_r_Y,elbow_r_Z,hand_r_X,hand_r_Y,hand_r_Z,head_X,head_Y,head_Z,GestureName,GestureCount
0.123456,-0.234567,0.345678, ... ,Jump,1
...
