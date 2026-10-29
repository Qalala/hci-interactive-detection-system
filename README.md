# HCI Interactive Detection System

An interactive Human–Computer Interaction system that combines **real-time object detection**, **gaze tracking**, and **face login** with multi-touch / tangible table input (TUIO) for classroom-style demos.

The stack is:
- **Desktop clients**: C# / .NET WinForms (TUIO demos, YOLO-enhanced UI, teacher/admin panels)
- **Vision helpers**: Python (YOLO, OpenCV, gaze / face pipelines)
- **Table input**: TUIO + OSC networking

This project is designed for live demos: a tracker or webcam feeds events into interactive forms that teachers/students can use around a multi-touch table or screen.

## Tech stack
- C# / .NET (WinForms)
- TUIO / OSC
- Python
- YOLO (Ultralytics)
- OpenCV
- Gaze tracking (MediaPipe / custom helpers)
- Face login pipeline

## Architecture (high level)

```
Sensors / inputs
├─ Webcam (YOLO objects, face, gaze)
└─ TUIO tracker (reacTIVision / table)

        │  pipes / sockets / TUIO events
        ▼
┌──────────────────────────────────────────┐
│ C# WinForms clients                      │
│ ├─ TuioDemo / TuioDump                   │
│ ├─ TuioDemoYolo / YoloEnhancedInterface  │
│ ├─ FaceLogin / LoginForm                 │
│ ├─ TeacherDashboardForm / AdminPanelForm │
│ └─ Services/*PipeClient                  │
└──────────────────────────────────────────┘
        ▲
        │ Python helpers
┌───────┴──────────────────────────────────┐
│ python/  GazeTracking/  YOLO/            │
│ ├─ YOLO detectors / pipe servers         │
│ ├─ gaze trackers / adapters              │
│ └─ face identification / emotion helpers │
└──────────────────────────────────────────┘
```

### Major C# pieces
- `TUIO_CSHARP.sln` — solution containing demo/lib/YOLO projects
- `TuioDemo.cs` / `TuioDump.cs` — baseline TUIO visualization / logging
- `TuioDemoYolo.cs`, `YoloEnhancedInterface.cs`, `FruitYoloDemo.cs` — detection-enhanced UIs
- `FaceLogin.cs`, `LoginForm.cs` — face-oriented access flow
- `TeacherDashboardForm.cs`, `AdminPanelForm.cs` — classroom/admin controls
- `Services/` — pipe clients bridging Python processes and the UI
- `TUIO/`, `OSC.NET/` — protocol/library code

### Major Python pieces
- `YOLO/` — detectors, webcam tests, pipe server
- `GazeTracking/` — multiple gaze experiments/adapters
- `python/` — face identification, emotion, gesture server, bridges

### Models (not in repo)
Large binaries are excluded on purpose. See `MODELS.md` for what to place locally:
- YOLO `.pt` weights
- dlib `shape_predictor_68_face_landmarks.dat`
- MediaPipe `face_landmarker.task`

---

## Running the project

### Prerequisites
- Windows with Visual Studio 2022 (recommended)
- .NET workload supporting the included `.csproj` targets
- Python 3.9+ for vision helpers
- Webcam
- Optional: reacTIVision / TUIO tracker for table demos (`download_reactivision.bat` helper is included)
- Model files from `MODELS.md`

### 1) C# client
1. Open `TUIO_CSHARP.sln`
2. Restore NuGet packages if prompted
3. Place model files where your chosen demo expects them
4. Build and run the project matching your demo:
   - TUIO-only: `TUIO_DEMO`
   - Detection-enhanced: YOLO-related projects
   - Face login / dashboards: corresponding forms/projects

### 2) Python helpers
Example (exact package set depends on script):

```bash
cd python
pip install -r requirements.txt
```

Then run the helper you need (detector pipe, gaze server, face login, etc.). Many C# services expect those helpers to already be listening.

### 3) TUIO table path
1. Start reacTIVision (or another TUIO source)
2. Start the C# TUIO demo
3. Interact with fiducials/touches and confirm events in the UI

---

## How to navigate the repo
- Start with `README_YOLO_INTEGRATION.md` for detection wiring notes
- Use `MODELS.md` before expecting vision features to run
- Browse `Services/` to see how UI talks to Python
- Explore `YOLO/`, `GazeTracking/`, `python/` for the vision side
- Keep `bin/`, `obj/`, `.vs/`, datasets, and weight files out of git (already ignored)

---

## Demo walkthroughs

### A) TUIO baseline
1. Launch a TUIO tracker.
2. Run `TuioDemo`.
3. Move objects/cursors and watch the canvas update.

### B) YOLO-enhanced interaction
1. Place YOLO weights as documented.
2. Start the Python YOLO pipe/server if required by your chosen UI.
3. Run the YOLO-enhanced C# interface.
4. Point a webcam at demo objects and confirm detections drive UI behavior.

### C) Face login + dashboards
1. Prepare face samples / model files used by the face scripts.
2. Start the face helper script.
3. Launch login UI and verify recognized users reach teacher/admin flows.

### Notes / limitations
- Binary models and large datasets are intentionally omitted
- Several experimental gaze scripts exist; pick the stable path for your demo machine
- Calibration quality (camera, lighting, tracker setup) strongly affects results
- This is a lab/demo system, not a packaged commercial installer

---

## Project purpose
Show how HCI demos can fuse multi-touch/TUIO input with computer-vision channels (object detection, gaze, face login) into interactive classroom applications.
