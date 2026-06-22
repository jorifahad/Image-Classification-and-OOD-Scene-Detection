# Image Classification and Out-of-Distribution Scene Detection

This project compares two approaches for image classification and out-of-distribution (OOD) detection:

* Handcrafted features using HOG, PCA, and cosine similarity.
* Deep learning using a convolutional neural network (CNN) and ODIN.

CIFAR-10 is used for known image classes, while CIFAR-100 is used to evaluate the detection of unfamiliar images.

---

## Problem Description

Conventional image classifiers may assign unfamiliar images to known classes with high confidence, even when the prediction is incorrect.

This project addresses the challenge of building a system that can:

* Accurately classify familiar images.
* Detect images that do not belong to the training distribution.
* Compare handcrafted and learned visual representations.

---

## Datasets

### CIFAR-10

CIFAR-10 contains:

* 60,000 RGB images.
* 50,000 training images.
* 10,000 testing images.
* Image size: 32 × 32 pixels.
* 10 categories:

`airplane`, `automobile`, `bird`, `cat`, `deer`, `dog`, `frog`, `horse`, `ship`, and `truck`.

### CIFAR-100

CIFAR-100 is used as the OOD dataset because it has the same image format as CIFAR-10 but contains different object categories.

---

## Approaches

### 1. Handcrafted Features

The traditional pipeline applies:

```text
Image
  ↓
Resize and Grayscale Conversion
  ↓
Gaussian Blur
  ↓
HOG Feature Extraction
  ↓
PCA Reduction to 446 Components
  ↓
Cosine-Similarity Classification
```

HOG captures edge and shape information, while PCA reduces feature dimensionality.

For classification, each image is compared with the mean feature vector of every CIFAR-10 class.

OOD detection is based on the maximum cosine-similarity score. Lower scores indicate that an image is more likely to be out-of-distribution.

### 2. Deep Learning

The deep learning approach uses a CNN trained directly on CIFAR-10 RGB images.

The network includes:

* Convolutional layers.
* Batch normalization.
* ReLU activation.
* Max pooling.
* Dropout.
* Adaptive average pooling.
* A fully connected classification layer.

For OOD detection, the CNN uses ODIN, which applies temperature scaling and a small input perturbation before calculating the maximum softmax probability.

---

## Results

### Classification Performance

| Approach                      | Accuracy | 
| ----------------------------- | -------: | 
| HOG + PCA + Cosine Similarity |   41.86% |     
| CNN                           |      87% |    

The handcrafted method performed better on visually distinctive classes but struggled with similar categories such as cats, dogs, deer, and birds.

The CNN achieved stronger and more balanced classification by learning textures, shapes, and semantic visual features directly from the data.

### OOD Detection Performance

| Approach             | OOD Method                  |  AUROC |
| -------------------- | --------------------------- | -----: |
| Handcrafted Features | Cosine-similarity threshold | 0.5156 |
| CNN                  | ODIN                        | 0.8217 |

The handcrafted method showed considerable overlap between CIFAR-10 and CIFAR-100 scores.

The CNN with ODIN achieved clearer separation between familiar and unfamiliar images.

---

## Main Findings

* CNN features were more effective than handcrafted HOG features.
* HOG captured edges and shapes but lost color and texture information.
* The CNN improved classification accuracy from **41.86% to 87%**.
* ODIN improved OOD AUROC from **0.5156 to 0.8217**.
* Handcrafted features remain useful as an interpretable baseline.

---

## Project Files

| File                         | Description                                                                                   |
| ---------------------------- | --------------------------------------------------------------------------------------------- |
| `Handcrafted_Features.ipynb` | Implements HOG extraction, PCA reduction, cosine-similarity classification, and OOD detection |
| `CNN.ipynb`                  | Implements CNN training, classification evaluation, and ODIN-based OOD detection              |
| `cifar10_cnn.pt`             | Saved checkpoint of the trained CNN                                                           |
---

## Conclusion

The CNN significantly outperformed the handcrafted approach in both image classification and OOD detection.

The handcrafted pipeline was interpretable and lightweight, but its fixed features were not expressive enough for complex natural images. The CNN learned richer visual representations and produced more reliable confidence scores for detecting unfamiliar inputs.
