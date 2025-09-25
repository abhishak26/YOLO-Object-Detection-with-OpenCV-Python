# YOLO Object Color Detection

This repository contains the necessary setup and a test script for performing object detection using YOLOv8 with OpenCV and Python. The project is designed to help you get started with real-time object detection using your webcam.

## Table of Contents
- [Requirements](#requirements)
- [Installation](#installation)
  - [macOS](#macos)
  - [Windows](#windows)
- [Setup Virtual Environment](#setup-virtual-environment)
- [Install Dependencies](#install-dependencies)
- [Running the Test Script](#running-the-test-script)
- [Troubleshooting](#troubleshooting)

---

## Requirements

* **Python 3.10.\*** (Note: Python 3.11 may not be fully supported; 3.10.10 is recommended if you encounter issues).
* Visual Studio Code (recommended IDE)
* Webcam

---

## Installation

### macOS

1.  **Open Terminal**.
2.  **Check Python Version**:
    ```bash
    python3 --version
    ```
    If you see "Python 3.10.\*", you're good. If "command not found" or an unsupported version, proceed with installation.

3.  **Install Homebrew**:
    If Homebrew isn't installed, run:
    ```bash
    /bin/bash -c "$(curl -fsSL [https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh](https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh))”
    ```
    Follow the prompts, entering your Mac password if requested.
    Then, configure Homebrew:
    ```bash
    echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
    eval "$(/opt/homebrew/bin/brew shellenv)"
    ```
    Verify Homebrew:
    ```bash
    brew -v
    ```
    You should see "Homebrew 4.x.x".

4.  **Install Python (via Homebrew)**:
    ```bash
    brew install python
    ```
    Verify Python:
    ```bash
    python3 --version
    ```
    You should see "Python 3.*.*".

5.  **Install pip**:
    ```bash
    python3 -m ensurepip --upgrade
    ```
    Verify pip:
    ```bash
    pip3 --version
    ```
    You should see "pip 24.x from /opt/homebrew/lib/python3.12/site-packages".

---

### Windows

1.  **Open Command Prompt** or **PowerShell**.
2.  **Check Python Version**:
    ```bash
    python --version
    ```
    If you see "Python 3.10.\*", you're good. If "command not found" or an unsupported version (e.g., 3.11), proceed with installation of Python 3.10.10.

3.  **Install Python 3.10.10**:
    * Go to the official Python website: [https://www.python.org/downloads/](https://www.python.org/downloads/)
    * Click on "Download for Windows". For Python 3.10.10 specifically, you might need to navigate to "Looking for a specific release?" and find 3.10.10.
    * Open the downloaded setup file. **Crucially, ensure you check "Add Python X.X to PATH" during installation.**
    * Follow the on-screen instructions.

4.  **Install Visual Studio Code**:
    * Go to: [https://code.visualstudio.com](https://code.visualstudio.com)
    * Download and install for Windows.

---

## Setup Virtual Environment

A virtual environment is crucial for managing project dependencies.

1.  **Create Project Folder**:
    * Create a new folder named `yolo-unity` on your computer disk.

2.  **Open Project in Visual Studio Code**:
    * Open Visual Studio Code.
    * Go to `File` > `Open Folder` and select the `yolo-unity` folder you just created.

3.  **Open Terminal in VS Code**:
    * In Visual Studio Code, go to `Terminal` > `New Terminal`.

4.  **Create Virtual Environment**:
    * In the VS Code terminal, run:
        ```bash
        python -m venv venv
        ```
        (On macOS, use `python3 -m venv venv`)
    This will create a `venv` folder within your `yolo-unity` directory.

5.  **Activate Virtual Environment**:
    * **macOS**:
        ```bash
        source venv/bin/activate
        ```
    * **Windows**:
        ```bash
        .\venv\Scripts\activate
        ```
        * **If you get an error like "cannot be loaded because running scripts is disabled on this system":**
            * Open **Windows PowerShell** as **Administrator** (Right-click > Run as Administrator).
            * Run the command:
                ```powershell
                Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
                ```
            * Type `A` (for Yes to All) and press Enter.
            * Go back to your Visual Studio Code terminal and try activating again:
                ```bash
                .\venv\Scripts\activate
                ```

    You should now see `(venv)` before your terminal prompt, indicating the virtual environment is active.

---

## Install Dependencies

While your virtual environment is active (you see `(venv)` in the terminal):

1.  **Upgrade pip**:
    ```bash
    pip install --upgrade pip
    ```
    (On Windows, you might use `python.exe -m pip install --upgrade pip`)

2.  **Create `requirements.txt`**:
    Create a file named `requirements.txt` in your `yolo-unity` folder and add the following content:
    ```
    opencv-python
    ultralytics
    numpy<2
    ```

3.  **Install packages from `requirements.txt`**:
    ```bash
    pip install -r requirements.txt
    ```

---

## Running the Test Script

1.  **Create `test_yolo.py`**:
    * In Visual Studio Code, right-click on the `yolo-unity` folder in the Explorer sidebar.
    * Select `New File` and name it `test_yolo.py`.
    * Paste the following code into `test_yolo.py`:

    ```python
    from ultralytics import YOLO
    import cv2

    # Load YOLOv8 model (Nano version - fastest)
    model = YOLO('yolov8n.pt')

    # Open webcam (0 = default webcam)
    cap = cv2.VideoCapture(0)

    while True:
        ret, frame = cap.read()
        if not ret:
            break

        # Run YOLO detection
        results = model(frame)

        # Draw detections on frame
        annotated_frame = results[0].plot()

        # Show frame
        cv2.imshow("YOLO Detection", annotated_frame)

        # Exit when 'q' is pressed
        if cv2.waitKey(1) & 0xFF == ord('q'):
            break

    cap.release()
    cv2.destroyAllWindows()
    ```

2.  **Save the file** (Ctrl+S or Command+S).

3.  **Run the script**:
    * In your VS Code terminal (with `(venv)` active), run:
        ```bash
        python test_yolo.py
        ```
    * A webcam window should pop up, displaying your webcam feed with real-time object detection (e.g., person, bottle, cup).

4.  **To quit**, click on the webcam window and press the `Q` key, or press `Ctrl + C` in the terminal.

---

## Troubleshooting

* **`numpy` version conflict**:
    If you encounter errors related to `numpy`, ensure you have a compatible version. The `requirements.txt` specifies `numpy<2`. You can explicitly check and install:
    ```bash
    pip install "numpy<2"
    ```
    Confirm the installed version:
    ```bash
    python -c "import numpy; print(numpy.__version__)"
    ```
    It should show a version like `1.26.x`. Then try running `python test_yolo.py` again.
