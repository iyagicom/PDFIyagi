![FileIyagi](1.png)
![FileIyagi](2.png)


# PDFIyagi v1.18.5

![PDFIyagi](pdfiyagi.png)

A **lightweight and fast PDF editor** for Windows and Linux.

PDFIyagi allows you to edit PDF text, add new text, insert stamp images, redact sensitive information, manage pages, and save documents while keeping all edits fully re-editable.

---

## ✨ Features

### PDF Editing

* **Text Editing** — Edit existing PDF text directly
* **Re-editable Documents** — Reopen and continue editing after saving
* **Text Formatting** — Bold, Italic, Underline, and Strikethrough
* **Text Colors** — Change text color freely
* **L-Edit Mode** — Line-based paragraph editing
* **Edit Mode** — Field-based editing for forms and structured documents

### Image PDF Support

* **Add Text to Scanned PDFs** — Insert new text into image-based PDFs
* **Stamp Image Insertion** — Place signatures, seals, and stamp images anywhere
* **Text Overlay** — Add text directly on top of images
* **Clipboard Image Paste** — Paste copied images directly into PDFs
* **Re-editable Images** — Move and edit inserted images after reopening the document

### Redaction & Security

* **Black Redaction** — Permanently remove sensitive information
* **Blur Redaction** — Apply blur effects to selected areas

### Scanner Support (Linux)

* **ADF Multi-page Scanning** — Scan multiple pages continuously using an Automatic Document Feeder (ADF)
* **OCR (Text Recognition)** — Save scanned documents as searchable PDFs (Korean + English, requires ocrmypdf)
* **Vectorization** — Convert scanned images into line-art vectors (available in grayscale and monochrome modes)
* **Tested Devices**: Samsung (including SL-T1672DW series), HP (Samsung OEM models)
* **Supported Protocols**: eSCL (AirScan), SANE

> ADF multi-page scanning is automatically enabled when `scanimage` (from the `sane-utils` package) is installed.
>
> OCR requires `ocrmypdf` and `tesseract-ocr-kor`.

### Productivity

* **Undo / Redo** — Full Ctrl+Z and Ctrl+Y support
* **Ctrl+Drag Duplication** — Duplicate text and image objects
* **Text Extraction** — Extract text from the current page or the entire document
* **Global Font Replacement** — Replace fonts throughout the entire document
* **Page Management** — Reorder pages and insert blank pages
* **Thumbnail Navigation** — Quickly navigate using page thumbnails

### Rendering Engine

* **Type 1** — Fast Rendering
* **Type 2** — Accurate Rendering
* **Type 3** — Highest Quality Rendering (Recommended)

---

## 🎮 Keyboard Shortcuts

| Shortcut           | Action                            |
| ------------------ | --------------------------------- |
| Ctrl + O           | Open PDF                          |
| Ctrl + S           | Save PDF                          |
| Ctrl + Shift + S   | Save As                           |
| Ctrl + B           | Insert Blank Page                 |
| Ctrl + I           | Insert Image                      |
| Ctrl + V           | Paste Clipboard Image             |
| Ctrl + Z           | Undo                              |
| Ctrl + Y           | Redo                              |
| Delete             | Delete Selected Object            |
| Ctrl + T           | Extract Current Page Text         |
| Ctrl + Shift + T   | Extract Entire Document Text      |
| Ctrl + Shift + F   | Replace Fonts in Entire Document  |
| Ctrl + Mouse Wheel | Zoom In / Out                     |
| ← / →              | Previous / Next Page              |
| Arrow Keys         | Move Selected Object by 1 Pixel   |
| Shift + Arrow Keys | Move Selected Object by 10 Pixels |
| Ctrl + Drag        | Duplicate Text or Image           |

---

## ⬇ Download

### Windows

Available from Microsoft Store.

### Linux

Download the latest release from GitHub Releases.

```bash
chmod +x PDFIyagi
./PDFIyagi
```

---

## 🖥 Supported Platforms

* Windows 10 / 11
* Linux (Ubuntu, Debian, and compatible distributions)

---

## 👤 Author

IYAGI INC

Email: [iyagicom@gmail.com](mailto:iyagicom@gmail.com)

GitHub: https://github.com/iyagicom

---

## 📜 License

Copyright (c) 2026 IYAGI INC. All rights reserved.

This software is distributed in binary form only. Source code is not publicly available.

### Linux Version

Free for personal, commercial, educational, governmental, and organizational use. Installation, packaging, and redistribution are permitted.

### Windows Version

Distributed through Microsoft Store.

Usage and licensing are subject to Microsoft Store policies.
