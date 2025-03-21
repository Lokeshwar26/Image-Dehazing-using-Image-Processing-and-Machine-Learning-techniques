# **Requirements for Image Dehazing using CNN in MATLAB**

## **1️⃣ Software & Environment**
- **MATLAB** (with Deep Learning Toolbox, Image Processing Toolbox)
- **Visual Studio Code** (optional, for code editing)

## **2️⃣ Hardware Requirements**
- **GPU (Recommended for faster training):** NVIDIA GPU with CUDA support
- **CPU (Minimum requirement):** Intel i5/i7 or equivalent
- **RAM:** At least 8GB (16GB recommended)
- **Storage:** At least 20GB free space for dataset and model

## **3️⃣ Dataset Requirements**
- **Dataset Used:** RESIDE (SOTS - Indoor & Outdoor)
- **File Format:** JPEG, PNG
- **Dataset Structure:📂 dataset
├── 📂 outdoor
│ ├── 📂 hazy # Hazy images (outdoor)
│ ├── 📂 gt # Ground Truth images (outdoor)
├── 📂 indoor
│ ├── 📂 hazy # Hazy images (indoor)
│ ├── 📂 gt # Ground Truth images (indoor)
**

## **4️⃣ MATLAB Functions Required**
- `imageDatastore()` – To store and manage image datasets
- `imread()` – To read images
- `imresize()` – To resize images (256x256)
- `im2double()` – To normalize pixel values
- `trainNetwork()` – To train the CNN model
- `save()` – To save dataset splits and trained model

## **5️⃣ CNN Model Architecture**
- **Input Layer:** 256x256x3 image input
- **Multi-Scale Feature Extraction:**
- 3x3 convolution (64 filters) + ReLU
- 5x5 convolution (64 filters) + ReLU
- **Residual Learning:**
- Addition layer
- 3x3 convolution (64 filters) + ReLU
- **Attention Mechanism:**
- Channel Attention (1x1 convolution + Sigmoid)
- **Output Layer:**
- 3x3 convolution (3 filters) for dehazed image
- Regression layer for pixel-wise comparison

## **6️⃣ Training Setup**
- **Optimizer:** Adam
- **Learning Rate:** 1e-4
- **Mini-batch Size:** 8
- **Max Epochs:** 50
- **Validation Split:** 20% (Train: 80%, Validation: 20%)
- **Data Shuffle:** Every epoch

## **7️⃣ Output Files**
- `dataset_split.mat` – Stores train and validation datasets
- `dehazing_CNN.mat` – Trained CNN model
