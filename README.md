# Nainikha AI Privacy Shield
### Your AI-Powered Privacy Layer for Video Calls

**Protect what appears on your screen. Keep your private information private.**

Nainikha AI Privacy Shield is a computer vision project designed to help protect sensitive visual information during video calls and live camera streaming. It uses **YOLO object detection, OpenCV image processing, and a Flask-based video interface** to identify selected objects and blur them in the camera feed before that feed is shared with a meeting application.

The project aims to make visual privacy more accessible by providing a local, customizable solution that can work with tools such as **Google Meet and Zoom through OBS Virtual Camera**.

> **Privacy should be built into the camera feed, not left to chance.**

---

## Table of Contents

- [The Problem](#the-problem)
- [The Solution](#the-solution)
- [Key Features](#key-features)
- [How It Works](#how-it-works)
- [System Architecture](#system-architecture)
- [Technology Stack](#technology-stack)
- [Getting Started](#getting-started)
- [Connecting to Google Meet or Zoom](#connecting-to-google-meet-or-zoom)
- [Project Structure](#project-structure)
- [Privacy and Security](#privacy-and-security)
- [Current Limitations](#current-limitations)
- [Future Enhancements](#future-enhancements)
- [Use Cases](#use-cases)
- [Contributing](#contributing)
- [Author](#author)

---

## The Problem

Video calls have become an essential part of education, remote work, interviews, presentations, and online collaboration. However, a camera can unintentionally reveal more than the user intends to share.

Examples include:

- Identity cards and personal documents visible in the background.
- Mobile phones and computer screens displaying private information.
- People appearing unexpectedly in the camera frame.
- Confidential papers or work materials visible during presentations.
- Personal surroundings that the user does not want to expose.

Traditional background blur does not necessarily protect individual objects inside the camera frame. Manually moving documents or adjusting the camera can also be inconvenient during live meetings.

## The Solution

Nainikha introduces an AI-assisted privacy layer between the webcam and the video conferencing application.

It processes camera frames, detects supported object categories, applies blur to selected regions, and makes the processed video available for use through OBS Virtual Camera.

The goal is simple: **detect potentially sensitive visual content and obscure it before the processed camera feed is shared.**

---

## Key Features

### 1. AI-Based Object Detection

Uses the YOLOv8 Nano object detection model to identify supported object categories in camera frames.

### 2. Real-Time Privacy Blur

Applies Gaussian blur to detected regions to make their visual contents harder to recognize. The blur region can include an expanded margin around the detected object.

### 3. Configurable Protection

The application includes controls for privacy filtering and detection display, such as blur strength and bounding-box visibility, according to the implemented dashboard settings.

### 4. Sensitive-Object Detection Experiments

Includes additional detection logic intended to identify card-like or ID-like regions. The reliability of this feature depends on the image, lighting, angle, and detection implementation.

### 5. Live Video Processing

Uses OpenCV to capture and process camera frames, with a Flask endpoint that serves the processed video stream.

### 6. Video Conferencing Integration

Can be used with OBS Virtual Camera so supported meeting applications can receive the processed camera feed instead of the original webcam feed.

### 7. Local Processing Design

The main camera-processing pipeline runs on the user's computer. This reduces the need to send camera frames to an external processing service, although browser access, networking configuration, and any optional external services should still be reviewed separately.

### 8. Windows Desktop Support

Designed for a Windows development and deployment workflow, with the possibility of packaging the Python application into a desktop executable.

---

## How It Works

1. **Capture:** OpenCV obtains frames from the selected webcam.
2. **Detect:** The YOLOv8 Nano model identifies supported objects in the frame.
3. **Select:** The application determines which detected regions should receive privacy filtering.
4. **Blur:** OpenCV applies Gaussian blur to the selected regions.
5. **Stream:** Flask exposes the processed frames through a video-stream endpoint.
6. **Broadcast:** OBS displays the processed stream and sends it to the selected meeting application through OBS Virtual Camera.

The result is a privacy-filtered camera feed that can be selected in a compatible video conferencing application.

---

## System Architecture

```text
       Laptop Webcam
             |
             v
    OpenCV Frame Capture
             |
             v
    YOLOv8 Nano Detection
             |
             v
   Sensitive Region Selection
             |
             v
       Gaussian Blur
             |
             v
   Flask Processed Video Feed
             |
             v
        OBS Studio
             |
             v
     OBS Virtual Camera
             |
             v
    Google Meet / Zoom
```

**Important:** This architecture filters the outgoing camera feed. It does not automatically blur the other participants' incoming video, and it does not protect every possible source of sensitive information on the computer.

---

## Technology Stack

| Technology | Purpose |
|---|---|
| Python | Main application logic |
| YOLOv8 Nano | Object detection |
| OpenCV | Webcam capture, image processing, and blur |
| Flask | Dashboard and video-stream endpoint |
| Flask-SocketIO | Real-time communication where used by the application |
| NumPy | Image-array operations |
| OBS Studio | Virtual camera integration |
| PyInstaller | Packaging the Python application for Windows |

---

## Getting Started

### Prerequisites

Install the following software:

- Python 3.12, if supported by the versions of your project dependencies.
- Git.
- OBS Studio.
- A working webcam.
- Google Meet or Zoom for testing the processed camera feed.

### Step 1: Clone the repository

```bash
git clone https://github.com/PrateekKudari/Nainikha-shield.git
cd Nainikha-shield
```

If the Python application is inside a nested folder, navigate into the folder containing `app.py` before running the following commands.

### Step 2: Create a virtual environment

On Windows:

```powershell
python -m venv venv
.\venv\Scripts\Activate.ps1
```

If PowerShell blocks activation, you can use Command Prompt instead:

```bat
venv\Scripts\activate.bat
```

### Step 3: Install dependencies

If the repository contains a `requirements.txt` file:

```bash
python -m pip install --upgrade pip
pip install -r requirements.txt
```

Otherwise, install the dependencies specified by the project before continuing.

### Step 4: Start Nainikha

```bash
python app.py
```

Open the local dashboard in your browser:

```text
http://127.0.0.1:5000
```

The processed video stream is available at:

```text
http://127.0.0.1:5000/video_feed
```

The exact routes depend on the current application configuration. Keep the application running while using the stream.

---

## Connecting to Google Meet or Zoom

Nainikha can be used with meeting applications through OBS Virtual Camera.

### Step 1: Start Nainikha

Run the application and confirm that the processed camera feed is working.

### Step 2: Open OBS Studio

Add a **Browser Source** and configure it to display the processed video stream:

```text
http://127.0.0.1:5000/video_feed
```

If your OBS version or source configuration does not display the stream correctly, use a suitable browser source or media-source configuration for the endpoint's format.

### Step 3: Start OBS Virtual Camera

In OBS Studio, select **Start Virtual Camera**.

### Step 4: Select the virtual camera

Open Google Meet or Zoom and select **OBS Virtual Camera** as the camera input.

### Step 5: Verify the result

Place a test object in front of the webcam and confirm that the processed feed applies the expected blur before sharing it in a meeting.

**Safety check:** Test with non-sensitive objects first. Do not assume a card, document, face, or screen will always be detected correctly.

---

## Project Structure

A possible structure for the project is shown below. Actual filenames and folders may differ depending on the current repository version.

```text
Nainikha-shield/
├── app.py
├── requirements.txt
├── README.md
├── templates/
│   └── index.html
├── static/
│   ├── css/
│   ├── js/
│   └── images/
├── models/
│   └── YOLO model weights
└── .gitignore
```

The model weights may be downloaded or loaded automatically by the installed detection library, depending on how the application is configured.

---

## Privacy and Security

Nainikha is designed to reduce accidental visual exposure, but privacy protection depends on correct detection and configuration.

- **Local-first processing:** The primary camera pipeline is intended to run locally.
- **Processed output:** Only the feed routed through Nainikha and OBS receives the application's filtering.
- **Detection uncertainty:** An object may be missed, partially detected, or incorrectly classified.
- **Network access:** A local Flask server should not be exposed publicly without appropriate security controls.
- **Camera permissions:** Meeting applications still require camera permission and may access a different camera if configured incorrectly.
- **Verification:** Confirm that the meeting application is using OBS Virtual Camera, not the unfiltered physical webcam.

Avoid using the application as the sole protection for highly confidential documents, financial information, medical information, or other sensitive material.

---

## Current Limitations

This is an evolving project, and the following limitations should be considered:

- Detection performance depends on lighting, object size, camera angle, and occlusion.
- A general-purpose YOLO model cannot guarantee recognition of every sensitive object.
- Card and ID detection may require specialized training data to improve reliability.
- Real-time processing performance varies by computer hardware.
- OBS integration requires additional setup.
- Automatic meeting detection and a per-call permission prompt are separate features unless explicitly implemented and tested.
- The application does not automatically filter incoming video, screen sharing, or other applications' camera streams.

---

## Future Enhancements

Potential improvements include:

- Custom-trained models for identity cards, credit cards, documents, and private screens.
- Object tracking to reduce flickering between detection frames.
- Configurable privacy zones and category-specific filtering.
- Stronger detection confidence handling and automatic blur fallback.
- A cleaner Windows desktop interface with an allow/deny permission prompt.
- Meeting-session controls so filtering can be enabled for a specific call.
- Improved performance and CPU/GPU optimization.
- Packaging and installation improvements for easier deployment.
- Automated tests for detection, blur coverage, and virtual-camera output.
- Additional platform support after the Windows version is stable.

These are development goals, not claims that every feature is already implemented.

---

## Use Cases

- **Online meetings:** Reduce accidental exposure of objects in the camera frame.
- **Remote interviews:** Keep personal items and documents less visible.
- **Online classes:** Add a visual privacy layer when attending from home.
- **Remote work:** Help obscure selected objects in the camera background.
- **Demonstrations and presentations:** Experiment with real-time privacy filtering.
- **Computer vision research:** Explore object detection, image processing, and virtual-camera workflows.

---

## Contributing

Contributions, suggestions, bug reports, and testing feedback are welcome.

1. Fork the repository.
2. Create a feature branch.
3. Implement and test your changes.
4. Submit a pull request with a clear explanation of the changes.

For privacy-related changes, include test cases that demonstrate when the filter works and when it can fail.

---

## Author

**Prateek Kudari**

GitHub: [@PrateekKudari](https://github.com/PrateekKudari)

Project repository: [Nainikha AI Privacy Shield](https://github.com/PrateekKudari/Nainikha-shield)

---

## License

Choose and add a license file before publishing the project for reuse. Without an explicit license, others should not assume that they have permission to copy, modify, or redistribute the code.

---

### Final Note

Nainikha AI Privacy Shield explores how computer vision can help people control what their webcam reveals during online communication.

**Detect. Blur. Share with greater confidence.**

This project is intended as a privacy-assistance tool, not a guarantee that all sensitive information will be hidden.
