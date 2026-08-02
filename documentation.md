# SqueezeNative v1.0 — Technical Documentation

**SqueezeNative** is a self-contained, high-performance native Windows desktop application designed to optimize and compress media files and documents. Built with C++ and Dear ImGui, it provides a fast drag-and-drop interface while processing heavy compression workloads through an asynchronous background engine.

---

# 1. Core Features

* **Self-Contained Architecture:** All required backend components are packaged with the application and deployed without additional user installation steps.
* **Asynchronous Processing:** The UI remains responsive while compression tasks are executed on background worker threads.
* **Real-Time Progress Tracking:** The application provides live compression progress and estimated completion time during supported operations.
* **Portable Design:** No installation process is required. The application can run directly from any supported Windows directory.
* **Secure Licensing System:** Includes a built-in 3-day evaluation period and offline license activation.

---

# 2. Supported Formats & Compression Pipelines

SqueezeNative automatically detects supported file formats and applies optimized compression pipelines depending on the media type.

---

## 2.1 Video Compression

**Supported Extensions:**
```
.mp4
.mov
.avi
.mkv
```

**Backend Engine:**
```
Embedded FFmpeg backend
```

**Compression Pipeline:**

* Video Codec: `libx264` (H.264 Advanced Video Coding)
* Rate Control: Constant Rate Factor (CRF)
* Compression Profile: CRF `28`
* Preset: `faster`

The default profile is designed to provide a balanced trade-off between compression ratio, processing speed, and output quality.

Example pipeline:

```
-y -i "<input>" -vcodec libx264 -crf 28 -preset faster "<output>"
```

---

## 2.2 PNG Image Optimization

**Supported Extensions:**

```
.png
```

**Backend Engine:**

```
Embedded pngquant backend
```

**Compression Pipeline:**

* Algorithm: Palette optimization and color reduction.
* Quality Range: `65-80`
* Transparency information is preserved.

Example pipeline:

```
--force --quality=65-80 "<input>" --output "<output>"
```

---

## 2.3 JPEG / WebP Optimization

**Supported Extensions:**

```
.jpg
.jpeg
.webp
```

**Backend Engine:**

```
Embedded FFmpeg backend
```

**Compression Pipeline:**

* Algorithm: Image re-encoding with optimized compression parameters.
* Quality scale: `q:v 3`

Example pipeline:

```
-y -i "<input>" -q:v 3 "<output>"
```

---

## 2.4 PDF Optimization

**Supported Extensions:**

```
.pdf
```

**Backend Engine:**

```
Embedded pdfcpu backend
```

**Compression Pipeline:**

* Structural optimization.
* Removal of redundant objects.
* Embedded image optimization.

Example pipeline:

```
optimize "<input>" "<output>"
```

---

# 3. Architecture & Threading Model

SqueezeNative separates interface rendering from compression workloads to maintain stable performance, prevent application freezing, and enable simultaneous high-speed processing.

---

## Phase 1: Main UI Thread

**Technology:**
```
Dear ImGui + DirectX
```

Responsibilities:
* User interface rendering at stable frame rates.
* Drag-and-drop handling for multiple files and entire folders.
* User input processing and dynamic list rendering.
* Displaying real-time progress bars, logs, and application status.

---

## Phase 2: Asynchronous Worker Threads & Batch Processing
Compression tasks are completely isolated from the UI thread and execute in parallel.
Responsibilities:
* Batch queue management: accepting multiple file pathways simultaneously.
* File validation and automatic format detection.
* Parallel pipeline execution: spawning multiple isolated worker threads based on the hardware capacity to maximize C++ performance without throttling the system.
* Direct real-time progress updates sent back to the main Dear ImGui table.

---

## Phase 3: Backend Process Management
SqueezeNative manages embedded backend processes internally.
Main operations:
* Extract required backend components safely when needed.
* Launch processing engines (FFmpeg, pngquant, pdfcpu) in isolated, sandboxed background processes.
* Monitor thread execution status and catch exit codes.
* Safely manage output streams to prevent any data loss, saving compressed files with a `_compressed` suffix

---

# 4. Licensing, Subscriptions & Security Module

SqueezeNative includes a secure local licensing system designed for modern agile teams and enterprise commercial distribution.

---

## 4.1 Trial Period
* The first launch initiates a fully functional 3-day evaluation period.
* Full batch processing and maximum native speed are available during the trial.
* After expiration, compression features are locked until activation with a valid license key.

---

## 4.2 Annual License Activation
* License keys are issued for specific commercial packages: **SOLO** (1 PC), **TEAM** (Up to 5 PCs), or **ENTERPRISE** (Up to 20 PCs) on an **annual subscription basis ($39, $149, or $449 / year)**.
* License validation is verified safely through an internal time-locking mechanism.
* The application runs 100% offline, guaranteeing complete enterprise data privacy and absolute compliance with strict corporate NDAs.


---

# 5. Technical Summary

SqueezeNative combines a native C++ interface, asynchronous processing architecture, embedded compression engines, and offline licensing into a lightweight portable Windows application.

The design focuses on:

* Performance.
* Reliability.
* Simple deployment.
* Efficient media optimization.
