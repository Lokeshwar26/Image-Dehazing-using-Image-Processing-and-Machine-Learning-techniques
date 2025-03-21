# 🚀 Image Dehazing Using CNN

## 📂 Dataset Structure
📂 dataset
├── 📂 outdoor
│ ├── 📂 hazy # Hazy images (outdoor)
│ ├── 📂 gt # Ground Truth images (outdoor)
├── 📂 indoor
│ ├── 📂 hazy # Hazy images (indoor)
│ ├── 📂 gt # Ground Truth images (indoor)

---

## 🔹 Dataset Split (80-20)
- **Outdoor Dataset**:  
  - **Training**: `80%` of outdoor images  
  - **Validation**: `20%` of outdoor images  

- **Indoor Dataset**:  
  - **Training**: `80%` of indoor images  
  - **Validation**: `20%` of indoor images  

---

## 🔬 Training Details
- **Dataset Used:** RESIDE (Real and Synthetic Hazy Images)  
- **Data Split:**  
  - **Outdoor Images:** `80% Training` | `20% Validation`  
  - **Indoor Images:** `80% Training` | `20% Validation`  
- **Optimization Algorithm:** Adam  
- **Learning Rate:** `1e-3`  
- **Batch Size:** `32`  
- **Epochs:** `20`  

---

## 🔥 CNN Model Design
### **Layer Information:**
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

✅ **Layer information saved to:** `CNN_Layer_Info.txt`  

---

## ⚙️ Implementation Steps
1️⃣ **Preprocess Dataset**:  
   - Resize images to `256x256`  
   - Normalize pixel values  
   - Convert images into tensors  

2️⃣ **Define CNN Model**:  
   - Multi-scale feature extraction  
   - Attention mechanism for haze removal  
   - Brightness compensation  

3️⃣ **Train Model**:  
   - Loss Function: `Mean Squared Error (MSE)`  
   - Optimizer: `Adam`  
   - Regularization applied  

4️⃣ **Evaluate Model**:  
   - Test on real-world & synthetic images  
   - Compare with baseline models  

---

## 📈 Results
- **Evaluation Metrics:** PSNR, SSIM  
- **Performance Improvements:**  
  - Reduction in haze  
  - Enhanced visibility  
  - Better object recognition  

---

## 📝 Conclusion
This CNN-based **image dehazing model** effectively removes haze from real-world and synthetic images using **multi-scale feature extraction**, **attention mechanisms**, and **brightness compensation**.

---

## 📌 References
- **Dataset:** [RESIDE](https://www.reside-dataset.com/)  
- **CNN Architecture:** Based on state-of-the-art dehazing models  
