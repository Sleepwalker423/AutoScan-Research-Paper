# AutoScan: Leveraging Computer Vision to Improve Comic Book Translations and Editing

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

## Overview

This repository contains the research paper and supporting materials for **AutoScan**, an automated system that leverages computer vision and machine learning to streamline manga (Japanese comic book) translation and editing workflows. The system processes Japanese manga pages, detects speech bubbles and text, translates content into English, and seamlessly integrates translations while preserving the original artistic style and font characteristics through intelligent bubble classification.

## Abstract

As Machine Learning becomes ever more ubiquitous, it offers promise in automating many previously time-consuming processes, including making literature more accessible. This work focuses on the challenges facing the manga community regarding translation and widespread dissemination of thousands of available comics. Not only is translating and editing manga time-consuming, but translated copies are often unofficial and may be subject to individuality in translation as opposed to an official and standard translation.

AutoScan addresses these challenges by introducing an end-to-end automated pipeline that:

- **Detects Speech Bubbles**: Uses YOLOv8 to classify 7 types of speech bubbles for font consistency
- **Extracts Text**: Employs Manga OCR for accurate Japanese character recognition
- **Translates Content**: Utilizes DeepL for high-quality Japanese-to-English translation
- **Preserves Context**: Maintains varying fonts to preserve visual meaning and sentiment

This research demonstrates improvements in translation efficiency while maintaining the visual context that lends additional meaning to the original story.

## System Architecture

The AutoScan pipeline consists of four main steps:

1. **Speech Bubble Detection**: Locates text within frames using YOLOv8 by Ultralytics
2. **Text Extraction**: Extracts text from frames using Manga OCR
3. **Translation**: Translates extracted text using DeepL translator
4. **Image Editing**: Edits frames to include new translations with appropriate fonts

## Repository Contents

```
AutoScan-Research-Paper/
├── README.md                 # This file
├── paper/
│   ├── AutoScan_Paper.pdf   # Complete research paper
│   ├── abstract.md          # Extended abstract
│   └── references.bib       # Bibliography
├── figures/
│   ├── system_architecture.png
│   ├── detection_pipeline.png
│   └── results_comparison.png
├── examples/
│   ├── input/               # Sample manga pages (input)
│   └── output/              # Translated pages (output)
├── methodology/
│   └── detailed_methods.md  # Detailed methodology
└── LICENSE
```

## Key Features

### 1. Speech Bubble Classification
- Uses YOLOv8 object detection model by Ultralytics
- Classifies 7 types of speech bubbles to maintain font consistency:
  - Regular speech bubbles
  - Shout bubbles
  - Narrative bubbles
  - Happy bubbles
  - Evil bubbles
  - Thought bubbles
  - Scared bubbles
- Achieved 0.724 mAP at IoU 0.50

### 2. Specialized Japanese OCR
- Utilizes Manga OCR (based on Microsoft's TrOCR)
- Trained specifically for manga text recognition
- Handles Japanese text including Furigana (pronunciation guides)
- Works well with text on top of images

### 3. High-Quality Translation
- Uses DeepL translator for accurate Japanese-to-English translation
- Currently translates bubble-by-bubble
- Future versions will incorporate contextual translation

### 4. Generative Fill Technology
- Employs Adobe's generative fill tool for image editing
- Removes Japanese text and extends backgrounds naturally
- Future versions will automate using Google's Vertex AI

## Results

AutoScan demonstrates:
- **0.724 mAP** (mean Average Precision) at IoU 0.50 for speech bubble detection
- **7 bubble types** classified for font consistency maintenance
- **Manga OCR** performance superior to Tesseract, EasyOCR, and Google Cloud Vision
- **Font preservation** through bubble type classification enables context maintenance
- **Automated translation** pipeline from detection through editing

## Training Data

The system was trained and tested on:
- **1,062 manga page images** annotated using Roboflow
- **Transfer learning** applied to adapt YOLOv8 for manga-specific detection
- **7 speech bubble classes** for font variation tracking

## Methodology

The system employs a four-step processing pipeline:

1. **Speech Bubble Detection**: YOLOv8 model locates and classifies speech bubbles, saving (x, y) coordinates for each bounding box
2. **Text Extraction**: Manga OCR extracts Japanese text from cropped bubble regions
3. **Translation**: DeepL translates the extracted text to English
4. **Image Editing**: Adobe's generative fill removes Japanese text and extends backgrounds, then translated text is overlaid with appropriate fonts based on bubble type

For detailed methodology, see [methodology/detailed_methods.md](methodology/detailed_methods.md).

## Example Results

Below are sample comparisons showing original Japanese manga pages and their AutoScan-translated English versions:

| Original (Japanese) | AutoScan Output (English) |
|---------------------|---------------------------|
| ![Input Example](examples/input/sample_page_01.jpg) | ![Output Example](examples/output/sample_page_01_translated.jpg) |

*More examples available in the `examples/` directory*

## Research Contributions

This research makes the following contributions:

1. **Literature Summary**: Summarizes leading contributions to automatic manga translation
2. **AutoScan Pipeline**: Presents an automatic manga translator based on compilation of leading contributions
3. **Font Consistency**: Implements font consistency using multiple bubble classes (7 types)
4. **Generative Fill**: Explores generative fill technology for improving the cleaning process before translation
5. **Future Directions**: Identifies current challenges and future directions for automatic manga translation

## Author Contributions

This research paper was developed by a team from Embry-Riddle Aeronautical University:
- **Kino Leonhart** - Department of Electrical Engineering and Computer Science
- **Lynn Vonderhaar** - Department of Electrical Engineering and Computer Science
- **Juan Couder** - Department of Electrical Engineering and Computer Science
- **Charles Walker** - Department of Electrical Engineering and Computer Science
- **Omar Ochoa** - Department of Electrical Engineering and Computer Science

**My Contributions**: I reviewed, edited, and wrote significant portions of the paper, including methodology refinement, results analysis, and technical writing improvements.

## Future Work

- Speech bubble ordering for context-aware translation of complete scenes
- Character tracking to build character personas for better translation context
- Emotion analysis combined with character tracking for improved translations
- Automated generative fill using Google's Vertex AI
- LLM integration for character persona creation and fan fiction generation
- Improved handling of Furigana-heavy text
- Text mask tracking for precise cleaning

## Citation

If you use this research in your work, please cite:

```bibtex
@inproceedings{leonhart2024autoscan,
  title={AutoScan: Leveraging Computer Vision to Improve Comic Book Translations and Editing},
  author={Leonhart, Kino and Vonderhaar, Lynn and Couder, Juan and Walker, Charles and Ochoa, Omar},
  booktitle={IEEE Conference Proceedings},
  year={2024},
  organization={Embry-Riddle Aeronautical University}
}
```

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments

We thank our peer reviewers and collaborators for their valuable feedback and contributions to this research..

## Contact

For questions, suggestions, or collaboration opportunities, please see authors' contact information.

---

*This research aims to support and enhance the manga translation community, not replace human translators. The system is designed as a tool to improve efficiency while preserving the artistic and cultural integrity of the original works.*
