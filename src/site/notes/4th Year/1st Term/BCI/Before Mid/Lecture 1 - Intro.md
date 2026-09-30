---
{"dg-publish":true,"permalink":"/4th-year/1st-term/bci/before-mid/lecture-1-intro/","dg-note-properties":{}}
---



## The Golden Circle of BCI

**WHY? (Purpose and Applications)**

  

- BCI aims to understand and utilize brain activity through both invasive (intracranial/iEEG) and non-invasive (EEG) methods.
<br>
- Practical applications for these interfaces include motor imagery, emotion classification, detection of sleep disorders, and eye tracking.
    
      
    

**HOW? (Methodology and Processing)**

  

- **Experimental Design:** Researchers must understand different brain areas, determine the sources of activity, and design structured experiments (such as monitoring subjects during wake/sleep cycles or conducting explicit motor imagery tests).
  <br>
- **Data Acquisition:** Brain activity is recorded using equipment like EEG head caps, amplifiers, and control boxes, alongside supplementary sensors like EOG (for eye movement) and EMG (for muscle activity).
<br>
- **Pre-processing & Feature Extraction:** The raw data must be segmented and filtered to remove noise and outliers. Key features are then extracted using statistical measures (mean, median, variance) and other metrics (power spectrum, time-domain features, phase).
<br>
- **Classification:** Supervised linear and non-linear machine learning models are used to classify the extracted features into distinct intents.
    
      
    

**WHAT? (Practical Outputs)**

  

- The ultimate goal is to translate the classified brain signals (such as a subject imagining moving their left or right hand) into actionable commands to control external devices, like a robot moving left.
<br>
- A successful example of this pipeline is the P300 speller, which can achieve 97% accuracy.
    
      
    

## Biological and Neurological Fundamentals

To successfully interface with the brain, the lecture covers fundamental neuroscience and cognitive electrophysiology—the study of how cognitive functions like emotions, memory, and behavior control are implemented by electrical neuronal activity.

  

- **The Neuron:** The human brain contains approximately 86 billion neurons. Each neuron consists of dendrites, a cell body, an axon, and axon terminals.
<br>
- **Brain Regions:** Different cognitive and physical functions are localized to specific lobes:
    
      
    - **Frontal Lobe:** Motor control, problem-solving, and speech production.
        
          
        
    - **Parietal Lobe:** Touch perception and body orientation.
        
          
        
    - **Temporal Lobe:** Auditory processing, memory, and language comprehension.
        
          
        
    - **Occipital Lobe:** Visual reception and interpretation.
        
          
        
    - **Inner Structures & Brainstem:** Deeper structures like the thalamus and hippocampus play crucial roles, while the brainstem and cerebellum handle involuntary responses and balance.
        
![Pasted image 20261001002606.png](/img/user/imgs/Pasted%20image%2020261001002606.png)      
In the context of this slide, "multivariate data" refers to the fact that brain activity is not a single, unified signal, but a complex dataset composed of multiple variables happening simultaneously across different spatial locations.

When building machine learning models for brain-computer interfaces, the extracted data is inherently multidimensional because cognitive functions are highly localized. The slide illustrates this by breaking down the brain into distinct regions:

- **Spatial Variables:** Data collected from the brain varies drastically depending on the physical location of the sensor. For example, sensors placed over the **Frontal Lobe** will capture variables related to motor control and problem-solving, whereas sensors over the **Occipital Lobe** will capture visual processing.
    <br>
- **Functional Variables:** The brain processes different types of information in parallel. A subject might be processing auditory information in the **Temporal Lobe** while simultaneously processing touch perception in the **Parietal Lobe**.
    

The slide uses this anatomical map to demonstrate that analyzing brain signals means processing multiple distinct "features" or variables at once, rather than looking at a single, simple time-series output.

![Pasted image 20261001001533.png](/img/user/imgs/Pasted%20image%2020261001001533.png)

This slide, titled "Cognitive electrophysiology," illustrates the multidisciplinary nature of the field by presenting it as a spectrum bridging two main disciplines:

  

- **The Cognitive (Psychology) End:** On the left side of the spectrum, the focus is on psychological aspects. The slide notes that readings extracted from the brain are more sensitive indicators of cognitive processes than traditional behavioral measures, such as a subject's reaction time.
<br>
- **The Electrophysiology (Neuroscience) End:** On the right side, the focus shifts to neuroscience. Researchers on this end of the spectrum are primarily interested in studying raw brain activity and neural networks, with the goal of modeling this physical behavior.
    
      
    

The annotations and hand-drawn lines on the slide emphasize the contrast between studying outward behavioral measures and inward physiological signals (depicted by the squiggly waveform drawing).

> [!ايوه يعني ايه برضو]
> Think of the spectrum of cognitive electrophysiology like studying a complex computer system from two distinct angles: the software versus the hardware.
> 
>   
> 
> - **The Cognitive Angle (Psychology):** This side focuses on the "software"—understanding abstract mental processes like decision-making, attention, or memory. Traditionally, psychologists rely on outward behavioral measures, like timing how long it takes a subject to press a button (reaction time). However, extracting direct electrical readings from the brain provides a much more sensitive and immediate measure. For example, researchers can detect the exact millisecond a subject's brain registers a visual anomaly, long before their finger physically reacts to press a button.
> <br>
> - **The Electrophysiology Angle (Neuroscience):** This side focuses on the "hardware"—the physical wiring and biological circuitry. Researchers at this end of the spectrum are primarily interested in the raw brain activity and the mechanics of neural networks. Rather than decoding the abstract thoughts a person is having, their goal is to map how populations of neurons fire, how electrical waves propagate across the cortex, and to build mathematical models of this biological behavior.
>     
>       
>     
> 
> In short, the cognitive side uses brain signals as a tool to decode human thoughts and psychology, while the electrophysiological side studies those exact same signals to map the biological machine generating them.

## EEG Capabilities and Challenges

The lecture heavily focuses on Electroencephalography (EEG) as the primary non-invasive BCI tool.

  

- **Comparison with Other Modalities:** Compared to Functional Magnetic Resonance Imaging (fMRI), which offers excellent spatial resolution, EEG is prized for its high temporal resolution (capturing data down to the 10ms scale) and lower cost.
<br>
- **Signal-to-Noise Ratio (SNR):** A major challenge with EEG is its low SNR. The electrical signals generated by active synapses must travel through multiple anatomical layers—including the pia mater, subarachnoid space, arachnoid, dura mater, skull, and scalp—before reaching the electrode. This results in "over hearing," where electrodes pick up a blurred mixture of signals.
<br>
> **Signal-to-Noise Ratio (SNR)** In EEG, it represents the clarity of the target brain signal compared to background electrical noise.
> 
> EEG typically struggles with low SNR because the electrical signals generated by active synapses must travel through multiple anatomical barriers—including the pia mater, subarachnoid space, arachnoid, dura mater, skull, and scalp—before reaching the surface electrode. This physical distance causes the signals to spread and blur, leading to an effect called "over hearing," where an electrode picks up a mixed, noisy blend of activity from many different areas rather than a clear, isolated signal.

- **When to Avoid EEG:** EEG is not universally ideal; it should be avoided if target frequencies are too low, if trials are highly jittered, or if other measurement tools are better suited to the specific research question.
<br>

> - **When other measures are more suitable:** EEG only captures electrical activity that manages to penetrate the skull, resulting in a blurry surface map. If the objective is to pinpoint the exact 3D anatomical origin of a neural event deep within the brain, EEG lacks the necessary spatial resolution. In these cases, a tool like fMRI is required to track precise spatial changes.
>  <br>
> - **When the frequency is low:** Low-frequency brain waves have very long wavelengths. To capture enough complete cycles for accurate signal processing and feature extraction, you need long, uninterrupted recording windows. Furthermore, low-frequency EEG data is highly susceptible to baseline drift and artifacts from slow physical movements (like sweat potentials or slight shifts in the electrode cap), making it difficult to isolate the true neural signal from the noise.
><br>
> - **When the trials are jittered:** In BCI pipelines, improving the low Signal-to-Noise Ratio often involves averaging the time-series data from dozens of repeated trials to isolate a clear response. If the timing of the event is "jittered"—meaning the exact millisecond the signal fires shifts slightly across different trials—the peaks and troughs of the electrical waves will not align perfectly. When you average these misaligned arrays, the positive and negative voltages cancel each other out, destroying the temporal features your classification models rely on.

- **Multivariate Dimensions:** EEG data is highly complex and multi-dimensional, requiring analysis across space, time, frequency, and phase.
    
      

> [!Comparisons]
> **Invasive vs. Non-Invasive BCI**
> 
>   
> 
> - **Invasive:** Sensors are placed surgically inside the skull to record brain activity directly, such as Intracranial EEG (iEEG).
>     
>       
>     
> - **Non-Invasive:** Sensors record brain signals from outside the head without surgical intervention, such as standard EEG.
>     
>       
>     
> 
> **Comparison of Brain Imaging Tools**
> 
>   
> 
> - **EEG (Electroencephalography):** Records electrical activity using electrodes placed on the scalp. It offers excellent temporal resolution (capturing rapid changes down to 10 milliseconds) but has low spatial resolution, making it difficult to pinpoint the exact deep-brain source of the signal.
>     
>       
>     
> - **fMRI (Functional Magnetic Resonance Imaging):** Measures brain activity by detecting changes associated with blood flow. It provides excellent spatial resolution (accurately mapping exact anatomical locations) but has poor temporal resolution, as it takes seconds to capture an image.
>     
>       
>     
> - **MEG (Magnetoencephalography):** Records the magnetic fields produced by the brain's natural electrical currents. It shares the high, millisecond-level temporal resolution of EEG, while offering a spatial capability that falls roughly between EEG and fMRI.
> 
> **Temporal Resolution** refers to precision in time—how quickly and frequently a measurement tool can capture changes in brain activity. High temporal resolution means the device can record very rapid, split-second neural events as they happen. For example, EEG and MEG have excellent temporal resolution, capable of capturing continuous data on a 10-millisecond scale.
> 
>   
> 
> **Spatial Resolution** refers to precision in space—how accurately a measurement tool can pinpoint the exact physical location or anatomical source of the brain activity. High spatial resolution means the device can clearly map exactly where the signals are originating from deep within the brain's structures. For example, fMRI provides highly detailed spatial images to locate specific active regions, but it takes seconds to capture those images, giving it poor temporal resolution.
## The 10-20 System for Electrode Placement

To accurately map the spatial dimension of EEG data, researchers use the standardized 10-20 system.

  

- The system uses anatomical landmarks: the nasion (bridge of the nose) and the inion (back of the head).
<br>
- Electrodes are spaced at distances of 10% or 20% of the total distance between these landmarks.
<br>
- Electrodes are labeled based on their location (e.g., F for Frontal, C for Central, P for Parietal) and a number. Odd numbers represent the left hemisphere, even numbers represent the right hemisphere, and a "z" indicates the midline.
    
      
    

## Interdisciplinary Foundations

Building and analyzing BCI systems requires a multidisciplinary approach, relying heavily on signal processing, machine learning, deep learning, linear algebra, and statistical analysis. The broader contents of the course will cover advanced analytical techniques, including Fourier transforms, Event-Related Potentials (ERP), spatial filtering, and Riemannian geometry.