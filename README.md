# PDFIyagi v1.0.0

Linux PDF Viewer + Anonymization Editor (Qt6 Multi-Engine)

PDFIyagi is a **prebuilt binary application** designed for fast PDF viewing and secure document redaction.

No source distribution is provided. Only executable builds are released.

---

## ✨ Key Features

### Rendering Engine
- Multi-engine runtime switching
  - Qt PDF (default, fast)
  - Poppler (accurate text extraction)
  - MuPDF (high-quality rendering)

---

### Editing Tools
- Text editing (overlay-based replacement)
- Redaction tool (black box irreversible masking)
- Blur tool (pixelation-based anonymization)
- Image paste (clipboard / drag & drop)
- Blank page insertion

---

### Save / Export
- Image-based PDF export only
  - Each page rendered as image
  - All edits flattened into output
  - Export via QPdfWriter

⚠️ Original PDF text layer is NOT preserved

---

### Text Extraction
- Poppler-based extraction
- Page / full document extraction supported

---

## 🖥 UI Layout
