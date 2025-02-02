# Image Dehazing using CNN

This function performs image dehazing using a Convolutional Neural Network (CNN) and various image processing techniques. It aims to remove haze from an input image while also performing object detection and calculating the Weighted Signal-to-Noise Ratio (WSNR) for evaluation.

## Inputs
- **im**: Input hazy RGB image.

## Outputs
- **dehaze**: Dehazed output image.
- **layer_outputs**: Outputs from different layers in the network (F1, F2, F3, F4, F4_filtered).
- **wsnr**: Weighted Signal-to-Noise Ratio (WSNR) between the original hazy image and the dehazed image.

## Function Workflow

### 1. Preprocessing and Parameter Setup
- Convert the input image to grayscale (`gray_I`).
- Load pre-trained CNN weights and biases from `dehaze.mat`.
- Subtract 0.5 from the input image for normalization (`haze = im - 0.5`).

### 2. Feature Extraction (F1)
- Perform convolution on the hazy image using `weights_conv1` and `biases_conv1`.
- Reshape the output of the convolution and apply a max pooling operation in a step-wise manner to extract features.

### 3. Multi-scale Mapping (F2)
- Apply convolutions with multiple kernel sizes (3x3, 5x5, 7x7) using `weights_conv3x3`, `weights_conv5x5`, and `weights_conv7x7`.
- Store the resulting feature maps in a tensor `F2`.

### 4. Local Extremum (F3)
- Apply a 3x3 convolution to `F2` to extract local extrema and store the result in `F3`.

### 5. Non-linear Regression (F4)
- Apply a fully connected layer to `F3` using the weights and biases (`weights_ip`, `biases_ip`).
- Clip the output to ensure values remain within the range [0, 1].

### 6. Atmospheric Light (A)
- Sort `F4` values and identify the brightest pixels.
- Compute the atmospheric light based on these pixels using the original image and the `A` value.

### 7. Guided Filter
- Apply a guided filter to `F4` to improve the quality of the estimated transmission map (`F4_filtered`).

### 8. Final Dehazing Step
- Subtract the atmospheric light `A` from the image.
- Divide the result by the filtered transmission map (`F4_filtered`).
- Add the atmospheric light back to obtain the dehazed image (`dehaze`).

### 9. Object Detection (Hazy and Dehazed)
- Perform object detection on the original hazy image using a pre-trained YOLOv4 detector.
- Annotate detected objects in the hazy image with bounding boxes and labels.
- Repeat object detection on the dehazed image and annotate the results.

### 10. WSNR Calculation
- Compute the WSNR between the original hazy image and the dehazed image.
  - Formula:  
    \[
    \text{WSNR} = 10 \log_{10} \left( \frac{\sum (I_{\text{original}} - I_{\text{dehazed}})^2}{\sum (I_{\text{original}} - \mu_{\text{original}})^2} \right)
    \]
  where \( I_{\text{original}} \) is the original image, \( I_{\text{dehazed}} \) is the dehazed image, and \( \mu_{\text{original}} \) is the mean of the original image.

### 11. Visualize and Annotate Results
- Display the following in a 3x3 grid:
  1. **Original Hazy Image** with object detection bounding boxes.
  2. **Feature Extraction (F1)**: Display the first channel of `F1`.
  3. **Multi-scale Mapping (F2)**: Display the first channel of `F2`.
  4. **Local Extremum (F3)**: Display the first channel of `F3`.
  5. **Non-linear Regression (F4)**: Display the processed `F4`.
  6. **Guided Filter on F4**: Display the filtered version of `F4`.
  7. **Atmospheric Light (A)**: Display the atmospheric light.
  8. **Dehazed Image** with object detection bounding boxes.

## Function Usage

```matlab
[dehaze, layer_outputs, wsnr] = run_cnn(im);

- ![English](Doc1.jphg) **English**  
