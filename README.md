# **Finding Lane Lines on the Road** 

Overview
---

When we drive, we use our eyes to decide where to go.  The lines on the road that show us where the lanes are act as our constant reference for where to steer the vehicle.  

Naturally, one of the first things we would like to do in developing a self-driving car is to automatically detect lane lines using an algorithm.

In this project I have built a pipeline detect lane lines in images and videos using Python and OpenCV.  OpenCV means "Open-Source Computer Vision", which is a package that has many useful tools for analyzing images.

The pipeline follows the following steps:

1. Transform image to grayscale

2. Apply Gaussian filter to remove noise

3. Apply Canny Edge detection

4. Apply mask to focus on region of interest

5. Apply Hough Transform

6. Average across line angles and intercepts
