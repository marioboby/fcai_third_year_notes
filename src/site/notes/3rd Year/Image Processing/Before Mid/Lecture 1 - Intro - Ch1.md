---
{"dg-publish":true,"permalink":"/3rd-year/image-processing/before-mid/lecture-1-intro-ch1/","dg-note-properties":{}}
---


> One picture is worth more than ten thousand words
## 1. Core Definitions

- **Computer Imaging:** The acquisition and processing of visual information by a computer. It is divided into two categories based on the ultimate receiver of the information: ==***Computer Vision and Image Processing***== .

- **Digital Image:** A representation of a two-dimensional image as a finite set of digital values.

- **Pixels:** The individual picture elements that make up a digital image. Pixel values represent intensity or gray levels.

- **Digitization:** Implies that a digital image is merely an approximation of a real scene.

- **Common Formats:** Images can be grayscale (1 sample per point) or RGB (3 samples per point: Red, Green, and Blue).

## 2. The Processing Continuum

The field transitions from simple image manipulation to deep understanding, broken into three levels:

- **Low-Level Process:** Takes an ==image as input and outputs an image== (e.g., noise removal, image sharpening).

- **Mid-Level Process:** Takes an ==image as input and outputs attributes== (e.g., object recognition, segmentation). _Note: This course will stop at this level_.

- **High-Level Process:** Takes ==attributes as input and outputs understanding== (e.g., scene understanding, autonomous navigation).

### Differentiating the Fields

- **Image Processing:** Focuses on processing images for human consumption (Image In $\rightarrow$ Image Out).

- **Computer Vision:** Focuses on processing output images for computer use .

- **Image Analysis:** Extracts measurements from an image to solve vision problems (Image In $\rightarrow$ Measurements Out). It involves feature extraction (acquiring high-level info) and pattern classification (identifying objects) .

- **Image Understanding:** Maps an image to a high-level description (Image In $\rightarrow$ High-level description Out).

---

## 3. History of Digital Image Processing

- **1920s:** Early applications were in the newspaper industry. The Bartlane system used submarine cables to transfer 5-level coded pictures between London and New York . This was later improved to 15-tone images.

- **1960s:** The space race and improvements in computing led to a surge in DIP. Computers were used to improve images of the moon taken by the Ranger 7 probe in 1964.

- **1970s:** DIP expanded into medical applications. Notably, Sir Godfrey N. Hounsfield and Prof. Allan M. Cormack won a Nobel Prize in 1979 for inventing tomography, leading to CAT scans.

- **1980s - Today:** The use of DIP techniques has exploded across various industries.

---

## 4. State-of-the-Art Applications

Digital image processing is utilized in highly diverse fields :

- **Image Enhancement:** Improving quality and removing noise, famously used to correct the early images from the Hubble Telescope .

- **Medicine:** Analyzing MRI scans to find boundaries between different tissue densities .

- **Artistic Effects**: Used to make images more visually appealing ,to add special effects and to make composite image.

- **GIS (Geographic Information Systems):** Manipulating satellite imagery for terrain classification, meteorology, and mapping night-time lights of the world .

- **Industrial Inspection:** Using machine vision to replace slow, expensive human operators, such as checking printed circuit boards (PCBs) for missing components and acceptable solder joints .

- **Law Enforcement:** Utilizing automated number plate recognition, fingerprint recognition, and CCTV enhancement .

- **Human-Computer Interfaces (HCI):** Developing natural interfaces via face and gesture recognition .

---

## 5. Key Stages in Digital Image Processing

A complete DIP pipeline involves several interconnected stages:

- **Image Acquisition:** The initial capture of the visual problem domain.

- **Image Enhancement:** Taking an image and improving its visual appearance.

- **Image Restoration:** Reversing known or estimated degradation to return an image to its original appearance .

- **Morphological Processing:** Extracting image components useful for representing shape, such as boundaries, skeletons, and convex hulls .

- **Segmentation:** Subdividing an image into its constituent parts or objects.

- **Object Recognition:** Identifying the segmented objects within the image.

- **Representation & Description:** Describing the recognized objects for further processing.

- **Image Compression:** Reducing the massive amount of data required to store or transmit an image.

- **Color Image Processing:** Handling the unique properties of colored components.