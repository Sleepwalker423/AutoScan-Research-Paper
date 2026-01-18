# AutoScan: Leveraging Computer Vision to Improve Comic Book Translations and Editing

## Authors

**Kino Leonhart**  
Department of Electrical Engineering and Computer Science  
Embry-Riddle Aeronautical University  
Daytona Beach, USA  
leonhartkino@gmail.com

**Lynn Vonderhaar**  
Department of Electrical Engineering and Computer Science  
Embry-Riddle Aeronautical University  
Daytona Beach, USA  
vonderhl@my.erau.edu

**Juan Couder**  
Department of Electrical Engineering and Computer Science  
Embry-Riddle Aeronautical University  
Daytona Beach, USA  
ortizcoj@my.erau.edu

**Charles Walker**  
Department of Electrical Engineering and Computer Science  
Embry-Riddle Aeronautical University  
Daytona Beach, USA  
walkec45@my.erau.edu

**Omar Ochoa**  
Department of Electrical Engineering and Computer Science  
Embry-Riddle Aeronautical University  
Daytona Beach, USA  
ochoao@erau.edu

---

## Abstract

As Machine Learning (ML) becomes ever more ubiquitous, it offers promise in automating many previously time-consuming processes. The applications are widespread, including the potential for making literature more accessible. This work focuses on the challenges facing the manga community regarding translation and widespread dissemination of the thousands of available comics. Not only is translating and editing manga time-consuming, but translated copies are often unofficial, so they may be subject to individuality in translation as opposed to an official and standard translation. Although some existing literature addresses automatic translation of manga, it lacks some details that lend additional meaning to the story, e.g., varying fonts and image context. The purpose of this work is to apply computer vision technology to advance the domain of automated manga translation by maintaining variating fonts and leveraging generative Artificial Intelligence (AI) tools to help automatic translations be truer to the original.


### Keywords
machine learning, manga, machine translation, comic book translation, generative artificial intelligence

---

## I. Introduction

The use of Machine Learning (ML) in literature offers a solution to time-consuming translations, including those of manga series. There are tens of thousands of manga series, but due to the high cost of translation, most have not been translated from their original language. This not only limits how many readers can enjoy the untranslated series but also leads to many unofficial translations that may be viewed millions of times. Machine Translation (MT) offers the ability to quickly put official translations on the market.

Existing literature discusses various solutions in making MT a reality including character tracking, translations utilizing scene context, and even fully automated translation pipelines. However, MT still faces many performance challenges in staying true to the original comic because of the need to utilize rich contextual information for a truer translation. Many of these challenges arise from needing to not only translate words, but also incorporate visual context into the translation process, making manga especially difficult to translate.

This work introduces AutoScan, which brings the manga community one step closer to fully automated and accurate MT of series. AutoScan not only offers fully automatic MT by utilizing computer vision technology, but also brings attention to additional visual cues, e.g., font consistency, to make the translations better representations of the original comics.

### Contributions

The contributions of this work are as follows:

1. Summarizing the leading contributions to automatic manga translation.
2. Presenting AutoScan, an automatic manga translator based on the compilation of leading contributions.
3. Implementing font consistency using multiple classes for speech bubbles.
4. Exploring generative fill technology for improving the cleaning process before translation.
5. Identifying current challenges and future directions for automatic manga translation.

---

## II. Background

Before describing the approach, it is critical to first explain object detection and Optical Character Recognition (OCR), two critical components of AutoScan.

### A. Object Detection

Object detection is an ML classification task that aims to locate specified classes within an image. Older methods use a Convolutional Neural Network (CNN) with a sliding window to locate the class instances within an image, but this can be relatively time-consuming. By contrast, the You Only Look Once (YOLO) models are CNNs that divide the image into a grid and any cell that includes the center of an object must detect that object with a confidence score. This means, as the name implies, that the YOLO model only needs to scan the image once to locate the desired classes within it, making it much faster than other popular methods.

### B. Optical Character Recognition

OCR is a method by which an ML model extracts text from an image. There are many potential applications of OCRs including converting the text in scanned documents to a digital format and, in the case of this work, finding text within a manga page for translation. OCRs may be trained to pick up different alphabets and languages, or even handwritten text. OCR is accomplished in four main steps:

1. **Pre-processing**: During this phase, the OCR reduces noise within the image. This phase is occasionally placed after the other three phases.
2. **Segmentation**: This is the process of detecting text lines within the rest of the image.
3. **Feature extraction**: This phase combines text lines into feature vectors for classification.
4. **Classification**: The OCR classifies the feature vectors into recognizable characters within an alphabet.

---

## III. Existing Solutions

The use of ML to aid manga translation has garnered much attention in the research community with contributions spanning various challenges facing automatic translation. Because of the complexity of the task, many authors focus on improving one aspect of the pipeline at a time. For instance, some authors focus on character and speech recognition. Various researchers have provided methods for extracting text from non-flat manga, speaker association, and using segmentation to locate speech bubbles.

Some authors do provide approaches to automatic manga translation. Notable work includes:
- Context-aware pipelines that translate comics utilizing the context of the frame and other bubbles
- Pipelines for extracting text and contributing it to characters for automatically writing manga transcripts for the visually impaired
- Utilizing longer contextual windows using previous scenes to improve translation
- Using bibliographic information including the title and author name
- Multimodal Large Language Models (LLMs) for improved context

All of this existing work helps with the process of accurate automatic translation of manga. However, none have maintained the varying fonts within the translated version, thereby losing some vital meaning.

---

## IV. Approach

Machine Translation for manga can be broken down into four main steps:

1. Locating text within the frames
2. Extracting text from the frames
3. Translating the text
4. Editing the frames to include the new translations

### Speech Bubble Detection and Classification

In this work, locating the text within the frames utilizes the **YOLOv8 model by Ultralytics** to classify the speech bubbles. For this application, **1,062 images of manga pages** were annotated using Roboflow to adapt the model to this application via transfer learning.

There are several types of speech bubbles which often have different fonts to reflect the sentiment of the scene. The following types of speech bubbles were tracked as different classes as part of font maintenance:

- Regular speech bubbles
- Shout bubbles
- Narrative bubbles
- Happy bubbles
- Evil bubbles
- Thought bubbles
- Scared bubbles

After training, the model achieved a **0.724 mean Average Precision (mAP) at Intersection over Union (IoU) 0.50**. This means that the bubble location is accepted when the predicted box overlaps with the ground truth box with an IoU of 0.50 or greater. The bubble location step also saved the (x, y) coordinates for each corner of the bounding box within the page, which are used in a later step for cropping and editing the translated text. The bubble is then cropped from the image and saved for the OCR step.

### Text Extraction

**Manga OCR** is then used to extract the text from the regions classified as bubbles by the YOLOv8 model. Several OCRs were tried, but Manga OCR had the best success rate. Tested OCRs included:
- Tesseract
- EasyOCR
- Google Cloud Vision (GCV)
- Manga OCR (selected)

Manga OCR is a custom model trained for manga based on **Microsoft's TrOCR**. Manga OCR is only trained for Japanese, but it handles Japanese well, including text with Furigana (pronunciation guides) and text on top of images. However, experiments show some difficulty in handling Furigana-heavy text, where it sometimes understands the Furigana as normal speech. Additionally, Manga OCR does not track text metrics (e.g., location of the text), which is helpful for masking and editing the text in the final editing step.

### Translation

Step three is the translation step. AutoScan uses **DeepL** because of its superior performance compared to other translators like Google Translate. Currently, AutoScan takes each cropped bubble, runs Manga OCR on it, translates the characters to English, and then sends them to the editing step. However, a more accurate translation would come from a complete script that incorporates context into the translation.

### Image Editing

The final step is the editing step. Editing uses digital technology to target and erase areas in which the previous text is present. This involves removing the Japanese characters and replacing them with the translated characters. AutoScan uses **Adobe's generative fill tool** for the editing step. This is helpful because it does not ignore the background but rather extends it to make the new image more natural.

This process is currently manual, but future iterations of AutoScan will automate this step using models from **Google's Vertex AI**. It is important to note that although Manga OCR is more accurate than GCV for this application, GCV also outputs a text mask, which would offer an ideal way to target areas for erasing.

The translated text from step three is then inputted into the newly cleaned area in the image with the bounding box being the maximum size for the text to fit. **By utilizing the various types of speech bubbles, the pipeline is able to input the English text in font types that match the original text, thereby maintaining additional context from the original comic.**

---

## V. Discussion

The four-step process described in the approach section is a conglomerate of the state-of-the-art in MT for manga. This work then builds on the state-of-the-art to add font consistency and generative filling outside of speech bubbles.

However, there are still challenges and areas for future improvement. For instance, there is some existing literature on ordering speech bubbles so that scenes and longer scripts can be translated together. This helps to improve the transcription as opposed to translating a single speech bubble at a time without any context. Currently, AutoScan translates bubble-by-bubble, but incorporating methods for bubble ordering would be beneficial. This would likely include additional steps, e.g., frame tracking.

As the paper shows, text may occur outside of speech bubbles. In this case, Adobe's generative fill tool was used to continue the background before overlaying the new, translated text. With the rise of generative Artificial Intelligence (AI), e.g., diffusion models, this step could easily be automated in the future.

---

## VI. Challenges and Future Directions

AutoScan offers a promising direction for comic book transcription. However, there are some clear directions for future work:

1. **Character Tracking**: Adding character tracking to AutoScan would enable the building of character personas, potentially with the help of LLMs, which can be used to generate fan-based stories about the characters.

2. **Emotion Analysis**: Combining character tracking and emotional analysis would help in generating better translations because it adds further context about the speaker, the subject, and the storyline.

3. **Bubble Ordering**: Implementing methods for ordering speech bubbles for contextual translation of complete scenes.

4. **Automated Generative Fill**: Using Google's Vertex AI or similar tools to automate the cleaning process.

5. **Context-Aware Translation**: Incorporating longer scripts and context into the translation process for improved accuracy.

---

## VII. Conclusion

The purpose of this work was to add to the existing literature on the subject of automatic translation and editing for manga series. Existing literature spans many aspects of this area, but this work consolidates the state-of-the-art in the area into a new pipeline called AutoScan.

AutoScan follows the four main steps of automated MT and editing for comics and brings attention to font consistency as important visual context in producing a high quality comic translation. This work also discusses tools for generative filling of background when cleaning a page for translation. Finally, this work discusses future work in character tracking to build character personas and improve the overall quality of the translations.

---

*For the complete research paper with all figures, tables, and detailed references, please refer to `AutoScan_Paper.pdf` in this directory.*
