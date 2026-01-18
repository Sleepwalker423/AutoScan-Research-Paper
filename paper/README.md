# AutoScan Research Paper

## Complete Paper Available ✅

The complete research paper **"AutoScan: Leveraging Computer Vision to Improve Comic Book Translations and Editing"** is available in this directory as **`AutoScan_Paper.pdf`** (6 pages).

### Authors
- **Kino Leonhart** - Embry-Riddle Aeronautical University
- **Lynn Vonderhaar** - Embry-Riddle Aeronautical University
- **Juan Couder** - Embry-Riddle Aeronautical University
- **Charles Walker** - Embry-Riddle Aeronautical University
- **Omar Ochoa** - Embry-Riddle Aeronautical University

### Publication Information
- **Conference**: IEEE Conference Proceedings
- **Year**: 2024
- **Institution**: Department of Electrical Engineering and Computer Science, Embry-Riddle Aeronautical University
- **Location**: Daytona Beach, USA

---

## Paper Contents

### Abstract
The paper addresses the challenge of automating manga translation while preserving visual context. It introduces AutoScan, which uses computer vision to maintain font consistency through bubble classification and leverages generative AI for image cleaning.

### Key Sections

1. **Introduction** - Motivation for automated manga translation
2. **Background** - Object Detection (YOLO) and OCR fundamentals
3. **Existing Solutions** - Survey of related work in manga translation
4. **Approach** - Four-step AutoScan pipeline:
   - Speech bubble detection using YOLOv8
   - Text extraction with Manga OCR
   - Translation with DeepL
   - Image editing with generative fill
5. **Discussion** - Font consistency and generative fill integration
6. **Challenges and Future Directions** - Character tracking, emotion analysis, bubble ordering
7. **Conclusion** - Summary of contributions

### Key Results
- **0.724 mAP** at IoU 0.50 for speech bubble detection
- **7 bubble types** classified for font consistency
- **1,062 training images** annotated with Roboflow
- **YOLOv8 + Manga OCR + DeepL** pipeline

---

## Additional Resources in This Directory

| File | Description |
|------|-------------|
| `AutoScan_Paper.pdf` | Complete 6-page IEEE paper |
| `abstract.md` | Extended abstract with all sections |
| `AutoScan_Paper.md` | Paper structure overview |
| `references.bib` | Complete bibliography (30+ references) |

---

## Figures in the Paper

The PDF includes the following figures:
1. **Figure 1**: YOLO model's detection method
2. **Figure 2**: AutoScan pipeline diagram
3. **Figure 3**: YOLO results showing classified bubbles
4. **Figure 4**: Generative fill for text outside speech bubbles
5. **Figure 5**: Fully translated page with two font types

And a table comparing OCR systems (Tesseract, EasyOCR, Google Cloud Vision, Manga OCR).

---

## How to Use This Paper

### For Reading
- Open `AutoScan_Paper.pdf` for the complete paper with figures and formatting
- Read `abstract.md` for a detailed text summary of all sections
- Check `../methodology/detailed_methods.md` for expanded technical details

### For Citation
See the citation format in the main README.md or use the BibTeX entry:

```bibtex
@inproceedings{leonhart2024autoscan,
  title={AutoScan: Leveraging Computer Vision to Improve Comic Book Translations and Editing},
  author={Leonhart, Kino and Vonderhaar, Lynn and Couder, Juan and Walker, Charles and Ochoa, Omar},
  booktitle={IEEE Conference Proceedings},
  year={2024},
  organization={Embry-Riddle Aeronautical University}
}
```

### For Research
- The methodology is reproducible using the details provided
- Training dataset: 1,062 manga pages annotated via Roboflow
- Models: YOLOv8 (Ultralytics), Manga OCR, DeepL translator
- Tools: Adobe Photoshop generative fill (manual), future: Google Vertex AI

---

## Keywords

machine learning, manga, machine translation, comic book translation, generative artificial intelligence, YOLOv8, optical character recognition, computer vision

---

*The PDF file contains the official published version of the paper with IEEE formatting, figures, and complete references.*
