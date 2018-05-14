# **Finding Lane Lines on the Road** 

---

### Project Goals

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on my work in a written report

![image1](./examples/grayscale.jpg "Grayscale")

---

### Reflection

### 1. Pipeline Description

My pipeline consists of 6 steps. 

First, I converted the images to grayscale in order to detect lines of different colours.

Second, I ran a gaussian filter to remove noise. I used a kernel size of 5.

Third, I applied a Canny edge detector, setting the lower threshold at 50 and the upper threshold at 150. The lower threshold sets the limit below which edges are discarded. The upper threshold sets the limit above which all edges are considered. All edges with gradients between the lower and the upper thresholds are considered only if connected to edges with gradients greater than the upper threshold.

Fourth I masked the output of the Canny edge detector to ensure that only the lane and parts fo the road surrounding it are considered. I set the mask slightly (10)% above the base of the image to prevent the detection of large objects close to the camera which could be interpreted as lines. I was careful not to have the sides follow the lines too closely to cater for lateral movements of the car within the lane. The upper boundary was carefully selected to exclude items that sit above the horizon line.

Fifth I used a Hough Transform, with a threshold of 30, a minimum line length of 20 and a maximum distance between lines of 30. The threshold sets the minimum number of intersections between pixels in the Hough Transform space required for those pixels to be considered a line in the image. It sets the sensitivity of the Hough Transform to smaller lines. The minimm line length sets a threshold above which lines are considered. The maximum distance between lines is particularly important to the stability of the detected lines as it enables the Hough Transform to connect pieces of broken lines.

Here below is a picture of the output of the Hough transform overlayed to the original images.
![image 2](./test_images_output/Hough_image.jpg)

Sixth, in order to draw a single line on the left and right lanes, I calculated the length-weighted average of the slope angle and intercept of the Hough Transform lines. The weighted average was required to reduce sensitivity of the average to short lines in proximity of the camera. To further improve the stability of the weighted average I used the slope angle instead of the slope as fluctuation in the slope increase hyperbolically as the line gets closer to vertical. I also masked all lines with slope angle lower than 0.3radians, to improve the robustness to non-line features (shadows, markings)

Here below are the resulting lines.
![image 3](./test_images_output/Average_image.jpg)

I also created utility functions to process, print and save all test images at once.

### 2. Potential shortcomings with current pipeline

Here are the shortcomings of my pipeline:
1. robustness to edges located within the masked area, with angles greater than 0.3rad (other cars, marks on the road, shadows, wet sourfaces)
2. ability to detect curved lines (e.g. around tight bends) and more complex line patterns (parallel full + broken line)
3. robustness to vehicle movements causing lanes to fall in/out of masked area (e.g. during lane switches, vehicle vibration)
4. robustness to changes in road slope causing undesired/desired features to appear/disappear in/from the masked area (e.g. driving up and down hills in San Francisco)

### 3. Potential improvements to the pipeline

Some potential improvements to the current pipeline:
1. introduce probabilistic filtering (e.g. Kalman filtering). This would enable the pipeline to use information from prior images and from vehicle 6D position, making line detection more stable and robust to non-line features, vehicle vibrations, etc.
2. fit polynomial line model as opposed to straigh line to enable detection of curved lines. 
2. use Neural Networks or other image processing techniques to reject non-line features
