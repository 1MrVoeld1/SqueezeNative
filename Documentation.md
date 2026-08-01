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

SqueezeNative separates interface rendering from compression workloads to maintain stable performance and prevent application freezing.

---

## Phase 1: Main UI Thread

**Technology:**
```
Dear ImGui + DirectX
```

Responsibilities:

* User interface rendering.
* Drag-and-drop handling.
* User input processing.
* Displaying current application status.

---

## Phase 2: Worker Threads

Compression tasks are executed separately from the UI thread.

Responsibilities:

* File validation.
* Format detection.
* Compression pipeline selection.
* Backend execution.
* Progress updates.

---

## Phase 3: Backend Process Management

SqueezeNative manages embedded backend processes internally.

Main operations:

* Extract required backend components when needed.
* Launch processing engines in isolated processes.
* Monitor execution status.
* Capture progress information.
* Return results back to the user interface.

---

# 4. Licensing & Security Module

SqueezeNative includes a local offline licensing system designed for commercial distribution.

---

## 4.1 Trial Period

* The first launch starts a 3-day evaluation period.
* Trial status is stored locally.
* After expiration, compression features require activation with a valid license key.

---

## 4.2 License Activation

* License keys are validated locally using an offline verification mechanism.
* Activation does not require a permanent internet connection.
* A successful activation unlocks the licensed version of the software.

---

# 5. Technical Summary

SqueezeNative combines a native C++ interface, asynchronous processing architecture, embedded compression engines, and offline licensing into a lightweight portable Windows application.

The design focuses on:

* Performance.
* Reliability.
* Simple deployment.
* Efficient media optimization.
