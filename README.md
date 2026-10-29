# HCI Interactive Detection System

## Brief description
An interactive HCI system combining real-time object detection, gaze tracking, and face login with multi-touch table applications for classroom-style demos (TUIO / C# + Python helpers).

## Project purpose
Show how computer vision (YOLO / OpenCV), gaze tracking, and tangible/multi-touch input (TUIO) can be combined into interactive educational interfaces with teacher/admin panels and face-based login.

## Project requirements
- Windows with .NET (solution includes multiple `.csproj` targets)
- Visual Studio 2022 recommended
- Python 3.9+ for gaze / YOLO helper scripts under `python/` and `GazeTracking/`
- Optional: reacTIVision or another TUIO tracker for table input
- Model weight files — **not included**; see `MODELS.md`

## How to navigate
- `*.cs` / `*.csproj` / `TUIO_CSHARP.sln` — C# TUIO demos, YOLO-enhanced UI, login/admin forms
- `TUIO/`, `OSC.NET/` — TUIO/OSC library code
- `python/`, `GazeTracking/`, `YOLO/` — Python-side helpers
- `Services/`, `Config/`, `Data/` — app services and configuration
- `MODELS.md` — where to put large weight files locally
- `README_YOLO_INTEGRATION.md` — YOLO integration notes

## How to run
1. Open `TUIO_CSHARP.sln` in Visual Studio.
2. Place required model files as described in `MODELS.md`.
3. Install Python deps used by your chosen scripts (typically `opencv-python`, `ultralytics`, etc.).
4. Build and run the desired project (e.g. TUIO demo / YOLO-enhanced interface).
5. Start a TUIO tracker if you are using the multi-touch table flow.

> Large datasets, `bin/`, `obj/`, IDE caches, and binary model weights are intentionally omitted so the repo stays focused on source you authored/extended.