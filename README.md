# UE5 GestureRecorder Plugin (v1.1 - MediaPipe Optimized)

**GestureRecorder** is a lightweight, independent Unreal Engine 5 (UE5) plugin designed to track the joint movements of a Character's Skeletal Mesh and record them in real-time as high-quality CSV data for Machine Learning (AI) model training.

This version is **highly optimized for MediaPipe compatibility**. It automatically maps Unreal Engine's complex skeletal structure into MediaPipe's standard landmark naming convention to prevent the "Curse of Dimensionality" during AI training.

## Key Features & v1.1 Updates

* **Dynamic MediaPipe Mapping:** Maps UE5 bone names (e.g., `head`, `upperarm_r`) directly to MediaPipe landmark names (e.g., `nose`, `right_shoulder`).
* **Independent & Lightweight:** Operates using only default UE5 modules without heavy third-party dependencies.
* **Real-time AI Preprocessing:** Extracts relative coordinates using the **Pelvis as the origin (0,0,0)**.
* **Auto-Scale Normalization:** Automatically converts Unreal Engine's default scale (cm) into the AI training standard (m) by applying a 0.01 multiplier.
* **2D & 3D Environment Support:**
  * `UGestureRecorderComponent` (3D): Extracts relative X, Y, Z coordinates.
  * `UGestureRecorderComponent2D` (2D): Maps Unreal's Y and Z axes to 2D X and Y coordinates to seamlessly match webcam and MediaPipe setups.

## Installation

1. Navigate to the `Plugins` folder inside your UE5 project directory. (Create the folder if it does not exist).
2. Copy or clone this repository into the `Plugins` folder (e.g., `Plugins/GestureRecorder/`).
3. Right-click your project's `.uproject` file and select **"Generate Visual Studio project files"**.
4. Open the `.sln` file in Visual Studio and **Build** the project.

## How to Use

### Configuration (Details Panel)
Once attached to a Character Blueprint, you can configure the mapping in the details panel:
* **Root Bone Name:** The reference origin bone (Default: `pelvis`).
* **Target Bones (Mapping Struct):** An array of `FGestureBoneMapping` that links UE5 bones to MediaPipe names. (Pre-configured for standard humanoid rigs).
* **Normalize Scale:** The ratio used for meter conversion (Default: `0.01`).

### Blueprint Nodes
* `StartRecording()`: Begins the data recording process and dynamically generates the CSV header based on the target bones.
* `StopRecording()`: Ends the recording and generates the CSV file in the `Saved/GestureData/` directory.
* `UpdateGestureInfo(String Name, Int Count)`: Real-time update of the current gesture's name and its repetition count (ground truth label).

## Output Data Format (CSV)

The generated CSV files are structured dynamically based on the mapped MediaPipe landmarks, ready to be fed directly into your AI model.

**[Header Example - 2D Version]**
```csv
nose_X,nose_Y,left_shoulder_X,left_shoulder_Y,right_shoulder_X,right_shoulder_Y, ... ,GestureName,GestureCount
0.012345,0.567890,-0.123456,0.654321, ... ,Jump,1
...
