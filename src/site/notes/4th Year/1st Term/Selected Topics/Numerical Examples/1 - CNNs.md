---
{"dg-publish":true,"permalink":"/4th-year/1st-term/selected-topics/numerical-examples/1-cn-ns/","dg-note-properties":{}}
---


# 1 - Parameter and Shape calculation

### Case Study 1: LeNet-5 (Yann LeCun et al., 1998)

Designed for handwritten digit recognition (such as reading zip codes and bank checks), **LeNet-5** established the canonical blueprint for CNNs: alternating Convolution and Average Pooling layers followed by Fully Connected layers.

  

#### Architectural Walkthrough & Parameter Audit

Let us trace the exact layer-by-layer progression of LeNet-5 from the lecture slides:

  

1. **Input Layer:** Accepts a $32 \times 32$ image.
    
      
    - _Professor's Note on Slide 34 vs. Slide 35:_ You will notice a classic pedagogical discrepancy between Slide 34 and the parameter table on Slide 35 of your lecture deck! The architectural diagram on Slide 34 shows the canonical grayscale input of **$32 \times 32 \times 1$** (1 channel). However, the summary table on Slide 35 (adapted from Andrew Ng's coursework) calculates the activation size and CONV1 parameters assuming an RGB input of **$32 \times 32 \times 3$** ($32 \times 32 \times 3 = 3,072$ activations). Let us explicitly show the math for **both** so you are bulletproof on exams!
        
          
        
2. **CONV1 ($f=5, s=1, p=0$, $6$ filters):**
    
      
    - **Spatial Output:** $\lfloor \frac{32 + 0 - 5}{1} \rfloor + 1 = 28 \implies$ Output volume is **$28 \times 28 \times 6$** (Activation size $= 28 \times 28 \times 6 = 4,704$).
        
          
        
    - **Parameters (Assuming $32 \times 32 \times 3$ RGB input per Slide 35 Table):** Each of the $6$ filters has shape $5 \times 5 \times 3 = 75$ weights $+ 1$ bias $= 76$ parameters. Total $= 76 \times 6 =$ **$456$ parameters**. _(If using the $32 \times 32 \times 1$ grayscale input from Slide 34: $(5 \times 5 \times 1 + 1) \times 6 = 156$ parameters)_.
        
          
        
3. **POOL1 (Average Pooling, $f=2, s=2, p=0$):**
    
      
    - **Spatial Output:** $\lfloor \frac{28 + 0 - 2}{2} \rfloor + 1 = 14 \implies$ Output volume is **$14 \times 14 \times 6$** (Activation size $= 1,176$).
        
          
        
    - **Parameters:** **$0$** (Pooling layers never have learnable parameters!).
        
          
        
4. **CONV2 ($f=5, s=1, p=0$, $16$ filters):**
    
      
    - **Spatial Output:** $\lfloor \frac{14 + 0 - 5}{1} \rfloor + 1 = 10 \implies$ Output volume is **$10 \times 10 \times 16$** (Activation size $= 1,600$).
        
          
        
    - **Parameters:** Each of the $16$ filters has shape $5 \times 5 \times 6 = 150$ weights $+ 1$ bias $= 151$ parameters. Total $= 151 \times 16 =$ **$2,416$ parameters**.
        
          
        
5. **POOL2 (Average Pooling, $f=2, s=2, p=0$):**
    
      
    - **Spatial Output:** $\lfloor \frac{10 + 0 - 2}{2} \rfloor + 1 = 5 \implies$ Output volume is **$5 \times 5 \times 16$** (Activation size $= 400$).
        
          
        
    - **Parameters:** **$0$**.
        
          
        
6. **FC3 (Fully Connected Layer, $120$ neurons):**
    
      
    - The $5 \times 5 \times 16$ volume is flattened into a $400 \times 1$ vector and connected to $120$ neurons.
        
          
        
    - **Parameters:** $(400 \text{ inputs} \times 120 \text{ neurons}) + 120 \text{ biases} = 48,000 + 120 =$ **$48,120$ parameters**.
        
          
        
7. **FC4 (Fully Connected Layer, $84$ neurons):**
    
      
    - **Parameters:** $(120 \text{ inputs} \times 84 \text{ neurons}) + 84 \text{ biases} = 10,080 + 84 =$ **$10,164$ parameters**.
        
          
        
8. **Output Layer (Softmax Classifier, $10$ classes for digits $0\text{--}9$):**
    
      
    - **Parameters:** $(84 \text{ inputs} \times 10 \text{ classes}) + 10 \text{ biases} = 840 + 10 =$ **$850$ parameters**.
        
          
        

|**Layer Name**|**Hyperparameters**|**Activation Shape**|**Activation Size**|**Trainable Parameters (Slide 35 Math Breakdown)**|
|---|---|---|---|---|
|**Input**|—|$(32, 32, 3)$ _(or $1$)_|$3,072$ _(or $1,024$)_|$0$|
|**CONV1**|$f=5, s=1, p=0, n_C=6$|$(28, 28, 6)$|$4,704$|$(5 \times 5 \times 3 + 1) \times 6 = \mathbf{456}$|
|**POOL1**|Avg Pool, $f=2, s=2, p=0$|$(14, 14, 6)$|$1,176$|$\mathbf{0}$|
|**CONV2**|$f=5, s=1, p=0, n_C=16$|$(10, 10, 16)$|$1,600$|$(5 \times 5 \times 6 + 1) \times 16 = \mathbf{2,416}$|
|**POOL2**|Avg Pool, $f=2, s=2, p=0$|$(5, 5, 16)$|$400$|$\mathbf{0}$|
|**FC3**|$120$ Neurons|$(120, 1)$|$120$|$(400 \times 120) + 120 = \mathbf{48,120}$|
|**FC4**|$84$ Neurons|$(84, 1)$|$84$|$(120 \times 84) + 84 = \mathbf{10,164}$|
|**Softmax**|$10$ Output Classes ($\hat{y}$)|$(10, 1)$|$10$|$(84 \times 10) + 10 = \mathbf{850}$|
