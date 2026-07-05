# PDFIyagi v1.25.13

![PDFIyagi](pdfiyagi.png)

A lightweight and fast **PDF editor for Windows and Linux**.

PDFIyagi allows direct PDF text editing, new text insertion, stamp and signature placement, privacy redaction, page management, and persistent saving with full re-edit support after save.

---

## ✨ Key Features

### PDF Editing
- **Direct text editing** — Modify existing PDF text
- **Re-editable documents** — Continue editing after saving
- **Text formatting** — Bold, italic, underline, strikethrough
- **Text color control** — Fully customizable text colors
- **L-Edit mode** — Paragraph/line-based editing
- **Edit mode** — Form and field-level editing

### Image & PDF Support
- **Text on scanned PDFs** — Add text over image-based PDFs
- **Stamp & signature insertion** — Place seals, stamps, signatures freely
- **Text over images** — Overlay text on images
- **Clipboard image paste** — Paste images directly from clipboard
- **Re-editable images** — Move and edit images after saving

### Redaction & Security
- **Permanent redaction** — Irreversible sensitive data removal
- **Blur masking** — Apply blur to selected regions

### Neural OCR (v1.24)
- **PP-OCRv5 engine (ONNX Runtime CPU)** — < 1 sec per page
- **High accuracy** — Near-zero error on real-world contracts
- **Multilingual support** — Korean / English / Chinese / Japanese / Latin
- **No external dependencies** — Fully bundled model
- **Searchable PDF / AI7 output** — OCR text layer stored for search and selection

---

## 🧠 AI7 — Seven Capabilities of an AI-Native Document

AI7 is not just a file format.  
It is an AI-native document architecture that enables machines to read, understand, and act on documents.

```
Read → Understand → Structure → Connect → Search → Reason → Act
```

| # | Capability | Description | Implementation |
|---|------------|-------------|----------------|
| 1 | Read | Reads text, tables, images, metadata as a unified object | Layer 0 raster + OCR |
| 2 | Understand | Interprets semantic structure of content | content/document.md |
| 3 | Structure | Converts document into structured data | table extraction + metadata |
| 4 | Connect | Builds a knowledge graph | ai/document.kg |
| 5 | Search | Semantic search beyond keywords | embeddings |
| 6 | Reason | QA, summarization, analysis | KG + embeddings |
| 7 | Act | AI-driven document manipulation | overlay + history |

> AI7 is not a storage format.  
> It is a **knowledge architecture for AI-native documents**.

---

## 📄 AI7 (.ai7) Structure

```
/metadata.json
/document.kg
/content/
    document.md
    table_01.csv
/resources/
/styles/
/embeddings/
/history/
/agent_scripts/
```

---

## ✨ Core Features

### Document Editing
- Direct text editing
- Persistent re-edit after saving
- Rich text formatting
- Color control
- L-Edit / Edit modes

### Image Handling
- Stamp & signature support
- Text overlay on images
- Clipboard image paste
- Re-editable image layers

### Security
- Permanent redaction
- Blur masking

---

## 🧠 OCR Engine

- PP-OCRv5 (ONNX Runtime CPU)
- Sub-second processing per page
- Multi-language support
- Fully bundled (no external install)

---

## 🧭 Scanner Support

- ADF multi-page scanning
- 300–4800 DPI support
- WIA / SANE / eSCL
- AirScan auto detection

---

## ⚙️ Rendering Engine

```
IYAGI Engine (default)
        ↓ fallback
PDFium / Qt
```

- Fully custom PDF parser
- Incremental save system
- Corrupted PDF recovery support

---

## ⌨️ Shortcuts

```
Ctrl+O : Open
Ctrl+S : Save
Ctrl+Z : Undo
Ctrl+Y : Redo
Ctrl+I : Insert Image
Ctrl+T : Extract Text
Ctrl+Shift+F : Change Font
Ctrl+MouseWheel : Zoom
```

---

## ⬇ Download

### Windows
Available on Microsoft Store

### Linux
```bash
chmod +x PDFIyagi
./PDFIyagi
```

---

## 🖥 Supported Platforms

```
Windows 10 / 11
Linux (Ubuntu, Debian and compatible distributions)
```

---

## 📜 License

```
Copyright (c) 2026 IYAGI INC

Binary-only distribution.
No source code disclosure.
```

### Linux Version
Free for personal, commercial, educational, and redistribution use

### Windows Version
Distributed via Microsoft Store (store policy applies)
