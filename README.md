# PDFIyagi v2.10.1

![PDFIyagi](1.png)

A **lightweight and fast PDF editor** for **Windows and Linux**.

PDFIyagi lets you edit existing PDF text, add new text, insert signatures and stamps, redact sensitive information, manage pages, and save documents while preserving the ability to edit them again later.

> **Developers (including AI assistants): Before modifying the code, read [`ARCHITECTURE.md`](ARCHITECTURE.md) first.** It explains the project architecture, external dependencies, and components that must not be reimplemented. See [`MODULE_MAP.md`](MODULE_MAP.md) for the module layout, [`CALLGRAPH.md`](CALLGRAPH.md) for function call flow, and [`DEVNOTES.md`](DEVNOTES.md) for design decisions.

---

## ✨ Features

### PDF Editing

* **Edit existing PDF text**
* **Re-edit anytime** — Documents remain editable after saving.
* **Rich text formatting** — Bold, italic, underline, and strike-through.
* **Text color selection**
* **L-Edit Mode** — Line-based editing for paragraphs.
* **Edit Mode** — Field-based editing for forms and structured documents.

### Image PDF Support

* **Add text to scanned PDFs**
* **Insert signatures, stamps, and seals**
* **Place text directly on images**
* **Paste images from the clipboard**
* **Images remain editable after saving**

### Redaction & Annotation

* **Permanent redaction** of sensitive information
* **Blur redaction**
* **Vector drawing tools** *(since v1.28)* — Rectangle, ellipse, line, and arrow with adjustable color and line width.

### Page Cropping *(since v1.21)*

* **Non-destructive PDF Crop** — Uses the PDF `/CropBox`, compatible with Adobe Acrobat's *Crop Pages*. The original page content is preserved, and crop boundaries can be adjusted at any time before saving.
* **Permanent AI7 Crop** — Crops the actual page image and all overlays immediately. Original data is physically removed and cannot be restored.
* **Rotation-aware cropping** — Crops rotated pages correctly without coordinate errors.

### Advanced Saving *(since v1.29)*

* **Native IMGPDF Saving Engine**

  * Stores page backgrounds, inserted images, and edited text independently.
  * Adding a photo increases only that page's size instead of re-encoding the entire document.
* **Automatic compatibility fallback**

  * Documents containing only searchable text layers or rotated pages are automatically saved using the optimal method.

### Neural OCR *(since v1.24)*

* **Built-in PP-OCRv5** using ONNX Runtime
* **High recognition accuracy** on real scanned documents
* **Multilingual OCR**

  * Korean + English
  * Chinese + Japanese + English
  * Western European languages
* **No external OCR installation required**
* **Creates searchable PDF and AI7 documents**

---

## 🧠 AI7 — Seven Capabilities of an AI-Native Document

AI7 is more than a file format.

It is an **AI-Native Document Format** designed to let AI systems read, understand, search, analyze, and automate documents without requiring external preprocessing.

| # | Capability     | Description                                                                               | Implementation                          |
| - | -------------- | ----------------------------------------------------------------------------------------- | --------------------------------------- |
| 1 | **Read**       | Reads text, tables, images, and metadata as a unified document object.                    | Layer 0 Raster + Layer 1 OCR Text       |
| 2 | **Understand** | Understands document structure including headings, paragraphs, tables, and relationships. | `content/document.md`                   |
| 3 | **Structure**  | Stores semantic document structure.                                                       | Auto-generated CSV tables & metadata    |
| 4 | **Connect**    | Builds a document knowledge graph.                                                        | `ai/document.kg`                        |
| 5 | **Search**     | Performs semantic search instead of keyword matching.                                     | Embedded vector database                |
| 6 | **Reason**     | Supports summarization, comparison, analysis, and question answering.                     | Markdown + Knowledge Graph + Embeddings |
| 7 | **Act**        | Enables AI agents to edit, analyze, transform, and automate documents.                    | Non-destructive overlays + History      |

**Read → Understand → Structure → Connect → Search → Reason → Act**

> AI7 is not simply a document file format.
> It is a document architecture that transforms documents into structured knowledge for AI.

---

### AI7 (.ai7) Format

* **Non-destructive editing** using layered architecture
* **Semantic Markdown** for AI and RAG systems
* **Automatic table extraction**
* **Cell-level and row-level editing**
* **Automatic font size estimation**
* **Fast parallel saving**
* **Complete specification:** [`AI7FORMAT.md`](AI7FORMAT.md)

### Scanner Support (Windows & Linux)

* Multi-page ADF scanning
* 300 / 600 / 1200 / 2400 / 4800 DPI
* Built-in OCR
* Vectorization (Linux)
* Tested with Samsung and HP scanners
* Windows: WIA
* Linux: eSCL (AirScan) / SANE

> On Linux, multi-page ADF scanning is automatically enabled when `scanimage` (`sane-utils`) is installed.

### Productivity

* Undo / Redo
* Drag with Ctrl to duplicate objects
* Cut images (Ctrl+X)
* Rotate images
* Extract page or document text
* Replace all fonts
* Page management
* Thumbnail navigation
* Window position memory

### Rendering Engine

* **IYAGI PDF Engine**

  * Fully proprietary PDF engine.
  * Parses, renders, extracts text, decrypts encrypted PDFs (RC4/AES), merges PDFs, and saves documents without external PDF libraries.
  * Since **v2.0**, PDFium and Qt PDF have been completely removed.

* **WinRT (Windows only)**

  * Optional rendering engine based on the Windows Runtime PDF API.

---

## ⌨️ Keyboard Shortcuts

| Shortcut           | Function                  |
| ------------------ | ------------------------- |
| Ctrl+O             | Open PDF                  |
| Ctrl+S             | Save                      |
| Ctrl+Shift+S       | Save As                   |
| Ctrl+B             | Insert Blank Page         |
| Ctrl+I             | Insert Image              |
| Ctrl+V             | Paste Image               |
| Ctrl+Z             | Undo                      |
| Ctrl+Y             | Redo                      |
| Delete             | Delete Selected Object    |
| Ctrl+T             | Extract Current Page Text |
| Ctrl+Shift+T       | Extract Entire Document   |
| Ctrl+Shift+F       | Replace All Fonts         |
| Ctrl + Mouse Wheel | Zoom                      |
| Mouse Wheel        | Scroll Pages              |
| ← / →              | Previous / Next Page      |
| Ctrl + ← / →       | Rotate Selected Image     |
| Arrow Keys         | Move Selected Object      |
| Shift + Arrow Keys | Move by 10 Pixels         |
| Ctrl + Drag        | Duplicate Object          |
| Ctrl + X           | Cut Image                 |

---

## ⬇️ Download

### Windows

Available from the Microsoft Store.

### Linux

Download the latest release from GitHub Releases.

```bash
chmod +x PDFIyagi
./PDFIyagi
```

---

## 🖥️ Supported Platforms

* Windows 10 / 11
* Ubuntu
* Debian
* Debian-based Linux distributions

---

## 👤 Author

**IYAGI INC**

Email: [iyagicom@gmail.com](mailto:iyagicom@gmail.com)

GitHub: https://github.com/iyagicom

---

## 📋 Version History

> **Versioning Policy**
>
> * **Major** = Major architectural or feature changes
> * **Minor** = One new user feature
> * **Patch** = Bug fixes, performance improvements, and stability enhancements

### v2.10.1 (2026-07-19)

* Improved stability when opening damaged or unusually large AI7 documents.
* Enhanced protection against malicious AI7 files that attempt excessive memory usage.

### v2.10.0 (2026-07-19)

* Added AI7 Security Profiles (Strict / Extended).
* Improved compatibility with enterprise and government security policies.

### v2.9.1 (2026-07-19)

* Significantly strengthened AI7 file security.
* Added protection against zip bombs, CSV formula injection, and HTML injection.
* Improved stability when opening large AI7 files.

### v2.9.0 (2026-07-19)

* Added two AI7 save formats:

  * AI7 v1.0 (Compatible)
  * AI7 v1.2 (Current)
* Improved compatibility when sharing documents with organizations.

### v2.8.0 (2026-07-19)

* Finalized the AI7 v1.2 specification.
* Every AI7 document now includes built-in documentation for easier integration by developers and AI systems.

### v2.7.2 (2026-07-19)

* Improved image security for externally received AI7 files.
* Prevents crashes caused by extremely large images.

### v2.7.1 (2026-07-19)

* Improved redaction security.
* Redacted content is no longer preserved in document history or AI data.

### v2.7.0 (2026-07-19)

* AI7 documents can now record document authors and AI contributors.
* Makes document history easier to understand.

### v2.6.0 (2026-07-19)

* Added comments and review threads to AI7 documents.
* Enables collaborative review between people and AI.

### v2.5.0 (2026-07-19)

* Reorganized version numbering.
* No functional changes.

### v2.2.x (2026-07-18)

* Completed advanced PDF gradient and vector graphics support.
* Improved rendering of CAD drawings, government forms, and engineering documents.

### v2.2.0 (2026-07-18)

* Added native support for nearly all PDF image compression formats.
* Greatly improved compatibility with scanned PDFs.

### v2.1.x (2026-07-18)

* Added JPEG2000 and JBIG2 image support.
* Greatly improved compatibility with scanner-generated PDFs.

### v2.0.0 (2026-07-13)

* Introduced the fully native IYAGI PDF engine.
* Removed all dependencies on PDFium and Qt PDF.
* Improved speed, stability, and compatibility.

### v1.29.0 (2026-07-09)

* Introduced the new native PDF saving engine.
* Prevents unnecessary file size growth after adding images.
* Improved save performance and output quality.

### v1.28.0

* Added vector drawing tools including rectangles, ellipses, lines, and arrows.
* Drawings remain sharp at any zoom level.

### v1.25.x (2026-07-03 ~ 07-04)

* Expanded AI7 capabilities.
* Added automatic table recognition and editing.
* Added semantic search and document history.
* Full Windows support for AI7 and Neural OCR.

### v1.24.x (2026-07-03)

* Added built-in PP-OCRv5 neural OCR.
* Creates searchable PDF and AI7 documents from scanned pages.

### v1.23.x (2026-07-02 ~ 07-03)

* Greatly reduced saved PDF file sizes.
* Existing text PDFs can be edited without OCR.
* Improved text extraction and search accuracy.

### v1.21.x ~ v1.22.x (2026-07-01 ~ 07-13)

* Developed the native IYAGI PDF engine.
* Improved compatibility with damaged PDFs and Korean documents.
* Added encrypted PDF support, PDF merging, scanner PDF compatibility, and page cropping.

### v1.20.x (2026-06)

* Released the Windows version.
* Added editable AI7 documents.
* Added ADF multi-page scanning.

### v1.18.x

* Improved OCR accuracy.
* Enhanced the user interface and editing workflow.
* Improved ADF scanning stability.

---

## 📜 License

Copyright © 2026 IYAGI INC. All rights reserved.

This software is distributed as executable binaries only. The source code is not publicly available.

### Linux

Free for personal, commercial, educational, government, and enterprise use, including redistribution.

### Windows

Distributed through the Microsoft Store under Microsoft's licensing policies.
