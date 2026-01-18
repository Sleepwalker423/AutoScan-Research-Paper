# AutoScan Examples

This directory contains example input and output images demonstrating the AutoScan system's capabilities.

## Directory Structure

```
examples/
├── input/          # Original Japanese manga pages
└── output/         # Translated English manga pages
```

## Copyright and Usage Notice

⚠️ **Important:** Manga content is protected by copyright law.

The examples in this repository should:
- Be used only for research and educational purposes
- Come from public domain sources or with explicit permission
- Include proper attribution to original artists and publishers
- Not be redistributed without authorization

For this public repository, we **do not include copyrighted manga images** without permission.

## Example Categories

The following types of examples would demonstrate system capabilities:

### 1. Standard Dialogue Pages (`sample_page_01.*`)
**Description:** Basic manga page with standard speech bubbles
- **Complexity:** Low
- **Text Orientation:** Mixed (vertical and horizontal)
- **Bubble Count:** 4-6 speech bubbles
- **Difficulty:** Easy
- **Expected Accuracy:** 95%+

### 2. Thought Bubbles (`sample_page_02.*`)
**Description:** Page featuring thought bubbles with internal monologue
- **Complexity:** Medium
- **Bubble Type:** Thought bubbles (cloud-like borders)
- **Text Orientation:** Vertical Japanese
- **Difficulty:** Medium
- **Expected Accuracy:** 90%+

### 3. Action Scene (`sample_page_03.*`)
**Description:** Action-packed page with sound effects and minimal dialogue
- **Complexity:** High
- **Features:** Stylized sound effects (onomatopoeia), dynamic text
- **Text Orientation:** Various angles
- **Difficulty:** Hard
- **Expected Accuracy:** 85%+

### 4. Multiple Characters (`sample_page_04.*`)
**Description:** Conversation between multiple characters
- **Complexity:** Medium
- **Bubble Count:** 8-10 bubbles
- **Features:** Overlapping bubbles, small text
- **Difficulty:** Medium
- **Expected Accuracy:** 88%+

### 5. Narrative Boxes (`sample_page_05.*`)
**Description:** Page with narrator text boxes and scene-setting
- **Complexity:** Low
- **Bubble Type:** Rectangular narrative boxes
- **Text Orientation:** Horizontal
- **Difficulty:** Easy
- **Expected Accuracy:** 95%+

## File Naming Convention

Input files: `sample_page_[NN].jpg`
- Original Japanese manga pages
- High resolution (2000+ pixels height)
- JPG format for photographic content

Output files: `sample_page_[NN]_translated.jpg`
- Translated English versions
- Same resolution as input
- Preserves image quality

Comparison files: `sample_page_[NN]_comparison.jpg`
- Side-by-side comparison (optional)
- Left: Original, Right: Translated

## Metadata

For each example, metadata should include:

```json
{
  "page_id": "sample_page_01",
  "source": "[Public domain or licensed source]",
  "artist": "[Artist name if applicable]",
  "genre": "Shonen/Shoujo/Seinen/etc",
  "complexity": "Low/Medium/High",
  "text_regions": 6,
  "speech_bubbles": 5,
  "thought_bubbles": 0,
  "narrative_boxes": 1,
  "sound_effects": 2,
  "processing_time_ms": 4500,
  "detection_accuracy": 0.96,
  "translation_bleu": 0.47,
  "rendering_quality": 4.3
}
```

## How to Obtain Example Images

For research purposes, consider:

1. **Public Domain Manga:**
   - Pre-1928 works (in most jurisdictions)
   - Explicitly released public domain manga
   - Creative Commons licensed webcomics

2. **Licensed Content:**
   - Contact publishers for academic use permissions
   - Many publishers grant research exemptions
   - Include proper citation and attribution

3. **Original Creation:**
   - Commission original manga-style artwork
   - Create synthetic examples for demonstration
   - Use AI-generated manga-style images (clearly labeled)

4. **Fair Use (Academic Context):**
   - Limited excerpts for research and education
   - Proper attribution and citation
   - Non-commercial academic use
   - Consult institutional legal guidelines

## Processing Notes

When creating examples:

### Input Requirements:
- **Format:** JPG or PNG
- **Resolution:** Minimum 1200px height
- **Color:** Grayscale or color (system handles both)
- **Quality:** High-quality scans preferred
- **Orientation:** Portrait (standard manga page)

### Expected Output:
- **Format:** Same as input
- **Resolution:** Maintained from input
- **Quality:** Minimal compression artifacts
- **Text:** Clear, readable English translations
- **Style:** Visually consistent with original

## Example Processing Workflow

```bash
# Example command (if AutoScan were a command-line tool)
$ autoscan translate --input examples/input/sample_page_01.jpg \
                     --output examples/output/sample_page_01_translated.jpg \
                     --source-lang ja \
                     --target-lang en \
                     --preserve-style

Processing: sample_page_01.jpg
├─ Text Detection: 6 regions found (2.1s)
├─ Bubble Detection: 5 bubbles segmented (1.8s)
├─ OCR: 125 characters recognized (0.4s)
├─ Translation: 6 text blocks translated (0.3s)
└─ Rendering: English text integrated (0.2s)

Completed in 4.8 seconds
Output saved to: examples/output/sample_page_01_translated.jpg
```

## Quality Assessment

Each output should be evaluated on:

1. **Detection Quality:**
   - All text regions correctly identified
   - Bubble boundaries accurately segmented
   - No false positives

2. **Translation Quality:**
   - Accurate meaning preservation
   - Natural English phrasing
   - Appropriate tone and style

3. **Visual Quality:**
   - Font style matches original aesthetic
   - Text properly fits within bubbles
   - No artifacts or rendering errors
   - Maintains image quality

4. **Overall Usability:**
   - Readable and clear
   - Visually appealing
   - Professional appearance

## Viewer Tools

To compare input/output pairs:

```python
# Simple Python script to view comparisons
import matplotlib.pyplot as plt
from PIL import Image

def compare_translation(input_path, output_path):
    img_input = Image.open(input_path)
    img_output = Image.open(output_path)
    
    fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(15, 10))
    
    ax1.imshow(img_input)
    ax1.set_title("Original (Japanese)")
    ax1.axis('off')
    
    ax2.imshow(img_output)
    ax2.set_title("Translated (English)")
    ax2.axis('off')
    
    plt.tight_layout()
    plt.show()

# Usage
compare_translation(
    'examples/input/sample_page_01.jpg',
    'examples/output/sample_page_01_translated.jpg'
)
```

---

## Placeholder Examples

Since actual copyrighted manga images cannot be included without permission, this repository includes placeholder descriptions. To fully populate this directory:

1. Obtain properly licensed manga pages
2. Process through AutoScan system
3. Save both input and output
4. Include metadata files
5. Add comparison visualizations

**For reviewers/collaborators:** Access to actual example images can be provided separately for research evaluation purposes under appropriate confidentiality agreements.

---

*This README describes the structure and content that example images should have. Actual images should be added following copyright guidelines and proper licensing.*
