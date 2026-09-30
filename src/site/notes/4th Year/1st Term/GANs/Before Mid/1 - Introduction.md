---
{"dg-publish":true,"permalink":"/4th-year/1st-term/ga-ns/before-mid/1-introduction/","dg-note-properties":{}}
---


> [!Abstract]
> This foundational lecture introduces the philosophy and mechanics of deep generative models, positioning them as complex data simulators capable of capturing high-dimensional probability distributions. By learning to generate realistic data across modalities like images, text, and audio, these models demonstrate a deep mathematical understanding of the underlying signals. This course provides a rigorous mathematical framework for the representation, learning, and inference processes driving today’s most advanced AI systems.

  

## The Fundamental Challenge of Artificial Intelligence

Artificial intelligence subfields—computer vision, natural language processing (NLP), and robotics—all share a common bottleneck: making sense of complex, high-dimensional signals.

  

- **Analogy:** From the perspective of a computer, an image is nothing more than a massive matrix of raw numbers. The fundamental difficulty of AI is mapping that meaningless matrix into a structured representation that is useful for decision-making (e.g., identifying objects, determining materials, calculating velocity).
<br>
- Understanding these signals is philosophically complex, but generative models tackle it through creation.
<br>
- **Analogy:** The physicist Richard Feynman wrote on his whiteboard, "What I cannot create, I do not understand." While he was referring to deriving mathematical theorems, this is the exact philosophy driving generative AI.
<br>
- **Example:** If you claim to understand what an apple is, you should be able to picture one in your head. If you claim to understand Italian, you must be able to generate coherent Italian sentences.
<br>
- By forcing a system to generate coherent outputs (like ChatGPT writing text), we force it to develop an internal understanding of grammar, common sense, and the physics of the real world.
    
      
    

## Inverse Graphics vs. Statistical Modeling

Generating images from code is not a new problem; the computer graphics industry has done this for decades. However, our approach in this course fundamentally differs from traditional graphics.

  

- _The Graphics Approach:_ Relies on heavy priors. You provide a high-level description (scene layout, shapes, colors), and a renderer uses hard-coded rules about physics and light transport to generate the image.
  <br>
- _Inverse Graphics:_ The theoretical goal of computer vision—taking a raw image and inverting the rendering process to deduce the high-level scene description.
  <br>
- In this class, we discard the hard-coded physics priors and instead build **Statistical Generative Models**: Machine learning architectures driven primarily by vast datasets rather than pre-programmed physical laws.

![Pasted image 20260925224932.png](/img/user/imgs/Pasted%20image%2020260925224932.png)

![Pasted image 20260925225758.png](/img/user/imgs/Pasted%20image%2020260925225758.png)

- We frame these models mathematically as **Probability Distributions**: A function that takes a complex input $X$ (an image or sequence of text) and maps it to a scalar value representing how likely that specific input is under the model's parameters.
  <br>
- Ultimately, we are building controllable **Data Simulators**. In traditional machine learning, data is the input. Here, data is the _output_. We sample from our learned probability distributions to simulate new data points.
    
      
    

## Controllability and Multi-Modal Applications

The true power of a generative data simulator is that it can be steered. By injecting control signals into the sampling process, we can solve complex downstream tasks.

  

- **Example:** Feeding text in Chinese (the control signal) into a simulator trained to generate English text effectively creates a machine translation system.
    
      
    

### Image Generation and Editing

- Over the past decade, image generation has evolved from blurry, low-resolution black-and-white faces produced by early GANs to photorealistic synthesis.
    
      
    
- **Score-Based Diffusion Models**: A breakthrough model architecture (developed heavily here at Stanford) that drives modern text-to-image systems like Stable Diffusion, Midjourney, and DALL-E.
    
      
    
- **Example:** Prompting a model with "an astronaut riding a horse." The model has likely never seen this specific training pair, but by understanding the isolated concepts of "astronaut," "horse," and "riding," it can synthesize them perfectly, proving deep semantic comprehension.
    
      
    
- **Example:** Generating a "perfect Italian meal" yields stochastically different images upon every sample, capturing nuanced details like the architectural style of buildings out the window.
    
      
    
- **Example:** Controlling a model to generate a highly complex prompt like "a teddy bear wearing a costume in front of the Hall of Supreme Harmony singing Beijing Opera."
    
      
    
- Because generative models map the relationships between pixel values, they seamlessly solve inverse imaging problems:
    
      
    - **Example:** **Super-resolution** (mapping low-resolution inputs to high-resolution outputs).
        
          
        
    - **Example:** **Colorization** (mapping grayscale structures to colorized equivalents).
        
          
        
    - **Example:** **Inpainting** (predicting and filling in masked or missing pixels based on surrounding context).
        
          
        
    - **Example:** Using _SDEdit_ to take a rough user sketch (control signal) and force the model to synthesize a realistic masterpiece adhering to the stroke layout.
        
          
        
    - **Example:** Text-based editing, such as instructing the model to "spread the wings" of a bird, make two subjects kiss, or open a closed box in a source image.
        
          
        

### Medical Imaging and Anomaly Detection

- Generative models directly improve human health and safety by operating on non-visual sensor data.
    
      
    
- **Example:** Using raw signals from an MRI or CT scanner as a control signal to generate the final medical image. This allows doctors to achieve high-fidelity scans using drastically fewer measurements, directly reducing a patient's radiation exposure.
    
      
    
- **Example:** **Outlier Detection**. A self-driving car can evaluate a traffic sign against its learned probability distribution. If an adversarial attacker has altered the sign, the generative model will flag it as a highly improbable image and transfer control to a human.
    
      
    

### Audio, Text, and Code

- Early deep learning audio models (like WaveNet in 2016) were revolutionary but robotic. Modern autoregressive and diffusion variants capture nuance, emotion, and natural accents.
    
      
    
- **Example:** Audio super-resolution can take low-fidelity, muffled speech (like a bad phone connection) and hallucinate the missing frequencies to produce studio-quality sound.
    
      
    
- Large Language Models (LLMs) operate by learning probability distributions over text sequences.
    
      
    
- **Example:** While a 2019 model could only offer generic advice on how to pass CS 236, modern ChatGPT understands the contextual reality that CS 236 is the Deep Generative Models course at Stanford, and provides highly specific, targeted study advice without being explicitly prompted.
    
      
    
- Because programming code is fundamentally just text syntax, these text distributions apply directly to software engineering.
    
      
    
- **Example:** Providing a natural language description of a function's intent and using the model to autocomplete the syntactically valid Python code body.
    
      
    

### Video, Robotics, and the Sciences

- Video generation treats footage as a temporal stack of continuous images.
    
      
    
- **Example:** Generating a coherent short clip of a couple sliding down a snowy hill, or stitching multiple generated clips together to form a cohesive narrative.
    
      
    
- Robotics and decision-making can be framed as sequence generation.
    
      
    
- **Example:** **Imitation Learning**. Taking human examples of good behavior (safe driving trajectories, optimal robotic arm movements for stacking objects) and generating novel actions that achieve the same goals.
    
      
    
- In the hard sciences, generative models are designing the physical world.
    
      
    
- **Example:** Synthesizing novel 3D molecular structures, designing proteins with specific properties, or generating drug compounds tailored to bind to specific virus structures like COVID-19.
    
      
    

## The Three Mathematical Pillars of the Course

To build these data simulators, we must solve three rigorous mathematical challenges:

  

### 1. Representation

- How do we use neural networks to parameterize probability distributions over millions of interacting random variables (pixels, words)?
    
      
    
- Simple statistical distributions (like Gaussians) fail in high dimensions. We must design clever architectures that capture exactly how different pixels or words condition each other.
    
      
    

### 2. Learning

- How do we fit our chosen representation to the actual data?
    
      
    
- This involves choosing **Loss Functions**—mathematical metrics that measure the distance between the data's true distribution and the model's generated distribution.
    
      
    
- Because measuring similarity between complex, high-dimensional distributions is incredibly difficult, different model families use fundamentally different optimization strategies to pull the model distribution closer to the data distribution.
    
      
    

### 3. Inference

- Once trained, how do we efficiently sample from these models to generate new data?
    
      
    
- How do we invert the generative process to extract **Representations** (features)? By clustering data points with similar latent meanings, we can perform unsupervised learning on data lacking explicit labels.
    
      
    

## Taxonomy of Deep Generative Models

We will cover the specific mathematical trade-offs of the dominant model architectures:

  

- **Autoregressive & Flow-Based Models**: Architectures that provide direct, exact access to the data's likelihood. (Used heavily in LLMs).
    
      
    
- **Latent Variable Models**: Architectures (like Variational Autoencoders) that introduce hidden variables to increase expressive power, utilizing hierarchical variational inference.
    
      
    
- **Implicit Generative Models**: Architectures (like GANs) that abandon likelihood calculations entirely. Instead, they explicitly represent the _sampling process_. While they generate data quickly, they cannot use standard maximum likelihood estimation for training, requiring volatile two-sample tests instead.
    
      
    
- **Energy-Based & Diffusion Models**: The current state-of-the-art for continuous data generation. We will cover how they function, their connections to latent variables, and their mathematical training stability.
    
      
    

> [!Note]
> 
> If you take one thing away from today, it is this: creating is the ultimate proof of understanding. For decades, machine learning focused on discriminative tasks—drawing boundaries to separate cats from dogs. Deep generative models flip that paradigm. We are no longer just labeling the world; we are building mathematical engines capable of simulating it. By mastering the representations, learning objectives, and inference algorithms in this course, you are acquiring the toolkit to build the next generation of systems that reason, design, and create.