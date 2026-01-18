# Figures for AutoScan Research Paper

This directory contains visual materials supporting the AutoScan research paper.

## Figure Descriptions

### system_architecture.png
**Description:** High-level overview of the AutoScan pipeline architecture

**Content:**
- Input: Original Japanese manga page
- Module 1: Text Detection (YOLOv5)
- Module 2: Speech Bubble Detection (Mask R-CNN)
- Module 3: OCR (Manga-OCR)
- Module 4: Translation (mBART)
- Module 5: Text Rendering & Integration
- Output: Translated English manga page

**Visual Style:** Professional flowchart with module boxes, arrows showing data flow, and sample images at each stage

**Dimensions:** 1920x1080 pixels (16:9 aspect ratio for presentations)

---

### detection_pipeline.png
**Description:** Detailed visualization of text and bubble detection process

**Content:**
- Original manga panel
- Text detection bounding boxes (red)
- Speech bubble segmentation masks (blue overlay)
- Confidence scores displayed
- Classification labels (speech/thought/narrative)

**Visual Style:** Multi-panel comparison showing progressive detection stages

**Dimensions:** 2400x1200 pixels (panoramic layout)

---

### results_comparison.png
**Description:** Side-by-side quality comparison

**Content:**
Three columns showing:
1. **Original (Japanese):** Authentic manga page with Japanese text
2. **AutoScan Output:** Automated translation result
3. **Human Translation:** Professional translator result

Multiple examples (3-4 different pages) showing various scenarios:
- Standard dialogue
- Thought bubbles
- Narrative boxes
- Action scenes with minimal text

**Visual Style:** Clean grid layout with labels, equal-sized panels

**Dimensions:** 3000x2000 pixels (high resolution for detail inspection)

---

### translation_quality_metrics.png
**Description:** Charts showing translation performance

**Content:**
- BLEU score bar chart (comparing AutoScan to baselines)
- METEOR score comparison
- Human evaluation ratings (5-point scale)
- Translation time comparison

**Visual Style:** Professional data visualization with color-coded bars, error bars, and axis labels

**Dimensions:** 1600x900 pixels

---

### font_matching_examples.png
**Description:** Font style preservation demonstration

**Content:**
Grid showing:
- Row 1: Original Japanese text samples (various fonts)
- Row 2: Font classifier predictions
- Row 3: Matched English fonts in translated output
- Row 4: Side-by-side comparison overlays

**Visual Style:** Structured grid with annotations showing font names and similarity scores

**Dimensions:** 2000x1500 pixels

---

### error_analysis.png
**Description:** Examples of challenging cases and failure modes

**Content:**
Annotated examples showing:
1. **Success Case:** Standard page processed perfectly
2. **Partial Success:** Minor text fitting issues
3. **Failure Case 1:** Highly stylized artistic text (handwritten)
4. **Failure Case 2:** Overlapping speech bubbles
5. **Failure Case 3:** Complex non-standard layout

Each example includes:
- Original input
- AutoScan output
- Highlighted problem areas (red circles)
- Brief explanation text

**Visual Style:** Educational layout with clear annotations

**Dimensions:** 2400x1800 pixels

---

### processing_time_breakdown.png
**Description:** Performance timing analysis

**Content:**
- Pie chart: Time distribution across modules
- Bar chart: Comparison with manual translation workflow
- Line graph: Processing time vs. page complexity
- Table: Detailed timing statistics

**Visual Style:** Multi-panel visualization with professional styling

**Dimensions:** 1800x1200 pixels

---

### detection_accuracy_curves.png
**Description:** Precision-Recall curves for detection modules

**Content:**
- PR curve for text detection
- PR curve for bubble detection
- ROC curves
- mAP scores at different IoU thresholds

**Visual Style:** Standard ML performance visualization with legends

**Dimensions:** 1600x1200 pixels

---

## How to Generate Figures

Figures for research papers are typically created using:

1. **Architecture Diagrams:** 
   - Tools: draw.io, Lucidchart, Microsoft Visio, TikZ (LaTeX)
   - Export as high-resolution PNG or vector PDF

2. **Detection Visualizations:**
   - Tools: Python (matplotlib, OpenCV) with actual system output
   - Overlay detection results on sample images
   - Save with appropriate DPI (300+ for print)

3. **Performance Charts:**
   - Tools: Python (matplotlib, seaborn), R (ggplot2), Excel
   - Use consistent color schemes
   - Export as vector graphics (SVG/PDF) when possible

4. **Comparison Grids:**
   - Tools: Image editing software (Photoshop, GIMP)
   - Python PIL/Pillow for automated layout
   - Ensure consistent sizing and alignment

## Figure Guidelines

For publication-quality figures:
- **Resolution:** Minimum 300 DPI for raster images
- **Format:** PNG for screenshots/photos, PDF/SVG for diagrams
- **Fonts:** Use readable sizes (10pt minimum after scaling)
- **Colors:** Colorblind-friendly palettes
- **Labels:** Clear axis labels, legends, and captions
- **Copyright:** Only use images with proper permissions (original creation or licensed)

## Sample Images Note

Due to copyright considerations with manga content, actual sample images are not included in this public repository. For research purposes:

- Use public domain manga or obtain proper licenses
- Create synthetic examples for demonstration
- Obtain permission from publishers for academic use
- Cite sources for any copyrighted material

---

*Figures should be generated based on actual experimental results from the AutoScan system. The descriptions above outline what each figure should contain.*
