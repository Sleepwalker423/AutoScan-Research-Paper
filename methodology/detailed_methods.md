# AutoScan: Detailed Methodology

## Table of Contents
1. [System Overview](#system-overview)
2. [Background Technologies](#background-technologies)
3. [Training Data and Annotations](#training-data-and-annotations)
4. [Speech Bubble Detection (Step 1)](#speech-bubble-detection-step-1)
5. [Text Extraction (Step 2)](#text-extraction-step-2)
6. [Translation (Step 3)](#translation-step-3)
7. [Image Editing (Step 4)](#image-editing-step-4)
8. [Performance Evaluation](#performance-evaluation)

---

## System Overview

AutoScan implements a four-step pipeline for automated manga translation that consolidates state-of-the-art approaches in machine translation while adding novel contributions in font consistency through speech bubble classification.

### Pipeline Flow

```
Input Manga Page → Bubble Detection (YOLOv8) → Text Extraction (Manga OCR) → Translation (DeepL) → Image Editing (Generative Fill) → Translated Output
```

### Technical Stack
- **Object Detection**: YOLOv8 by Ultralytics
- **Annotation Tool**: Roboflow
- **OCR Engine**: Manga OCR (based on Microsoft's TrOCR)
- **Translation**: DeepL translator
- **Image Editing**: Adobe's generative fill tool
- **Future Automation**: Google's Vertex AI (planned)

---

## Background Technologies

### Object Detection with YOLO

Object detection is an ML classification task that aims to locate specified classes within an image. Traditional methods use a Convolutional Neural Network (CNN) with a sliding window to locate class instances, but this can be relatively time-consuming.

By contrast, the **You Only Look Once (YOLO)** models are CNNs that divide the image into a grid where any cell that includes the center of an object must detect that object with a confidence score. This means, as the name implies, that the YOLO model only needs to scan the image once to locate the desired classes within it, making it much faster than other popular methods.

### Optical Character Recognition (OCR)

OCR is a method by which an ML model extracts text from an image. There are many potential applications including converting scanned documents to digital format and finding text within manga pages for translation. OCRs may be trained to pick up different alphabets and languages, or even handwritten text.

**OCR Process Steps:**
1. **Pre-processing**: The OCR reduces noise within the image (sometimes placed after other phases)
2. **Segmentation**: Detecting text lines within the rest of the image
3. **Feature extraction**: Combining text lines into feature vectors for classification
4. **Classification**: Classifying feature vectors into recognizable characters within an alphabet

---

## Training Data and Annotations

### Dataset Specifications
- **Total Images**: 1,062 manga pages
- **Annotation Tool**: Roboflow
- **Method**: Transfer learning applied to YOLOv8
- **Bubble Classes**: 7 distinct types for font maintenance

### Speech Bubble Classes

The following bubble types were annotated as different classes to enable font consistency:

1. **Regular speech bubbles** - Standard dialogue
2. **Shout bubbles** - Emphasized, loud speech
3. **Narrative bubbles** - Narration/caption boxes
4. **Happy bubbles** - Joyful or cheerful dialogue
5. **Evil bubbles** - Sinister or threatening speech
6. **Thought bubbles** - Internal thoughts
7. **Scared bubbles** - Frightened or alarmed speech

Each bubble type is associated with a specific font style in the translated output to maintain the visual sentiment and context of the original comic.

---

## Speech Bubble Detection (Step 1)

### YOLOv8 Implementation

The speech bubble detection utilizes **YOLOv8 by Ultralytics**, adapted for manga-specific bubble classification through transfer learning.

### Training Process
1. Annotated 1,062 manga pages using Roboflow
2. Labeled bubbles into 7 classes
3. Applied transfer learning to YOLOv8
4. Trained model to detect and classify bubble types

### Performance Metrics
- **mAP@0.5**: 0.724 (mean Average Precision at IoU 0.50)
- **Acceptance Criteria**: Predicted box must overlap with ground truth box with IoU ≥ 0.50

### Output Data
The bubble detection step saves the following for each detected bubble:
- Bubble class/type (1 of 7 categories)
- Bounding box coordinates (x, y) for each corner
- Confidence score
- Image crop of the bubble region

These coordinates are used in later steps for:
1. Cropping the bubble for OCR
2. Targeting the region for text removal
3. Positioning translated text

---

## Text Extraction (Step 2)

### OCR Comparison Study

Multiple OCR engines were evaluated for Japanese manga text extraction:

| OCR System | Performance Notes |
|------------|------------------|
| **Tesseract** | Available in many languages; gave basic results but insufficient accuracy |
| **EasyOCR** | Supports 80+ languages; poor results for manga |
| **Google Cloud Vision (GCV)** | Better than previous; tracked text location with word-level bounding polygons; useful mask generation; slightly lower accuracy than Manga OCR |
| **Manga OCR** ✓ | Best performance; trained specifically on manga; handles Furigana; works with text on images; Japanese-only |

### Manga OCR Selection

**Manga OCR** was selected as the optimal solution based on:
- Custom training specifically for manga text
- Built on Microsoft's TrOCR architecture
- Superior handling of Japanese characters
- Ability to recognize Furigana (pronunciation guides)
- Effective with text overlaid on complex backgrounds

### Process Flow
1. Cropped bubble image received from Step 1
2. Manga OCR processes the image
3. Japanese text extracted and saved to script file
4. Constraining input to bubble regions improves accuracy

### Limitations and Challenges

**Furigana Handling**: Experiments show difficulty with Furigana-heavy text where small pronunciation characters may be interpreted as regular speech rather than pronunciation guides.

**Text Metrics**: Manga OCR does not track text location metrics within the bubble, which would be helpful for precise masking. Google Cloud Vision provides this feature but with lower overall accuracy. Future work may integrate GCV's mask generation with Manga OCR's superior recognition.

---

## Translation (Step 3)

### DeepL Translation

AutoScan uses **DeepL** for Japanese-to-English translation due to its superior performance compared to alternatives like Google Translate.

### Current Implementation
- **Method**: Bubble-by-bubble translation
- **Process**: 
  1. Receive extracted Japanese text from Manga OCR
  2. Send text to DeepL API
  3. Receive English translation
  4. Pass to editing step with bubble metadata

### Limitations
The current bubble-by-bubble approach translates each bubble independently without broader context. This can lead to:
- Loss of narrative flow
- Inconsistent pronoun usage
- Missing contextual nuances

### Future Improvements
A more accurate translation would incorporate:
- Complete page or scene scripts
- Context from previous bubbles
- Character tracking for consistent voice
- Bibliographic information (title, author)
- Longer contextual windows using previous scenes

Research shows that context-aware translation significantly improves accuracy, as demonstrated by existing literature on manga translation pipelines.

---

## Image Editing (Step 4)

### Adobe Generative Fill

The editing step uses **Adobe's generative fill tool** to:
1. Remove Japanese text from bubble regions
2. Extend backgrounds naturally to fill cleaned areas
3. Maintain visual consistency with surrounding artwork

### Process Flow
1. **Text Removal**: Target Japanese character regions within bubbles
2. **Background Extension**: Use generative fill to continue background patterns
3. **Text Overlay**: Place translated English text in cleaned area
4. **Font Matching**: Apply appropriate font based on bubble class
5. **Size Fitting**: Scale text to fit within bounding box constraints

### Font Assignment

Font styles are matched to bubble types to preserve visual meaning:

| Bubble Type | Font Style | Purpose |
|-------------|-----------|----------|
| Regular | Standard comic font | Normal dialogue |
| Shout | Bold, enlarged | Emphasis, volume |
| Narrative | Formal, caption-style | Story narration |
| Happy | Rounded, cheerful | Positive emotion |
| Evil | Jagged, dramatic | Threatening tone |
| Thought | Italic, lighter | Internal monologue |
| Scared | Shaky, uneven | Fear, alarm |

This font consistency maintains the additional visual context that lends meaning to the story, addressing a gap in existing automatic translation systems.

### Current Limitations
- **Manual Process**: Editing currently requires manual operation
- **Pixel Residue**: Some Japanese character pixels may remain after cleaning
- **Text Outside Bubbles**: Requires special handling with generative fill

### Future Automation
Future versions will automate the editing step using:
- **Google Vertex AI**: For automated generative fill
- **Text Masks**: Using GCV's bounding polygon data for precise targeting
- **Batch Processing**: Automated pipeline for entire pages

---

## Performance Evaluation

### Detection Performance
- **mAP@0.5**: 0.724 for speech bubble detection and classification
- **Class Coverage**: 7 bubble types successfully distinguished
- **Dataset**: Evaluated on 1,062 annotated manga pages

### OCR Performance
- **Best System**: Manga OCR outperformed Tesseract, EasyOCR, and Google Cloud Vision
- **Strengths**: Handles Japanese characters, Furigana, and text on images
- **Challenges**: Furigana-heavy text occasionally misinterpreted

### Translation Quality
- **System**: DeepL selected for superior performance
- **Method**: Currently bubble-by-bubble (future: context-aware)
- **Output**: English translations with maintained font styling

### Overall System
- **Complete Pipeline**: Four-step process from detection to final edited page
- **Key Innovation**: Font consistency through bubble classification
- **State-of-the-Art**: Consolidates leading contributions in manga translation
- **Novel Addition**: Generative fill for background preservation

---

## Challenges and Future Work

### Current Challenges

1. **Context Loss**: Bubble-by-bubble translation lacks narrative context
2. **Manual Editing**: Generative fill step requires manual operation
3. **Text Metrics**: Manga OCR doesn't provide location data for masking
4. **Furigana Handling**: Heavy pronunciation guides can confuse OCR
5. **Bubble Ordering**: No system for reading order determination

### Future Directions

1. **Bubble Ordering**: Implement methods to order bubbles for contextual script translation
2. **Character Tracking**: Build character personas for better translation context
3. **Emotion Analysis**: Combine with character tracking for improved translations
4. **Automated Generative Fill**: Use Google Vertex AI or similar diffusion models
5. **Frame Tracking**: Enable scene-level context for translation
6. **Text Masking**: Integrate GCV's polygon masks with Manga OCR's accuracy
7. **LLM Integration**: Use large language models for:
   - Character persona creation
   - Fan fiction generation
   - Context-aware translation improvement

### Research Integration

Future work could incorporate:
- Longer contextual windows from previous scenes
- Bibliographic information (title, author) for context
- Multimodal LLMs for visual and textual context
- Character emotion analysis for sentiment-aware translation
- Automated transcript generation for accessibility

---

## Reproducibility Notes

### System Requirements
- **Object Detection**: YOLOv8 by Ultralytics
- **Annotation**: Roboflow account and tools
- **OCR**: Manga OCR installation
- **Translation**: DeepL API access
- **Editing**: Adobe Photoshop with generative fill

### Training Steps
1. Collect manga page dataset
2. Annotate bubbles into 7 classes using Roboflow
3. Apply transfer learning to YOLOv8
4. Train until achieving acceptable mAP (target: >0.7)
5. Save model weights and configuration

### Pipeline Execution
1. Load trained YOLOv8 model
2. Process manga page through detection
3. Crop detected bubbles
4. Run Manga OCR on crops
5. Translate text with DeepL
6. Apply generative fill (manual or automated)
7. Overlay translated text with class-specific fonts

---

## Contributions to the Field

AutoScan makes the following contributions to automatic manga translation:

1. **Literature Consolidation**: Summarizes leading approaches in one pipeline
2. **Font Consistency**: Novel implementation using 7 bubble classes
3. **Generative AI Integration**: Explores generative fill for cleaning
4. **Practical Pipeline**: Functional end-to-end system demonstration
5. **Future Roadmap**: Identifies clear directions for advancement

The system demonstrates that font consistency is a crucial visual element that previous automated translation systems have overlooked, and provides a practical method for preserving this context through bubble classification.

---

*This methodology is based on the AutoScan research paper. For complete details including figures, tables, and experimental results, refer to the full paper at `paper/AutoScan_Paper.pdf`.*
