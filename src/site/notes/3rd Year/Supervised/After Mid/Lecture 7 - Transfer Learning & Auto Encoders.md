---
{"dg-publish":true,"permalink":"/3rd-year/supervised/after-mid/lecture-7-transfer-learning-and-auto-encoders/","dg-note-properties":{}}
---

# This note covers the content of Week 7 slides, to see more on topics mentioned in class like YOLO and ResNet, [[3rd Year/Supervised/Expanded Explanations/Lecture 7\|here]]

At its core, **transfer learning** is a machine learning technique where a model developed for one task is reused as the starting point for a model on a second, related task.

Think of it like learning to play the piano. Once you understand reading sheet music, rhythm, and basic finger coordination, learning to play the organ or the harpsichord becomes significantly easier. You don't have to relearn what a musical note is; you just adapt your existing knowledge to the new instrument.

In deep learning, this means taking a model that someone else has already spent immense amounts of time, data, and computing power training (often on massive datasets like ImageNet, which contains millions of images), and tweaking it to solve your specific problem.

![](https://encrypted-tbn1.gstatic.com/licensed-image?q=tbn:ANd9GcRQEWjwPdrlHYD5uGqcdlmBM2x5bk0K9bk7CZLI_q8Q97D52RecvvRVzFSMNulJ2LRrI0B7foY1GxJVOzpILJDYcGTCTp8Mli-RflUY-nDfC1DqXTM)

Here is a breakdown of how it works and why it is so powerful, particularly with Convolutional Neural Networks (CNNs).

### How Transfer Learning Works in CNNs

To understand why transfer learning is so effective with CNNs, you have to look at how CNNs learn to "see." They process images hierarchically:

1. **Early Layers (The Foundation):** The first few layers of a CNN learn very generic, universal features. They detect simple edges, color blobs, curves, and textures. These features are useful for almost _any_ image task, whether you are looking at a dog, a car, or an X-ray.
    
2. **Middle Layers (The Shapes):** These layers combine the edges and textures to find shapes and patterns, like circles, squares, or specific structural arrangements.
    
3. **Late Layers (The Specifics):** The final convolutional layers learn highly complex, task-specific features. If the model was trained to recognize dogs, these layers are looking for snouts, floppy ears, or tails.
    
4. **The "Head" (The Classifier):** At the very end of the network, the features are flattened and passed into dense, fully connected layers that spit out the final prediction (e.g., "This is a Golden Retriever").
    

When we use transfer learning, we typically strip away that final "Head" (the classifier) because it is entirely specialized to the original task. We keep the convolutional layers (often called the **convolutional base**) because they hold all that valuable, foundational knowledge about how to extract features from an image.

### The Two Main Strategies

Once you have your pre-trained convolutional base, you add a brand new, untrained "Head" designed for your specific categories. From there, you generally choose between two training strategies:

#### 1. Feature Extraction

In this approach, you **freeze** the entire convolutional base. "Freezing" means you tell the network not to update the weights of these layers during training. You pass your new images through the frozen base to extract the features, and you _only_ train your newly added classifier Head to make sense of those features.

- **When to use it:** When your new dataset is very small, and the new images are visually similar to the original dataset the model was trained on.
    

#### 2. Fine-Tuning

Fine-tuning goes a step further. You still train your new classifier Head, but you also **unfreeze** some of the top layers of the convolutional base. You then train both the Head and those top convolutional layers together at a very low learning rate. This allows the model to slightly adjust its complex feature detectors to better suit your specific data.

- **When to use it:** When you have a relatively large dataset, or if your images are quite different from the original training data (e.g., applying a model trained on everyday objects to satellite imagery).
    

### Why is Transfer Learning a Game Changer?

- **Requires Drastically Less Data:** Training a CNN from scratch requires thousands or millions of images to teach it basic concepts like edges and shapes. With transfer learning, the model already knows how to see; it just needs to learn what _your_ specific objects look like. You can often get great results with just a few hundred images per class.
    <br>
- **Saves Massive Compute Time:** Training a deep network from scratch can take days or weeks on expensive GPUs. Transfer learning can often be completed in hours or even minutes.
    <br>
- **Better Performance:** Because the model starts with a robust, generalized understanding of visual features, it often achieves a higher level of accuracy and is less prone to overfitting than a model trained from scratch on a small dataset.

### Transfer Learning Using VGG16

**VGG16** is a highly influential Convolutional Neural Network (CNN) architecture proposed by the Visual Geometry Group (VGG) at the University of Oxford in 2014. It achieved top results in the ImageNet Large Scale Visual Recognition Challenge (ILSVRC), which requires models to classify images into 1,000 different categories.

Even though newer architectures like ResNet are more commonly used today for state-of-the-art tasks, VGG16 remains a fundamental benchmark and a highly popular choice for **transfer learning** due to its simple, uniform architecture and excellent ability to extract robust visual features.

### The Architecture of VGG16

The "16" in VGG16 refers to the fact that the network has **16 weight-bearing layers**: 13 convolutional layers and 3 fully connected (dense) layers.

Its most significant innovation was proving that **depth is critical for accuracy**, and that you can build deeper networks by using exclusively small convolutional filters.

Before VGG16, networks (like AlexNet) used large filters (e.g., $11\times11$ or $7\times7$) in their early layers to capture large features. VGG16 replaced these with a stack of very small **$3\times3$ filters** applied sequentially.

Why was this brilliant?

1. **Fewer Parameters:** Two sequential $3\times3$ filters cover the same effective area (receptive field) as one $5\times5$ filter, but use far fewer mathematical weights.
    
2. **More Non-Linearity:** Because there are more layers, the data passes through more activation functions (ReLUs). This allows the network to learn much more complex, highly discriminative features.
    

The network follows a strict, repeating pattern:

- Stacks of $3\times3$ convolutions (with a padding of 1 to keep spatial dimensions the same).
    
- A $2\times2$ Max Pooling layer with a stride of 2 (to halve the image dimensions).
    
- As the spatial dimensions halve, the number of feature channels doubles (from 64, to 128, to 256, to 512).
    

### Transfer Learning Code Example (TensorFlow / Keras)

Because VGG16 has already spent weeks training on over a million images to learn what edges, shapes, and textures look like, we can use it as a **feature extractor** for a brand new task.

In this example, we will load VGG16, throw away its original 1,000-class classifier (the "head"), freeze its convolutional base so we don't destroy its learned weights, and add a custom classifier for a new task (e.g., classifying images as either Cats or Dogs).



```Python
import tensorflow as tf
from tensorflow.keras.applications import VGG16
from tensorflow.keras import layers, models

# 1. Load the VGG16 base model
# 'weights="imagenet"' loads the pre-trained weights.
# 'include_top=False' removes the final fully connected layers (the original 1000-class head).
# 'input_shape' specifies the size of our new images.
base_model = VGG16(weights='imagenet', 
                   include_top=False, 
                   input_shape=(224, 224, 3))

# 2. Freeze the base model
# We do not want to update the weights of the VGG16 convolutional layers during initial training.
base_model.trainable = False

# 3. Create the new custom model on top
model = models.Sequential()

# Add the pre-trained VGG16 base
model.add(base_model)

# Add new custom layers for our specific task
# Flatten the 3D feature maps from VGG16 into a 1D vector
model.add(layers.Flatten()) 

# Add a fully connected layer to learn the relationships between the extracted features
model.add(layers.Dense(256, activation='relu'))

# Optional: Add dropout to prevent overfitting on our new dataset
model.add(layers.Dropout(0.5))

# Add the final classification layer
# Using 'sigmoid' for binary classification (e.g., Cat vs. Dog). 
# If you had 10 classes, it would be Dense(10, activation='softmax')
model.add(layers.Dense(1, activation='sigmoid'))

# 4. Compile the model
# We compile it using an optimizer, a loss function appropriate for binary classification, and an evaluation metric.
model.compile(optimizer=tf.keras.optimizers.Adam(learning_rate=0.0001),
              loss='binary_crossentropy',
              metrics=['accuracy'])

# View the structure of our new hybrid model
model.summary()

# 5. Train the model
# Now, when you call model.fit(train_data, ...), ONLY the weights in the 
# custom Dense layers at the bottom will be updated. VGG16 acts solely as a feature extractor.
```

### Why this works so well

When you pass a new image of a dog through this compiled model, the frozen VGG16 base acts like a sophisticated filter system. It automatically identifies the fur textures, the shape of the eyes, and the curve of the ears. It passes a condensed mathematical summary of those features into your new `Flatten` and `Dense` layers, which only have to do the easy job of deciding: _"Do these specific features mean 'Cat' or 'Dog'?"_

---

### 1. Image Segmentation

Image segmentation using neural networks has revolutionized computer vision because it automatically learns features from data, unlike traditional methods that require manual feature extraction. The lecture outlines three primary types of segmentation:

- **Semantic Segmentation:** This method assigns a specific class label to every pixel in an image. However, it does not differentiate between multiple objects of the same class; for example, it will label all cars in an image simply as "car". DeeplabV3 is noted as an example model.
    <br>
- **Instance Segmentation:** Building on semantic segmentation, this approach distinguishes between individual objects of the same class. It identifies multiple cars or people separately (e.g., Person 1, Person 2). Popular models for this include Mask R-CNN, YOLACT, and SOLO.
    <br>
- **Panoptic Segmentation:** This is a hybrid approach that combines semantic and instance segmentation. It assigns both class labels and object instance IDs, allowing a model to distinguish individual cars while simultaneously segmenting background elements like roads and trees.
    

### 2. Classification and Localization

While basic classification uses a CNN and a Softmax layer to determine what an object is (e.g., Person, Fruit, Car, or Background), localization involves drawing a bounding box around the detected object.

For localization and classification, the network generates an output vector, $Y$, which includes the following parameters:

- $P_c$: The probability that an object from one of the target classes actually exists in that space.
    <br>
- $B_x, B_y, B_w, B_h$: The coordinates for the center of the bounding box ($B_x, B_y$) and its width and height ($B_w, B_h$). These are based on a normalized image width and height ranging from (0,0) to (1,1).
    <br>
- $C_1, C_2, C_3$: The specific class labels of the object.
    

If the network determines there is no object ($P_c = 0$), the rest of the values in the vector are treated as "Don't Care" parameters.

**Loss Calculations**

To train the network, loss must be calculated. The slides highlight a squared error approach:

$$S = (Y_1 - \hat{Y}_1)^2 + (Y_2 - \hat{Y}_2)^2 + (Y_3 - \hat{Y}_3)^2 + ... + (Y_8 - \hat{Y}_8)^2$$

Other loss functions that can be utilized include:

- **Log Error Loss:** Used specifically for the Softmax output classes ($C_1, C_2, C_3$).
    <br>
- **Logistic Regression Loss:** Used to evaluate the probability output ($P_c$).
    

### 3. Segmentation Architectures

The lecture introduces encoder-decoder architectures used heavily in segmentation tasks:

- **Autoencoders:** These architectures process an input image through an "Encoder" to compress it into a Latent Space Representation (or "Code"), which is then reconstructed into an output image by a "Decoder".
    <br>
- **U-Net:** The slides visually detail a specific convolutional encoder-decoder called U-Net. It uses a series of max pooling ($2\times2$) and convolutions ($3\times3$, ReLU) to encode the image, followed by up-convolutions ($2\times2$) and "copy and crop" connections to generate a precise output segmentation map.
    

### 4. Landmark Detection

Landmark detection is the process of identifying highly specific coordinate points on a target.

- **Face Landmarks:** A model can be trained to detect 32 specific landmarks on a human face, outputting coordinates from $L_{1x}, L_{1y}$ to $L_{32x}, L_{32y}$.
    <br>
- **Pose Landmarks:** Similarly, 17 landmarks can be used to track human body pose.
    <br>
- **Applications:** This technology is used for emotion detection, pose detection, and Augmented Reality applications (such as dynamically adding graphical objects like hats to a person's face).
    

### 5. Sliding Window Technique

Finally, the lecture touches on the sliding window technique, which is an older method for object detection. It involves moving a designated "window" across an image in steps to check for the presence of an object. The precision and scale of this technique are managed by using different window sizes and selecting a specific stride (the distance the window moves at each step).

---

# Autoencoders

An **autoencoder** is a type of artificial neural network designed to learn highly efficient, compressed representations of data. Instead of retaining all the complex, raw details of an input (like the millions of individual pixels in an image), the network learns to describe that data using as few key mathematical features as possible.

### How Autoencoders Work

The architecture of an autoencoder consists of three main components that form an "hourglass" shape:

![Machine Learning Autoencoder Diagram Data Compression to Embedding Vector](https://encrypted-tbn1.gstatic.com/licensed-image?q=tbn:ANd9GcTyg3nz1XzkYJq3DXAM__k1ChwQ3k4r6jdobmscb0p5g4ZURN8ldAKKl45b8zD3qQYBLidcFIaUXbjxNG3BJp2AblBHOlLTjcpp5DTapHgTRQ91eTY)

Machine Learning Autoencoder Diagram Data Compression to Embedding Vector

1. **The Encoder:** This part of the network takes the high-dimensional input data (like an image, or complex tabular data) and compresses it step-by-step into a smaller and smaller set of numbers.
    <br>
2. **The Bottleneck (Latent Space):** This is the smallest layer in the middle of the network. It holds the "latent representation"—a low-dimensional, highly condensed summary of the original data's essential features. The number of neurons in this bottleneck determines the "latent dimension" (e.g., if there are 2 neurons, the data is compressed into a 2D space coordinate).
    <br>
3. **The Decoder:** This network works in reverse. It takes the compressed, low-dimensional coordinates from the bottleneck and attempts to rebuild the original high-dimensional input data from scratch.
    

**The Training Process:**

The network learns by minimizing the **reconstruction error**—the difference between the original input and the output the decoder generates. This is typically measured using Mean Squared Error (MSE), which compares the images pixel by pixel. As training progresses, the encoder gets better at identifying which features are critical to save, and the decoder gets better at translating those limited features back into a full image.

### Purpose and Applications

The primary purpose of an autoencoder is **data dimensionality reduction** and finding meaningful underlying patterns in raw data.

- **Latent Space Organization:** During successful training, the latent space naturally organizes itself. Similar items (like handwritten number 7s) group together to form clusters, while different items (like number 4s) form their own distinct clusters in a different area of the space.
    <br>
- **Simplifying Complex Classification:** By converting incredibly complex data into an organized lower-dimensional space, classification algorithms can work much more effectively. For example, it is practically impossible for humans to determine a patient's gender just by looking at a raw brain MRI. However, an autoencoder can compress these MRIs into a latent representation where male and female brain structures naturally separate into distinct clusters, allowing for accurate classification.
    

### Advantages

- **Unsupervised Feature Learning:** Autoencoders automatically learn what features are most important to compress data without needing humans to manually label or extract features.
    <br>
- **Architectural Foundation:** The encoder-decoder structure is the backbone for powerful modern architectures like U-Net (used heavily in medical image segmentation) and forms the foundational logic for cutting-edge generative models like DALL-E and Midjourney.
    

### Weaknesses and Limitations

While foundational, basic autoencoders have significant limitations, mostly related to how unstructured and messy their latent space can become:

- **Diffuse Clusters and Misclassification:** If the latent dimension is too small (e.g., 2D), there isn't enough room to separate the data. Clusters overlap heavily. A "4" might be encoded so close to a cluster of "9s" that the decoder gets confused and reconstructs it as a "9".
    <br>
- **Poor Interpolation:** In a well-behaved latent space, if you pick a coordinate exactly halfway between a "0" and a "6", you would expect the decoder to output a mixture of the two. Instead, a basic autoencoder will often decode that midpoint into pure nonsense, or an entirely unrelated number like a "5", because the empty space between clusters is not smoothly mapped out.
    <br>
- **Sensitivity to Noise:** Autoencoders tend to overfit to the exact training data. Adding even a tiny amount of random noise to an input image can completely break the encoder's logic, resulting in a wildly incorrect reconstruction.
    

To fix these issues, modern machine learning relies on regularized versions, most notably the **Variational Autoencoder (VAE)**, which forces the latent space to be continuous, smooth, and tightly organized.

---

While standard autoencoders are incredibly effective at compressing images into a tiny, generalized latent space (like identifying that a picture contains a "4"), that heavy compression creates a major problem for image segmentation.

In segmentation tasks (like semantic, instance, or panoptic segmentation), the goal is not just to identify _what_ is in the image, but exactly _where_ it is, down to the precise pixel boundary. Standard autoencoders lose all that spatial information when they compress the data.

To solve this, the autoencoder architecture is modified into what is known as a **Fully Convolutional Network (FCN)**, with the most famous architecture being the **U-Net** (which features an encoder-decoder structure).

Here is exactly how the architecture is repurposed to draw precise bounding masks around objects:

### 1. The Encoder (Learning the "What")

Instead of flattening the image into a 1D array of numbers like a standard autoencoder, a segmentation network keeps the data in 2D feature maps.

The encoder acts as a contracting path. It uses standard convolutional layers and max-pooling to process the image.

- As the image goes deeper into the encoder, its spatial dimensions (height and width) shrink drastically.
    <br>
- However, the number of feature channels increases.
    <br>
- By the time the image reaches the bottleneck, the network has a deep, rich mathematical understanding of _what_ objects are in the image, but it has almost entirely forgotten exactly _where_ they are located.
    

### 2. The Decoder (Learning the "Where")

To get back to the original image size to draw the pixel-by-pixel mask, the decoder acts as an expanding path.

Instead of pooling, it uses **up-convolutions** (or transposed convolutions) to artificially enlarge the spatial dimensions of the feature maps step-by-step.

However, if you only use an encoder and a decoder, the final output will look like a blurry, ill-defined blob. The decoder knows it needs to draw a car, but because the encoder threw away all the sharp boundary coordinates during compression, the decoder has to guess the exact edges.

### 3. The Secret Weapon: Skip Connections

To fix the blurry blob problem, architectures like U-Net introduce a brilliant mathematical shortcut: **Skip Connections** (often implemented as a "copy and crop" function).

During the forward pass, before the encoder compresses a feature map, it saves a copy of it. Then, when the decoder reaches that exact same spatial resolution on the way back up, the network takes that saved high-resolution map from the encoder and concatenates it directly onto the decoder's features.

This combines the best of both worlds:

- The **Decoder** provides the deep, semantic knowledge ("This blob is definitely a car").
    <br>
- The **Encoder's Skip Connection** provides the high-resolution, shallow spatial coordinates ("Here are the exact sharp edges of the objects from the original image").
    

By merging these, the network can output a mathematically precise, pixel-perfect segmentation map.

Sources: Gemini and [Autoencoders](https://youtu.be/hZ4a4NgM3u0?si=kPMzHJGBCdwktAtc)