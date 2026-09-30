---
{"dg-publish":true,"permalink":"/3rd-year/supervised/after-mid/lecture-8-speech-and-rn-ns/","dg-note-properties":{}}
---


### 1. Supervised Learning & CNN Basics

The lecture begins with a brief review of foundational concepts.

- **Supervised Learning:** This approach relies on a supervisor, a training dataset, and a desired output to train an algorithm. The model processes input raw data to yield categorized outputs.
    
- **Convolutional Neural Networks (CNNs):** The slides provide structural examples of CNNs, detailing layers such as `Conv2D` and `MaxPooling2D`. They also demonstrate how to calculate parameters and output shapes for convolutions and overlapping max pooling based on kernel sizes, padding, and strides.
    

### 2. Digital Voice Analysis

This section explains how human speech is captured and processed by machines.

- **Human Hearing & Speech:** Humans can hear frequencies ranging from 20 Hz up to 20,000 Hz (20 kHz). Sound below 20 Hz is known as infrasound, while sound above 20 kHz is ultrasound. Speech itself is generated in the vocal tract, utilizing the lungs, vocal cords, tongue, and lips, producing continuous analog waves.
    
- **Digital Processing Pipeline:**
    
    1. **Voice Capture:** A microphone captures analog sound waves and converts them into a digital signal using an Analog-to-Digital Converter (ADC). This is necessary because computers require discrete digital data to process signals.
        
    2. **Pre-processing:** This step improves signal quality before analysis. It involves noise reduction, framing (breaking the signal into small, overlapping segments), and windowing (applying a smoothing function to reduce edge effects).
        
    3. **Feature Extraction:** The pre-processed audio is transformed into numerical features. These features are extracted from both the Time Domain and Frequency Domain. Key time domain features include Zero Crossing Rate (ZCR), Energy, Entropy, and Autocorrelation.
        
- **Evaluation Metrics:** Speech recognition models are evaluated using standard classification metrics like Accuracy, Precision, Recall, and the $F_1$ score. Additionally, signal quality is measured using Signal-to-Noise Ratio (SNR) and Segmental SNR (SSNR).
    

### 3. Recurrent Neural Networks (RNNs)

The lecture introduces RNNs to handle sequence data, such as time-series or speech.

- **Core Mechanism:** Unlike standard networks, RNNs act as regression models that can predict future values (e.g., tomorrow's value) based on past and present inputs. The architecture can be viewed as "folded" with a loop, or "unfolded" across discrete time steps ($t-1$, $t$, $t+1$).
    
- **Weight Sharing:** A critical feature of an RNN is that it uses the exact same network architecture and the same weights ($W_{ax}$, $W_{aa}$, $W_{ya}$) across all time steps.
    
- **Internal Calculations:** The activation output at time $t$ ($a^{<t>}$) depends on both the current input ($X^{<t>}$) and the previous activation output ($a^{<t-1>}$). The network calculates the state $S_t = X_t W_{ax} + a_{t-1} W_{aa} + b_1$, applies a $\tanh$ activation function to get $a_t$, and computes the final prediction $\hat{Y}_t = a_t W_{ya}$.
    
- **Sequence Topologies:** RNNs can be adapted for various tasks:
    
    - _One-to-many:_ Image captioning.
        
    - _Many-to-one:_ Sequence classification, such as sentiment analysis.
        
    - _Many-to-many:_ Encoder-decoder architectures for language translation or named entity recognition.
        

more on it [[3rd Year/Supervised/Expanded Explanations/Lecture 8#1. Sequence Topologies\|here]]

### 4. Advanced Architectures: GRU and LSTM

To solve limitations in standard RNNs (like forgetting long-term dependencies), the lecture introduces gated architectures.

- **GRU (Gated Recurrent Unit):** The GRU modifies the RNN by adding an "Update Gate" ($G_u$). The value of this gate is determined by the current input and the previous memory cell value, trained using specific weights ($W_{ux}$, $W_{uc}$). It calculates a candidate value to decide how the memory should be updated.
    
- **LSTM (Long Short-Term Memory):** The LSTM goes a step further by splitting the GRU's Update Gate into two independent gates: an "Update Gate" and a "Forget Gate". Furthermore, LSTMs explicitly differentiate between the internal Cell Memory ($C^{<t>}$) and the visible Cell Output ($a^{<t>}$). The cell memory is squashed using a $\tanh$ function to produce the output ($a^{<t>} = \tanh(C^{<t>})$), ensuring values are bounded between -1 and 1.
    

### 5. Mathematical Example: Backpropagation Through Time (BPTT)

The document concludes with a handwritten mathematical walkthrough of training a 2-step RNN.

- **Forward Pass:** Given initial weights ($W_{ax} = 0.5$, $W_{aa} = 0.8$, $W_{ya} = 1$), the example calculates the activation outputs ($a_1$, $a_2$) and predictions ($Y_1$, $Y_2$) for two time steps. It evaluates the individual errors and the total error ($E_{total} = 0.0081$).
    
- **Backpropagation:** The example calculates the gradients of the error with respect to the output weights ($W_{ya}$) and the hidden weights ($W_{ax}$, $W_{aa}$). Using a learning rate ($\alpha = 0.1$), it updates the weights using the formula $W_{new} = W_{old} - \alpha \times \text{gradient}$.