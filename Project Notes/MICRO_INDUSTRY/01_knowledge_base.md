# 01 — Knowledge Base: Microplastic Detection System
### Indisposable | Gagangiri Foods Industry Project | Feb–May 2026

---

## Table of Contents

1. [Project Overview](#1-project-overview)
2. [Problem Statement](#2-problem-statement)
3. [Dataset](#3-dataset)
4. [Preprocessing Pipeline](#4-preprocessing-pipeline)
5. [Core Theory: Convolutional Neural Networks (CNN)](#5-core-theory-convolutional-neural-networks-cnn)
6. [Transfer Learning](#6-transfer-learning)
7. [MobileNetV2 Architecture](#7-mobilenetv2-architecture)
8. [Model Architecture (Custom Head)](#8-model-architecture-custom-head)
9. [Activation Functions — Theory & Choices](#9-activation-functions--theory--choices)
10. [Regularization: Dropout](#10-regularization-dropout)
11. [Training Process](#11-training-process)
12. [Evaluation Metrics — Deep Dive](#12-evaluation-metrics--deep-dive)
13. [Results, Justification & Optimization](#13-results-justification--optimization)
14. [Ablation Study: Activation Functions](#14-ablation-study-activation-functions)
15. [Model Saving & Deployment](#15-model-saving--deployment)
16. [Flask API](#16-flask-api)
17. [Digital Holographic Microscopy (DHM)](#17-digital-holographic-microscopy-dhm)
18. [End-to-End System Summary](#18-end-to-end-system-summary)

---

## 1. Project Overview

**Project Name:** Indisposable — Microplastic Detection and Avoidance  
**Industry Partner:** Gagangiri Foods  
**Duration:** 3 months (February 2, 2026 – May 6, 2026)  
**Team Size:** 5 students  
**Platform:** Google Colab + Flask API  
**Core Technology Stack:** Python, TensorFlow/Keras, MobileNetV2, scikit-learn, OpenCV, Flask

The project develops an automated system to detect microplastics in water samples using machine learning and computer vision. The system integrates with digital holographic microscopy (DHM) to capture 3D holographic images of water samples, which are then processed through a CNN-based image classification pipeline to determine the presence or absence of microplastic particles.

**Why it matters for Gagangiri Foods:** As a food manufacturer, water quality directly impacts product safety and purity. Microplastic contamination in water used during food processing can lead to product recalls, regulatory violations, and health risks to consumers. Manual microscopic inspection is slow, expensive, and error-prone. An automated ML-based system allows continuous, fast, and scalable water quality monitoring on the production line.

---

## 2. Problem Statement

Microplastics are plastic particles smaller than 5 millimeters (often microscopic, in the micrometer range). They enter water systems through industrial runoff, plastic degradation, synthetic textile washing, and other pollution vectors. Traditional detection methods include:

- **Fourier Transform Infrared Spectroscopy (FTIR):** Accurate but expensive and slow
- **Raman Spectroscopy:** Very precise but requires skilled operators
- **Manual Microscopy:** Time-consuming and subjective

**The gap:** None of these methods are fast enough for real-time quality control in a food production line.

**Our solution:** Train a binary image classifier on microscopic images of water samples to distinguish between:
- **Class 0 (Clean/Negative):** Water sample with no microplastics
- **Class 1 (Plastics/Positive):** Water sample containing microplastic particles

The classifier runs on images captured by a digital holographic microscope, enabling automated, real-time microplastic detection.

---

## 3. Dataset

**Primary Source:** [Microplastic Dataset for Computer Vision by Mohamadreza Momeni — Kaggle](https://www.kaggle.com/datasets/imtkaggleteam/microplastic-dataset-for-computer-vision)

**Secondary Source:** OECD Plastic Waste Statistics (for contextual data and project motivation)

### Dataset Statistics

| Split | Microplastic Images | Clean Images | Total |
|-------|-------------------|-------------|-------|
| Training | ~625 | ~625 | ~1,250 |
| Validation | ~112 | ~112 | ~224 |
| Testing | ~100 | ~100 | ~200 |
| **Total** | **~781** | **~781** | **~1,562** |

The dataset is **perfectly balanced** — equal number of positive and negative samples. This is intentional and important: a balanced dataset means that a naive model predicting all zeros would only achieve 50% accuracy, so any accuracy above 50% represents genuine learning.

### Dataset Structure on Google Drive

```
P4_MicroPystics/Resources/
├── 1_Training/
│   ├── microplastic/   (625 images)
│   └── clean/          (625 images)
├── 2_Validation/
│   ├── microplastic/   (112 images)
│   └── clean/          (112 images)
└── 3_Testing/
    ├── microplastic/   (~100 images)
    └── clean/          (~100 images)
```

The directory structure follows Keras's `flow_from_directory` convention where each subdirectory name becomes a class label. Keras automatically assigns numeric labels based on alphabetical ordering of folder names (`clean = 0`, `microplastic = 1`).

### Why This Dataset Size is Challenging

1,562 images is a **small dataset** for deep learning. Standard large-scale image datasets (ImageNet, COCO) contain millions of images. With only ~1,500 images:
- The model cannot learn enough diversity to generalize perfectly
- Risk of overfitting is high (model memorizes training data rather than learning patterns)
- Transfer learning becomes essential — it is impractical to train from scratch

---

## 4. Preprocessing Pipeline

Preprocessing transforms raw images into a format suitable for the neural network. Every design decision has a reason.

### 4.1 Mounting Google Drive

```python
from google.colab import drive
drive.mount('/content/drive')
```

Google Colab runs on cloud VMs with no persistent storage. Mounting Google Drive provides access to the dataset and allows saving the trained model.

### 4.2 Image Resizing to 128×128

```python
def resize_images(image):
    resized_image = resize(image.squeeze(), (128, 128))
    return np.expand_dims(resized_image, axis=-1)
```

**Why 128×128?**
- MobileNetV2 requires a minimum input of 96×96 and works well at 128×128
- Larger sizes (224×224 or 256×256) would increase computation time and memory significantly
- 128×128 preserves enough spatial detail to identify microplastic particles in microscopic images
- The `squeeze()` removes any singleton dimensions; `expand_dims` adds the channel dimension back

### 4.3 Grayscale Conversion

```python
def rgb_to_gray(image):
    return rgb2gray(image)
```

**Why grayscale?**
- Microscopic images of water samples captured in grayscale/monochrome (as is common with DHM)
- Reduces memory footprint by 3× (1 channel vs 3 channels)
- Microplastics in microscopy are identified by their structural texture and shape, not color — color information is irrelevant and could add noise
- MobileNetV2 still needs 3 channels — grayscale values are replicated across all 3 channels during the `ImageDataGenerator` pipeline

### 4.4 ImageDataGenerator Configuration

```python
datagen = ImageDataGenerator(
    rescale=1./255,
    validation_split=0.2,
    preprocessing_function=lambda x: resize(x.squeeze(), (128, 128)),
    featurewise_center=True,
    featurewise_std_normalization=True,
    rotation_range=180,
    width_shift_range=0.2,
    height_shift_range=0.2,
    horizontal_flip=True,
    fill_mode='nearest',
)
```

Let's break down each argument:

#### `rescale=1./255`
Normalizes pixel values from the range [0, 255] to [0, 1]. Neural networks converge faster and more stably when input values are small and bounded. Raw pixel values in the range [0, 255] would cause large gradient magnitudes.

#### `featurewise_center=True` and `featurewise_std_normalization=True`
- **featurewise_center:** Subtracts the mean of the entire dataset from each image (zero-centers the data)
- **featurewise_std_normalization:** Divides by the standard deviation (standardizes to unit variance)
- Together these two achieve **Z-score normalization:** `x_normalized = (x - mean) / std`
- This matches the normalization used during ImageNet training of MobileNetV2, which helps transfer learning work correctly

#### `rotation_range=180`
Randomly rotates images up to ±180 degrees during training. Microplastic particles under a microscope can appear in any orientation — a fragment tilted at 45° and one at 135° are the same particle. 180° rotation teaches the model rotational invariance.

#### `width_shift_range=0.2` and `height_shift_range=0.2`
Randomly shifts images horizontally and vertically by up to 20% of the image dimension. This teaches the model that the position of a microplastic particle within the frame should not affect the classification.

#### `horizontal_flip=True`
Randomly flips images left-right. Again, a particle flipped horizontally is still the same type of particle. This doubles the effective training data for free.

#### `fill_mode='nearest'`
When a rotation or shift creates empty pixels at the edges of the image, fill them using the nearest existing pixel value. This avoids introducing artifacts (black borders, etc.) that could confuse the model.

### 4.5 Random Seeds for Reproducibility

```python
np.random.seed(14)
tf.random.set_seed(14)
```

Setting seeds ensures that the same random numbers are generated each time — shuffling of data, weight initialization, dropout masks — making experiments reproducible. Seed value 14 is arbitrary.

### 4.6 Data Generators

```python
train_generator = datagen.flow_from_directory(
    training_path,
    target_size=(128, 128),
    batch_size=1154,
    class_mode='binary',
)

validation_generator = datagen.flow_from_directory(
    validation_path,
    target_size=(128, 128),
    batch_size=208,
    class_mode='binary',
)
```

`flow_from_directory` reads images from a folder structure and automatically assigns class labels based on subdirectory names. `class_mode='binary'` tells the generator to produce single binary labels (0 or 1), which is appropriate for two-class problems.

**Batch sizes** equal the dataset sizes (1,154 ≈ entire training set; 208 ≈ entire validation set), meaning the model sees all data in one batch per step. This is effectively batch gradient descent rather than mini-batch, which can be computationally expensive but gives stable gradient estimates.

---

## 5. Core Theory: Convolutional Neural Networks (CNN)

### 5.1 What is a CNN?

A Convolutional Neural Network is a deep learning architecture specifically designed for processing structured grid data like images. Unlike fully connected (dense) neural networks, CNNs exploit the spatial locality of features in images — nearby pixels are more related to each other than distant ones.

### 5.2 Key Components

#### Convolutional Layer
The core operation is the **convolution**: a small matrix called a **kernel** or **filter** slides across the input image, computing a dot product at each position. The output is a **feature map**.

```
Input:   5×5 image
Kernel:  3×3 filter
Output:  3×3 feature map (with valid padding)
```

Each kernel learns to detect a specific pattern — edges, curves, textures, etc. A convolutional layer uses multiple kernels to extract multiple features simultaneously.

**Key properties:**
- **Weight sharing:** The same kernel is used across the entire image, massively reducing parameters compared to fully connected layers
- **Translation invariance:** A pattern detected in one region of the image can be detected anywhere
- **Hierarchical features:** Early layers detect edges → middle layers detect shapes → deep layers detect semantic features (e.g., "microplastic fragment")

#### Pooling Layer
Reduces the spatial dimensions of feature maps while retaining important information. **Max pooling** takes the maximum value in a region (retains the strongest activation). **Average pooling** takes the mean.

Benefits:
- Reduces computation in subsequent layers
- Provides a degree of spatial invariance
- Reduces risk of overfitting by abstracting features

#### Batch Normalization
Normalizes the activations within each mini-batch. This stabilizes training, allows higher learning rates, and acts as a mild regularizer.

#### Activation Functions
Applied element-wise after each layer to introduce **non-linearity**. Without activation functions, the entire network would be equivalent to a single linear transformation, incapable of learning complex patterns.

### 5.3 How CNNs Learn

CNNs learn through **backpropagation** with **gradient descent**:

1. **Forward pass:** Input image → through all layers → output prediction
2. **Loss computation:** Compare prediction to true label using a loss function
3. **Backward pass:** Compute gradient of loss with respect to every parameter using the chain rule
4. **Weight update:** Adjust parameters in the direction that reduces loss

This cycle repeats for every batch, over many epochs.

---

## 6. Transfer Learning

### 6.1 The Concept

Transfer learning is using a model trained on one task as the starting point for a different but related task. The intuition: the low-level features learned by a CNN (edges, textures, gradients) are universal — they're useful for recognizing almost any visual pattern, not just the original training task.

**Analogy:** A person who has learned to play piano can learn guitar faster than someone with no musical background — the knowledge of music theory, rhythm, and finger dexterity transfers.

### 6.2 Why Transfer Learning for This Project?

- **Small dataset:** ~1,500 images is insufficient to train a deep CNN from scratch. Deep models have millions of parameters; without enough data they overfit (memorize training data, fail on new data)
- **MobileNetV2 was trained on ImageNet:** 14 million images, 1,000 categories. It has learned extremely rich visual representations
- **Time and compute:** Training MobileNetV2 from scratch takes days on multiple GPUs. Transfer learning lets us fine-tune in hours on Google Colab

### 6.3 How We Applied It

**Strategy: Feature Extraction (frozen base)**

```python
base_model = MobileNetV2(weights='imagenet', include_top=False, input_shape=(128, 128, 3))
base_model.trainable = False  # FREEZE — don't update these weights during training
```

- `weights='imagenet'`: Load pretrained weights
- `include_top=False`: Exclude the final classification head (designed for 1,000 ImageNet classes)
- `base_model.trainable = False`: Freeze all convolutional weights — only our custom head will be trained

By freezing the base, we prevent the pretrained feature extractor from being corrupted by our small dataset. We only train the newly added classification layers, which have far fewer parameters and can be fitted with 1,500 images.

**Alternative strategy (not used here):** **Fine-tuning** — unfreeze some later layers of the base model and train them with a very low learning rate to adapt them to the specific domain. This can improve performance but requires more data and careful hyperparameter tuning to avoid destroying the pretrained weights.

---

## 7. MobileNetV2 Architecture

### 7.1 Overview

MobileNetV2 is a lightweight CNN architecture designed by Google for efficient inference on mobile and embedded devices. Published in 2018, it achieves near state-of-the-art accuracy on ImageNet with a fraction of the parameters of larger models (VGG, ResNet, Inception).

**Key stats:**
- Parameters: ~3.4 million (vs ResNet50's ~25 million)
- Input: 128×128×3 (our configuration; standard is 224×224)
- Top-1 ImageNet accuracy: ~71.8%

### 7.2 Core Innovation: Inverted Residuals with Linear Bottlenecks

MobileNetV2 is built on two key ideas:

#### Depthwise Separable Convolutions (from MobileNetV1)
A standard convolution applies an N×N×C_in filter to produce C_out feature maps — computationally expensive. Depthwise separable convolution splits this into:
1. **Depthwise convolution:** Apply one N×N filter per input channel independently
2. **Pointwise convolution:** Apply 1×1 convolutions to combine across channels

This reduces computation by approximately 8-9× with minimal accuracy loss.

#### Inverted Residual Blocks (MobileNetV2 innovation)
Standard residual blocks (ResNet): wide → narrow → wide (bottleneck compresses first)  
MobileNetV2 inverted residual: narrow → wide → narrow

Process:
1. **Expand:** 1×1 convolution expands channels by factor t (expansion ratio, usually 6)
2. **Depthwise conv:** 3×3 depthwise convolution in the expanded space
3. **Project:** 1×1 convolution projects back to narrow channels (**linear bottleneck — no activation**)
4. **Skip connection:** Add input to output (only when stride=1 and dimensions match)

The linear activation at the bottleneck (rather than ReLU) prevents information destruction — ReLU collapses the manifold in low-dimensional spaces.

### 7.3 GlobalAveragePooling2D

After the MobileNetV2 base, we apply:
```python
layers.GlobalAveragePooling2D()
```

This takes the final feature maps (shape: e.g., 4×4×1280 for 128×128 input) and averages across spatial dimensions, producing a 1D vector of 1280 values. This:
- Eliminates the need for flattening
- Dramatically reduces parameters in the next dense layer
- Acts as a structural regularizer
- Makes the model spatially invariant (position of the feature in the image doesn't matter)

---

## 8. Model Architecture (Custom Head)

```python
model = Sequential([
    base_model,                              # MobileNetV2 (frozen)
    layers.GlobalAveragePooling2D(),         # 4×4×1280 → 1280
    layers.Dense(256, activation='relu'),    # 1280 → 256
    layers.Dropout(0.4),                     # 40% dropout
    layers.Dense(128, activation='elu'),     # 256 → 128
    layers.Dropout(0.3),                     # 30% dropout
    layers.Dense(64, activation='leaky_relu'), # 128 → 64
    layers.Dropout(0.2),                     # 20% dropout
    layers.Dense(1, activation='sigmoid')    # 64 → 1 (binary output)
])
```

### Layer-by-Layer Explanation

| Layer | Input Dim | Output Dim | Parameters | Purpose |
|-------|-----------|-----------|------------|---------|
| MobileNetV2 (frozen) | 128×128×3 | 4×4×1280 | ~3.4M (frozen) | Feature extraction |
| GlobalAveragePooling2D | 4×4×1280 | 1280 | 0 | Spatial aggregation |
| Dense(256, relu) | 1280 | 256 | 328,192 | High-level combination |
| Dropout(0.4) | 256 | 256 | 0 | Regularization |
| Dense(128, elu) | 256 | 128 | 32,896 | Mid-level learning |
| Dropout(0.3) | 128 | 128 | 0 | Regularization |
| Dense(64, leaky_relu) | 128 | 64 | 8,256 | Refinement |
| Dropout(0.2) | 64 | 64 | 0 | Regularization |
| Dense(1, sigmoid) | 64 | 1 | 65 | Binary classification |

**Trainable parameters:** ~369,409  
**Frozen parameters (MobileNetV2):** ~3,400,000  
**Total parameters:** ~3,769,409

### Why 4 Dense Layers?

The classification head transitions from the rich 1280-dimensional representation of MobileNetV2 down to a single binary output. A direct connection (1280→1) would lose too much information. The gradual reduction (1280 → 256 → 128 → 64 → 1) allows the network to learn progressively more abstract and task-specific combinations of features.

### Decreasing Dropout Rates (0.4 → 0.3 → 0.2)

Dropout rates are higher in earlier (wider) layers and lower in later (narrower) layers. This is intentional:
- Wider layers (256 nodes) have more redundant capacity and benefit from stronger regularization
- Narrower layers (64 nodes) have fewer neurons — too much dropout at this stage would prevent learning entirely
- The gradual decrease creates a "funnel" of increasing certainty as information progresses through the network

---

## 9. Activation Functions — Theory & Choices

### 9.1 Why Activation Functions are Necessary

Without non-linear activation functions, a deep neural network collapses into a single linear transformation regardless of depth: `f(g(x)) = Ax + b` (linear composition of linear functions is linear). Activation functions break this linearity, enabling the network to approximate any function (Universal Approximation Theorem).

### 9.2 ReLU (Rectified Linear Unit) — Layer 1

**Formula:** `f(x) = max(0, x)`

```
Output
 |    /
 |   /
 |  /
 | /
-+-------  Input
```

**Properties:**
- Simple and computationally cheap
- Does not saturate for positive values — gradients flow freely
- **Dead ReLU problem:** For x < 0, output = 0 and gradient = 0. Neurons can get "stuck" and stop learning permanently if they receive only negative inputs
- Sparse activation: ~50% of neurons typically output 0, which can be efficient

**Why Layer 1 (Dense 256)?** The first layer after pooling deals with high-dimensional, general features. ReLU's strong sparsity-inducing property helps select the most relevant features from 1280 dimensions.

### 9.3 ELU (Exponential Linear Unit) — Layer 2

**Formula:** `f(x) = x if x ≥ 0 else α(e^x - 1)`  where α ≈ 1.0

```
Output
 |    /
 |   /
 | _/   (smooth curve below 0)
-/------  Input
```

**Properties:**
- Like ReLU for positive values
- Smooth, non-zero output for negative values → **no dead neuron problem**
- Negative values allow the mean activation to be closer to zero → faster convergence
- More expensive than ReLU (exponential computation) but more biologically plausible

**Why Layer 2?** Middle layers benefit from ELU's smoother gradient profile, which helps stable training after the initial feature selection by ReLU.

### 9.4 Leaky ReLU — Layer 3

**Formula:** `f(x) = x if x ≥ 0 else αx`  where α = 0.01 (default)

```
Output
 |    /
 |   /
  \ /   (small negative slope)
---+---  Input
```

**Properties:**
- Fixes the dead neuron problem of ReLU: negative inputs produce small but non-zero gradients (slope = 0.01)
- Computationally simpler than ELU (no exponential)
- Less aggressive sparsification than ReLU

**Why Layer 3?** As the network narrows (128→64), we want to preserve gradient flow carefully. Leaky ReLU ensures no neurons die out during this compression step.

### 9.5 Sigmoid — Output Layer

**Formula:** `f(x) = 1 / (1 + e^{-x})`

**Output range:** [0, 1], interpreted as probability

**Properties:**
- Maps any real value to (0, 1)
- Output directly interpretable as P(microplastic present)
- If sigmoid output > 0.5 → predicted Class 1 (microplastic)
- If sigmoid output ≤ 0.5 → predicted Class 0 (clean)

**Why sigmoid for the output?** This is a **binary classification problem** (2 classes). Sigmoid produces a single probability value. The alternative would be softmax with 2 output nodes, but sigmoid + 1 node is mathematically equivalent and more efficient.

---

## 10. Regularization: Dropout

### 10.1 What is Dropout?

During each training step, dropout randomly sets a fraction `p` of neurons to zero. Those neurons do not contribute to the forward pass and receive no gradient update.

```python
layers.Dropout(0.4)  # 40% of neurons are randomly zeroed
```

### 10.2 Why Dropout Prevents Overfitting

**Overfitting** occurs when a model learns specific patterns in the training data that don't generalize — it memorizes rather than learns.

Dropout prevents this by:
1. **Ensemble effect:** Each training step uses a different random sub-network. The final trained model is equivalent to an ensemble of 2^N models (where N = number of neurons), all sharing weights
2. **Preventing co-adaptation:** Neurons cannot rely on specific other neurons always being present, so they must learn more robust, independent features
3. **Noise injection:** Dropout introduces stochastic noise, which acts as a regularizer similar to L2 weight decay

### 10.3 Dropout at Test Time

During inference (prediction), **dropout is disabled**. All neurons are active, but their outputs are scaled by `(1 - p)` to compensate for the fact that more neurons are active than during training. Keras handles this automatically.

---

## 11. Training Process

### 11.1 Loss Function: Binary Cross-Entropy

```python
model.compile(optimizer=Adam(), loss='binary_crossentropy', metrics=['accuracy'])
```

Binary cross-entropy is the standard loss function for binary classification:

**Formula:** `L = -[y·log(ŷ) + (1-y)·log(1-ŷ)]`

Where:
- `y` = true label (0 or 1)
- `ŷ` = model's predicted probability

**Interpretation:**
- If y=1 and ŷ=0.9: L = -log(0.9) ≈ 0.105 (small loss, correct)
- If y=1 and ŷ=0.1: L = -log(0.1) ≈ 2.303 (large loss, wrong)
- The log function heavily penalizes confident wrong predictions

**Why binary cross-entropy over MSE?** Mean Squared Error (MSE) for classification produces small gradients near 0 and 1, causing slow learning. Binary cross-entropy produces larger gradients when the model is confidently wrong, leading to faster correction.

### 11.2 Optimizer: Adam

Adam (Adaptive Moment Estimation) combines two improvements over basic gradient descent:
1. **Momentum (first moment):** Accumulates past gradients to smooth out updates and overcome local minima
2. **RMSProp (second moment):** Adapts the learning rate per-parameter based on gradient history — large gradients get smaller learning rates

**Adam update rule:**
```
m_t = β₁·m_{t-1} + (1-β₁)·g_t        # first moment (momentum)
v_t = β₂·v_{t-1} + (1-β₂)·g_t²       # second moment (gradient variance)
θ_t = θ_{t-1} - α·m̂_t / (√v̂_t + ε)   # parameter update
```

Default parameters: α=0.001, β₁=0.9, β₂=0.999, ε=1e-8

**Why Adam?** Adam converges faster than SGD for most problems, requires less hyperparameter tuning, and handles sparse gradients well. It's the standard first choice optimizer for most deep learning tasks.

### 11.3 Epochs: 50

```python
epochs = 50
history = model.fit(train_generator, epochs=epochs, validation_data=validation_generator)
```

50 epochs means the model sees the entire training set 50 times. With only ~1,500 images, 50 epochs balances:
- Enough repetition for the model to converge
- Not so many epochs that the model overfits the training data

The `history` object stores training and validation accuracy/loss at each epoch, allowing us to plot learning curves and identify overfitting.

---

## 12. Evaluation Metrics — Deep Dive

This section provides a comprehensive explanation of all evaluation metrics used, their values, interpretation, and significance.

### 12.1 The Confusion Matrix

The confusion matrix is the foundation of all classification metrics. It shows the distribution of predictions:

```
                  Predicted
                  Clean (0)   Plastics (1)
Actual  Clean (0)    TN           FP
        Plastics(1)  FN           TP
```

| Term | Meaning | Implication for this project |
|------|---------|------------------------------|
| **True Positive (TP)** | Model says plastics, it IS plastics | Correctly detected contamination |
| **True Negative (TN)** | Model says clean, it IS clean | Correctly cleared water sample |
| **False Positive (FP)** | Model says plastics, it's actually clean | Unnecessary investigation/rejection of clean water |
| **False Negative (FN)** | Model says clean, but there ARE plastics | **DANGEROUS** — contaminated water passes through |

**Critical insight for this project:** From a food safety perspective, **False Negatives are far more dangerous than False Positives.** A False Negative means contaminated water is used in food production. A False Positive just means clean water is re-tested or rejected unnecessarily (cost, not safety risk).

The README notes the model is "prone to false positives" — this means it's biased toward labeling things as microplastic-present. In food safety terms, this is the **safer type of error** (better to over-flag than to miss contamination).

### 12.2 Accuracy

**Formula:** `Accuracy = (TP + TN) / (TP + TN + FP + FN)`

**Our Value: 70.9%**

This means the model correctly classifies approximately 7 out of every 10 water sample images.

**Interpretation:**
- **Baseline (random guessing on balanced dataset):** 50%
- **Our model:** 70.9% → 20.9% improvement over random chance, demonstrating genuine learning
- For a binary medical/safety classification problem on a small dataset, 70.9% is modest but not negligible

**Why not higher?**
1. **Dataset size:** ~1,500 images is small for a CNN-based model
2. **Visual similarity:** Microscopic debris, dust, air bubbles in clean water can look superficially similar to microplastic particles
3. **Grayscale limitation:** Color information (which could help distinguish plastic from organic debris) is discarded
4. **Domain gap:** Transfer learning from natural images (ImageNet) to microscopy images involves a significant domain shift

### 12.3 Loss Value: 59% (0.59)

**Formula (Binary Cross-Entropy):** `L = -[y·log(ŷ) + (1-y)·log(1-ŷ)]`

**Our Value: ~0.59**

**What does 0.59 loss mean?**
- Perfect model loss → 0
- Random guessing loss on balanced dataset → `-[0.5·log(0.5) + 0.5·log(0.5)] = log(2) ≈ 0.693`
- Our loss 0.59 < 0.693 → the model IS performing better than random chance
- However, 0.59 is not close to 0 → there is significant uncertainty in predictions

**Why is loss high relative to accuracy?**
Binary cross-entropy is sensitive to **prediction confidence**. Even when the model predicts the correct class, if it does so with only moderate confidence (e.g., 0.6 probability rather than 0.95), the loss is still substantial. 

Example:
- True label: 1 (microplastic)
- Model prediction: 0.62 → Correct class (>0.5), contributes to accuracy
- Loss: -log(0.62) ≈ 0.478 → Still significant loss

So the model is classifying correctly but with moderate confidence. To reduce loss while maintaining accuracy, the model would need to be more decisive in its predictions — which typically requires more data.

### 12.4 Precision

**Formula:** `Precision = TP / (TP + FP)`

Precision measures: "Of all the times the model predicted microplastic, what fraction were actually microplastic?"

**Interpretation:**
- High precision → few false alarms
- Low precision → many false positives
- Given the model is "prone to false positives," precision is likely lower than recall

### 12.5 Recall (Sensitivity / True Positive Rate)

**Formula:** `Recall = TP / (TP + FN)`

Recall measures: "Of all the actual microplastic samples, what fraction did the model detect?"

**Interpretation:**
- High recall → few missed detections
- For food safety, recall is the **more critical metric** — we want to miss as few contaminated samples as possible
- A model with high recall but low precision is over-triggering (many false alarms) — acceptable in this context

### 12.6 F1 Score

**Formula:** `F1 = 2 × (Precision × Recall) / (Precision + Recall)`

F1 is the harmonic mean of precision and recall. It provides a single metric that balances both concerns. The harmonic mean penalizes extreme values — a model with 0.1 precision and 1.0 recall gets F1 = 0.18, not 0.55.

### 12.7 Training vs. Validation vs. Testing Performance

Three separate evaluations are conducted:

| Metric | Training | Validation | Testing |
|--------|---------|------------|---------|
| Purpose | See if model learned | Tune hyperparameters | Final unbiased evaluation |
| Data | Training set | Validation set | Testing set (never seen) |

**Overfitting detection:** If training accuracy >> validation accuracy, the model is overfitting. If both are close (within 5-10%), the model is generalizing. The accuracy/loss plots over epochs reveal this visually:
- Overfitting: Training curve improves, validation curve plateaus or worsens
- Good fit: Both curves improve together and plateau at similar values

### 12.8 Classification Report

The `classification_report` from sklearn provides precision, recall, F1, and support for each class separately. This is important because a model might perform well on one class and poorly on the other, which aggregate accuracy would hide.

```python
from sklearn.metrics import confusion_matrix, classification_report
print(classification_report(true_labels, binary_predictions))
```

Output format:
```
              precision    recall  f1-score   support
           0       x.xx      x.xx      x.xx       100
           1       x.xx      x.xx      x.xx       100
    accuracy                           x.xx       200
   macro avg       x.xx      x.xx      x.xx       200
weighted avg       x.xx      x.xx      x.xx       200
```

---

## 13. Results, Justification & Optimization

### 13.1 Final Results

| Metric | Value | Interpretation |
|--------|-------|----------------|
| Test Accuracy | **70.9%** | Correct classification 7/10 times |
| Test Loss | **~0.59** | Moderate prediction uncertainty |
| False Positive tendency | Observed | Model over-flags (safer error type) |

### 13.2 Why These Results Are Reasonable

1. **Small dataset effect:** Deep learning typically requires thousands to millions of labeled examples per class. With ~780 images per class, 70.9% is a reasonable baseline.

2. **Domain gap:** MobileNetV2 was trained on natural images (dogs, cars, furniture). Microscopic images of water samples are structurally very different — low contrast, grayscale, fine-grained texture differences. The feature extractor must adapt more than usual.

3. **Balanced dataset:** On a balanced dataset, 70.9% accuracy is genuinely significant. A random classifier would achieve exactly 50%.

4. **False positive pattern:** The model is more sensitive than specific. It tends to label ambiguous images as positive (microplastic). In food safety, this is a conservative and appropriate bias — false alarms are acceptable, missed detections are not.

### 13.3 Optimization Strategies Applied

**Dropout regularization:** Applied at rates 0.4, 0.3, 0.2 across dense layers to prevent overfitting. Without dropout, early experiments likely showed training accuracy much higher than validation accuracy.

**Data augmentation:** Rotation, flipping, shifting synthetically expands the dataset. With 781 training images and augmentation, the model encounters different versions of the same image each epoch, improving generalization.

**Transfer learning:** The most impactful optimization. Using MobileNetV2's pretrained weights gives the model a powerful feature extractor for free, compensating for the small dataset.

**Multiple activation functions:** Rather than using ReLU everywhere, the team experimented with different activations (elu, leaky_relu) to find the best combination — validated by the ablation study.

### 13.4 Further Optimization (What Could Improve Results)

1. **More data (most impactful):** Collecting 5,000–10,000 images per class could push accuracy above 85%
2. **Fine-tuning:** Unfreeze the last few layers of MobileNetV2 with a very low learning rate (1e-5) to adapt base features to microscopy images
3. **Class-weighted loss:** Although the dataset is balanced, applying class weights could prioritize recall over precision (reduce false negatives)
4. **Larger input resolution:** Using 224×224 input would give MobileNetV2 more spatial detail to work with
5. **Ensemble methods:** Train multiple models and average predictions to reduce variance
6. **Different base models:** EfficientNet or ResNet may handle microscopy images differently than MobileNetV2
7. **Color images:** If DHM can produce color output, retaining color channels may help distinguish plastics from organic debris
8. **Learning rate scheduling:** ReduceLROnPlateau or cosine annealing to adaptively reduce learning rate when validation loss plateaus

---

## 14. Ablation Study: Activation Functions

An ablation study removes or changes one component at a time to understand its contribution. The team conducted an activation function ablation study:

```python
activation_functions = ['leaky_relu', 'elu', 'tanh', 'selu', 'sigmoid', 'relu']
results = {}

for activation in activation_functions:
    model = Sequential([
        base_model,
        GlobalAveragePooling2D(),
        Dense(128, activation='relu'),
        Dropout(0.5),
        Dense(64, activation=activation),   # ← varied
        Dropout(0.3),
        Dense(1, activation='sigmoid')
    ])
    model.compile(optimizer=Adam(), loss='binary_crossentropy', metrics=['accuracy'])
    history = model.fit(train_generator, epochs=15, validation_data=validation_generator)
    results[activation] = history.history['val_accuracy']
```

**What was tested:** Each of 6 activation functions in the middle Dense(64) layer, for 15 epochs each, with validation accuracy as the selection criterion.

**Methodology:** Keep all other hyperparameters identical; vary only the activation function. This isolates the effect of the activation function choice.

**Outcome:** The combination ReLU → ELU → Leaky ReLU was selected for the final model based on validation accuracy performance across these experiments.

**Why this matters:** Most tutorials blindly use ReLU everywhere. The team went a step further and empirically validated the activation function choice — this is proper ML engineering practice.

---

## 15. Model Saving & Deployment

### 15.1 H5 Format (Weights Only)

```python
model.save_weights('MicroPystics.h5')
```

Saves only the model weights (not the architecture). To use this:
```python
model = build_model()          # recreate architecture
model.load_weights('MicroPystics.h5')  # load weights
```

Good for: Sharing weights separately from code; slightly smaller file size.

### 15.2 TensorFlow SavedModel Format

```python
model.save("./drive/MyDrive/P4_MicroPystics/MicroPystics", save_format="tf")
```

Saves the complete model: architecture + weights + optimizer state + training configuration. This is the recommended format for deployment:
- Can be loaded without any knowledge of the original code
- Supports TensorFlow Serving for production deployment
- Compatible with TensorFlow Lite for mobile deployment
- Note: `save_format="tf"` was used for Mac compatibility (H5 format sometimes has path issues on macOS)

---

## 16. Flask API

The Flask API serves the trained model to enable predictions on new images without running the full training pipeline.

### 16.1 Architecture

```
User/Dashboard → HTTP Request (image) → Flask API → Load Model → Preprocess Image → Predict → Return JSON Response
```

### 16.2 Key Dependencies

| Library | Purpose |
|---------|---------|
| Flask | Web framework; handles HTTP requests |
| TensorFlow | Load and run the trained model |
| Pillow (PIL) | Image loading and resizing |
| NumPy | Array operations for preprocessing |
| os | File path management |

### 16.3 Prediction Endpoint Logic

```python
# Pseudocode for the prediction endpoint
@app.route('/predict', methods=['POST'])
def predict():
    image = load_image_from_request()
    image = preprocess(image)          # resize to 128×128, normalize
    prediction = model.predict(image)  # sigmoid output 0-1
    result = "Microplastic" if prediction > 0.5 else "Clean"
    confidence = float(prediction)
    return jsonify({"result": result, "confidence": confidence})
```

### 16.4 Integration with Dashboard

The Flask API acts as a bridge between:
- The **DHM system** (which captures images) → sends images to the API
- The **Dashboard** (which displays results) → receives JSON responses and visualizes

---

## 17. Digital Holographic Microscopy (DHM)

### 17.1 What is DHM?

Digital Holographic Microscopy is an advanced imaging technique that captures 3D information about microscopic samples by recording the interference pattern (hologram) between a reference laser beam and a beam that has passed through the sample.

Unlike conventional microscopy (which produces a 2D image), DHM:
- Captures the full **wavefront** (amplitude + phase) of light
- Enables 3D reconstruction of the sample
- Can image transparent particles (like many plastics) that are invisible under normal light microscopy
- Is label-free — no staining or chemical preparation needed

### 17.2 Why DHM for Microplastic Detection?

- **Transparency problem:** Many microplastics are transparent or translucent — conventional microscopy provides poor contrast. DHM's phase imaging makes them visible
- **3D information:** Size, shape, and depth information of particles
- **Speed:** Real-time image acquisition suitable for quality control pipelines
- **Non-destructive:** Water samples are unmodified during imaging

### 17.3 Integration with Our System

DHM captures images of water samples → images are saved/streamed → sent to Flask API → CNN classifier runs → result returned to operator. The CNN was trained on microscopy images from the Kaggle dataset (similar imaging conditions to DHM output), making the pretrained classifier applicable.

---

## 18. End-to-End System Summary

```
Water Sample
     ↓
Digital Holographic Microscope
     ↓
Holographic Image (128×128 grayscale)
     ↓
Flask API receives image
     ↓
Preprocessing:
  - Resize to 128×128
  - Normalize pixels [0,1]
  - Standardize (zero mean, unit variance)
     ↓
MobileNetV2 (frozen, ImageNet weights)
  - Extracts 1280 feature representations
     ↓
GlobalAveragePooling2D
  - Aggregates spatial features → 1280-dim vector
     ↓
Dense(256, ReLU) → Dropout(0.4)
Dense(128, ELU) → Dropout(0.3)
Dense(64, LeakyReLU) → Dropout(0.2)
Dense(1, Sigmoid)
  - Output: P(microplastic) ∈ [0, 1]
     ↓
Decision threshold: 0.5
  - >0.5 → Microplastic Detected ⚠️
  - ≤0.5 → Clean ✅
     ↓
JSON Response → Dashboard
  - Result + Confidence score
     ↓
Operator Alert / Quality Control Action
```

### System Performance Summary

| Attribute | Value |
|-----------|-------|
| Accuracy | 70.9% |
| Loss | 0.59 |
| Dataset size | 1,562 images |
| Training epochs | 50 |
| Base model | MobileNetV2 (ImageNet) |
| Input resolution | 128×128×3 |
| Trainable params | ~369,409 |
| Total params | ~3.77M |
| Error tendency | False positive bias (safer for food safety) |
| Deployment | Flask API + Dashboard |

---

*This document provides complete coverage of the Indisposable project — from problem motivation through dataset, preprocessing, CNN theory, transfer learning, model architecture, training, evaluation metrics, results, and deployment. Reading this document in full should equip you to answer any technical or conceptual question about the project.*
