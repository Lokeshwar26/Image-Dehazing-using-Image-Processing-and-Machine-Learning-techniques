# Image Dehazing using CNN

This MATLAB implementation uses a Convolutional Neural Network (CNN) for dehazing an image, performing object detection, and calculating the Weighted Signal-to-Noise Ratio (WSNR).

## Prerequisites
1. The input image `im` should be in RGB format.
2. The pre-trained CNN weights are saved in `dehaze.mat`.

## Code Implementation

```matlab
function [dehaze, layer_outputs, wsnr] = run_cnn(im)
    % Convert the input image to grayscale for preprocessing
    gray_I = rgb2gray(im);
    
    % Load pre-trained CNN weights and biases from the .mat file
    load('dehaze.mat', 'weights_conv1', 'biases_conv1', 'weights_conv3x3', 'weights_conv5x5', 'weights_conv7x7', ...
         'weights_ip', 'biases_ip');
    
    % Normalize the input image by subtracting 0.5
    haze = im - 0.5;
    
    % Feature extraction (F1)
    F1 = convn(haze, weights_conv1, 'same') + biases_conv1;
    F1 = reshape(F1, [], size(F1, 4));
    F1 = max(F1, 0);
    
    % Multi-scale mapping (F2)
    F2_3x3 = convn(haze, weights_conv3x3, 'same');
    F2_5x5 = convn(haze, weights_conv5x5, 'same');
    F2_7x7 = convn(haze, weights_conv7x7, 'same');
    F2 = cat(4, F2_3x3, F2_5x5, F2_7x7);
    
    % Local extremum (F3)
    F3 = convn(F2, ones(3, 3, 3), 'same');
    
    % Non-linear regression (F4)
    F4 = convn(F3, weights_ip, 'valid') + biases_ip;
    F4 = max(F4, 0);
    
    % Atmospheric light (A)
    [~, ind] = sort(F4(:), 'descend');
    A = im(ind(1));
    
    % Guided filter (F4_filtered)
    F4_filtered = imfilter(F4, ones(3, 3) / 9, 'replicate');
    
    % Final dehazing step
    dehaze = (im - A) ./ F4_filtered + A;
    
    % Object Detection (Hazy and Dehazed)
    detector = load('yolov4.mat');
    hazy_detections = detect(detector, haze);
    dehazed_detections = detect(detector, dehaze);
    
    % WSNR Calculation
    wsnr = weighted_snr(im, dehaze);
    
    % Visualize the results
    visualize_results(im, dehaze, hazy_detections, dehazed_detections, F1, F2, F3, F4, F4_filtered, A, wsnr);
    
    % Return feature maps and WSNR
    layer_outputs = struct('F1', F1, 'F2', F2, 'F3', F3, 'F4', F4, 'F4_filtered', F4_filtered);
end

% WSNR Calculation
function wsnr = weighted_snr(im_original, im_dehazed)
    signal = (im_original - im_dehazed) .^ 2;
    noise = (im_original - mean(im_original(:))) .^ 2;
    wsnr = 10 * log10(sum(signal(:)) / sum(noise(:)));
end

% Visualization function
function visualize_results(im, dehaze, hazy_detections, dehazed_detections, F1, F2, F3, F4, F4_filtered, A, wsnr)
    % Display original and dehazed images
    figure;
    subplot(3, 3, 1), imshow(im), title('Original Hazy Image');
    subplot(3, 3, 2), imshow(dehaze), title('Dehazed Image');
    
    % Feature Maps and Processing Stages
    subplot(3, 3, 3), imshow(F1(:,:,1), []), title('Feature Extraction (F1)');
    subplot(3, 3, 4), imshow(F2(:,:,1), []), title('Multi-scale Mapping (F2)');
    subplot(3, 3, 5), imshow(F3(:,:,1), []), title('Local Extremum (F3)');
    subplot(3, 3, 6), imshow(F4(:,:,1), []), title('Non-linear Regression (F4)');
    subplot(3, 3, 7), imshow(F4_filtered(:,:,1), []), title('Guided Filter on F4');
    
    % Display Atmospheric Light (A)
    subplot(3, 3, 8), imshow(A), title('Atmospheric Light (A)');
    
    % Display WSNR
    subplot(3, 3, 9), text(0.5, 0.5, ['WSNR: ', num2str(wsnr)], 'HorizontalAlignment', 'center');
end

