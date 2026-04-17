# PDFIyagi v0.9.0

**Image-based PDF Processor + Fast Viewer (Qt6 Multi-Engine)**

PDFIyagi is a **privacy-focused PDF editor and fast viewer** that reconstructs documents as images.
All saved outputs are **fully image-based PDFs**, and original text or structure is not preserved.

---

## 🔥 Key Features

* All exports are **forced into image-based PDFs**
* Removes original text and metadata → **full anonymization**
* WYSIWYG output (what you see is what you get)
* Switchable rendering engines (speed vs quality)

👉 One-line summary
**Not editing PDFs — rebuilding them as images**

---

## ✨ Features

### Rendering Engines

* Qt PDF (fast)
* Poppler (accurate text extraction)
* MuPDF (high-quality rendering)

---

### Editing Tools

* Text overlay editing
* Black box redaction
* Pixel blur anonymization
* Image insertion (clipboard / drag & drop)
* Insert blank pages

---

### Save / Export

* Image-based PDF only

  * Render each page as image
  * Merge all edits
  * Output via QPdfWriter

⚠️ No text selection / search / copy
⚠️ File size may increase

---

### Text Extraction

* Powered by Poppler
* Page / full document extraction

---

## 🧰 Modes

| Mode    | Description                              |
| ------- | ---------------------------------------- |
| View    | View-only mode                           |
| Edit    | Overlay editing (modify / move / delete) |
| Marking | Rectangle redaction                      |
| Blur    | Pixelation                               |


---

## ⌨ Shortcuts

| Key             | Action                    |
| --------------- | ------------------------- |
| Ctrl+O          | Open PDF                  |
| Ctrl+S          | Save PDF                  |
| Ctrl+Shift+S    | Save with options         |
| Ctrl+B          | Insert blank page         |
| Ctrl+V          | Paste image               |
| Ctrl+Z / Ctrl+Y | Undo / Redo               |
| Del             | Delete selection          |
| Ctrl+T          | Extract current page text |
| Ctrl+Shift+T    | Extract all text          |
| Ctrl+Shift+F    | Select font               |
| Ctrl+Shift+C    | Select marking color      |
| ← / →           | Navigate pages            |
| Ctrl+Wheel      | Zoom                      |

---

## 📦 Distribution

### Policy

* Prebuilt binaries only
* No source code provided

### Linux

* Portable / packaged versions
* Free for all use cases
* Redistribution allowed

### Windows

* Distributed via Microsoft Store
* Subject to Store policies

---

## ⚠️ Best For

* Personal data anonymization
* Document redaction
* Preparing PDFs for external sharing

## ❌ Not Suitable For

* Editing while preserving structure
* OCR-based editing
* Reusing original text
