# Image Dehazing using CNN 🚀

## 📂 Project Structure

📂 **Image-Dehazing**  
├── 📂 **dataset**               # Contains all dataset files  
│   ├── 📂 **outdoor**  
│   │   ├── 📂 **hazy**          # Hazy images (outdoor)  
│   │   ├── 📂 **gt**            # Ground Truth images (outdoor)  
│   ├── 📂 **indoor**  
│   │   ├── 📂 **hazy**          # Hazy images (indoor)  
│   │   ├── 📂 **gt**            # Ground Truth images (indoor)  
├── 📂 **models**                # Trained models and weights  
│   ├── dehazing_CNN.mat         # Saved trained model  
├── 📂 **notebooks**             # Jupyter/Colab Notebooks for experiments  
│   ├── Image_Dehazing.ipynb     # Training and testing notebook  
├── 📂 **src**                   # Source code files  
│   ├── train.m                  # MATLAB training script  
│   ├── test.m                   # MATLAB testing script  
│   ├── preprocess.m             # Preprocessing functions  
│   ├── customReadFcn.m          # Custom image read function   
├── 📂 **docs**                  # Documentation and model info  
│   ├── CNN_Layer_Info.txt       # CNN Model Layer Information  
├── dataset_split.mat            # Preprocessed dataset split file  
├── README.md                    # Project documentation  
├── requirements.txt             # Dependencies and requirements  

---

## 📌 Description
This repository contains an implementation of an **Image Dehazing model using CNNs**.  
The dataset consists of **outdoor and indoor images** with corresponding **hazy and ground truth** versions.  
The model leverages **multi-scale feature extraction, attention mechanisms, and residual learning**.

---

## 🔹 CNN Model Layer Information
**12×1 Layer array with layers:**

| #  | Layer Name            | Type                | Description |
|----|----------------------|--------------------|-------------|
| 1  | `input`              | Image Input       | 256×256×3 images with 'zerocenter' normalization |
| 2  | `conv1_3x3`          | 2-D Convolution   | 64 3×3×3 convolutions with stride [1 1] and padding 'same' |
| 3  | `relu1`              | ReLU              | ReLU |
| 4  | `conv1_5x5`          | 2-D Convolution   | 64 5×5×3 convolutions with stride [1 1] and padding 'same' |
| 5  | `relu2`              | ReLU              | ReLU |
| 6  | `add1`               | Addition          | Element-wise addition of 2 inputs |
| 7  | `conv2`              | 2-D Convolution   | 64 3×3×64 convolutions with stride [1 1] and padding 'same' |
| 8  | `relu3`              | ReLU              | ReLU |
| 9  | `channel_attention`  | 2-D Convolution   | 64 1×1×64 convolutions with stride [1 1] and padding 'same' |
| 10 | `sigmoid1`           | Sigmoid           | Sigmoid activation function |
| 11 | `output`             | 2-D Convolution   | 3 3×3×64 convolutions with stride [1 1] and padding 'same' |
| 12 | `regression_output`  | Regression Output | Mean-squared-error with response 'Response' |

✅ **Layer information is also saved in** `CNN_Layer_Info.txt`

---

## 🔧 Requirements
- MATLAB 2023 or later  
- Deep Learning Toolbox  
- Image Processing Toolbox  
- GPU (optional but recommended for training)  

---

## 🚀 How to Use
1. Clone the repository:  
   ```bash
   git clone https://github.com/yourusername/Image-Dehazing.git
   cd Image-Dehazing
