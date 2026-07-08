# PDFIyagi v1.29.2

![PDFIyagi](pdfiyagi.png)

A **lightweight and fast PDF editor** for Windows and Linux.

PDFIyagi lets you edit existing PDF text, add new text, insert signatures and stamp images, permanently redact sensitive information, manage pages, and save documents while keeping them editable for future editing sessions.

---

# ✨ Features

## PDF Editing

- **Edit existing PDF text**
- **Re-edit after saving** — edited documents remain editable after reopening
- **Rich text formatting** — bold, italic, underline, and strike-through
- **Text color selection**
- **L-Edit Mode** — paragraph/line-based editing
- **Edit Mode** — field-based editing for forms and structured documents

## Image PDF Support

- **Add text to scanned PDFs**
- **Insert signatures and stamp images**
- **Place text anywhere on an image**
- **Paste images directly from the clipboard**
- **Images remain editable after saving**

## Redaction & Annotation

- **Permanent information redaction**
- **Blur redaction**
- **Shape drawing (v1.28)**

  - Rectangle
  - Ellipse
  - Line
  - Arrow
  - Adjustable line width
  - Custom colors
  - Saved as vector graphics

## Saving Engine (New in v1.29)

### New IMGPDF Native Save Engine

PDFIyagi now uses its own IMGPDF save engine instead of relying on QPdfWriter or PDFium.

Features include:

- Saves background images and inserted images independently
- Text edits are stored as vector outlines
- Adding one image increases only that page's size
- No full-document rerasterization
- Original image quality is preserved
- Automatic fallback to the traditional PDFium save path when required
  (such as searchable OCR-only text layer documents or page rotation)

---

# Neural Network OCR (New in v1.24)

- Built-in **PP-OCRv5**
- ONNX Runtime CPU execution
- Less than one second per page
- Extremely high recognition accuracy
- Korean + English
- Chinese + Japanese + English
- Western European languages
- No external OCR packages required
- Searchable PDF and AI7 document generation

---

# 🧠 AI7 — Seven Capabilities of an AI-Native Document

AI7 is not simply another document format.

It is an **AI-native document architecture** designed so AI systems can read, understand, search, analyze, and act on documents.

| Capability | Description |
|------------|-------------|
| **Read** | Read text, images, tables and metadata accurately |
| **Understand** | Understand document semantics such as titles, paragraphs and tables |
| **Structure** | Convert content into structured data |
| **Connect** | Build a document knowledge graph |
| **Search** | Semantic search using vector embeddings |
| **Reason** | AI summarization, comparison and question answering |
| **Act** | Non-destructive editing and AI automation |

**Read → Understand → Structure → Connect → Search → Reason → Act**

AI7 transforms documents into structured knowledge that AI systems can directly consume.

---

# AI7 (.ai7) Format

- Non-destructive editing
- Layer 0: Original page image
- Layer 1: OCR text layer
- Layer 2: Editable overlay
- Semantic Markdown (`content/document.md`)
- Automatic table extraction
- CSV export
- Table cell editing
- Automatic font size estimation
- Parallel page encoding
- OCR cache reuse
- AI7 format specification available in `AI7FORMAT.md`

---

# Scanner Support (Windows & Linux)

- Multi-page ADF scanning
- 300 / 600 / 1200 / 2400 / 4800 DPI
- Built-in Neural Network OCR
- Searchable AI7 generation
- Raster-to-vector conversion (Linux)
- Tested with Samsung and HP scanners
- Windows: WIA
- Linux: eSCL (AirScan) and SANE

When `scanimage` (sane-utils) is installed, Linux automatically enables multi-page ADF scanning.

---

# Productivity

- Undo / Redo
- Ctrl + Drag object duplication
- Cut images
- Rotate images
- Extract page text
- Extract document text
- Replace all fonts
- Page management
- Thumbnail navigation
- Window size restoration

---

# Rendering Engine

## IYAGI Engine

The default rendering engine.

A fully self-developed PDF engine implementing:

- PDF parsing
- Rendering
- Text extraction
- Incremental save
- No external PDF library required

## Fallback Engines

- PDFium
- Qt PDF

Automatically selected for documents unsupported by the IYAGI engine.

---

# ⌨ Keyboard Shortcuts

| Shortcut | Action |
|----------|--------|
| Ctrl + O | Open PDF |
| Ctrl + S | Save |
| Ctrl + Shift + S | Save As |
| Ctrl + B | Insert Blank Page |
| Ctrl + I | Insert Image |
| Ctrl + V | Paste Clipboard Image |
| Ctrl + Z | Undo |
| Ctrl + Y | Redo |
| Delete | Delete Selected Object |
| Ctrl + T | Extract Current Page Text |
| Ctrl + Shift + T | Extract Entire Document Text |
| Ctrl + Shift + F | Replace All Fonts |
| Ctrl + Mouse Wheel | Zoom |
| Mouse Wheel | Scroll Pages |
| ← / → | Previous / Next Page |
| Ctrl + ← / → | Rotate Selected Image |
| Arrow Keys | Move Object |
| Shift + Arrow Keys | Move Object (10 px) |
| Ctrl + Drag | Duplicate Object |
| Ctrl + X | Cut Selected Image |

---

# Download

## Windows

Available from the Microsoft Store.

## Linux

Download the latest release from GitHub Releases.

```bash
chmod +x PDFIyagi
./PDFIyagi
```

---

# Supported Platforms

- Windows 10
- Windows 11
- Ubuntu
- Debian
- Compatible Linux distributions

---

# Developed By

**IYAGI INC**

Email: iyagicom@gmail.com

GitHub: https://github.com/iyagicom

---

# Version History

## v1.29.0 (2026-07-09)

### New Native IMGPDF Save Engine

- Completely redesigned image PDF save engine
- Eliminated unnecessary file size growth
- Independent storage for background and inserted images
- Vector-based text saving
- Improved save reliability
- Automatic compatibility fallback
- Cross-viewer validation improvements

---

## v1.28.0

### Shape Drawing

- Rectangle
- Ellipse
- Line
- Arrow
- Adjustable stroke width
- Live preview
- Vector storage
- Editable inside AI7

---

## v1.25.x

Major AI7 improvements

- Complete Seven Capabilities implementation
- Automatic table detection
- Semantic Markdown
- Vector embeddings
- Knowledge graph
- Edit history
- Faster parallel saving
- Windows OCR support
- Improved scanning

---

## v1.24.x

- Built-in PP-OCRv5
- AI7 specification published
- Semantic Markdown generation
- Lossless WEBP storage
- Multiple stability improvements

---

## v1.23.x

- Native incremental PDF save
- Dramatically reduced output file size
- Improved text extraction
- Reuse existing PDF text layers
- Better OCR handling
- Engine selector improvements

---

## v1.21.x – v1.22.x

- New IYAGI PDF rendering engine
- Broken PDF recovery
- Massive Korean CID font compatibility improvements
- Automatic encrypted PDF detection
- Numerous AI7 editing fixes

---

## License

Copyright (c) 2026 IYAGI INC.

All rights reserved.

The software is distributed as executable binaries only.
Source code is not publicly available.

### Linux

Free for personal, commercial, educational, government, and enterprise use.
Redistribution and packaging are permitted.

### Windows

Distributed through Microsoft Store under Microsoft's licensing policies.
