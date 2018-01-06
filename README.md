# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 7 steps:

* Convert Image to grayscale
* Smooth image (gaussian_blur) with 3x3 Kernel
* Calculating image median to dynamically adjust tresholds of edge detection 
* Finding edges with Canny-Algorithm 
* Using morphological transformations (in this case dilation) to compensate pixelated canny results
* approximately mask the region of intererest (roi) of the image via given vertices of a polygon
* finding proper line segments in the image with hough-transformation

In order to draw a single line on the left and right lanes, i took the lines generated from the hough-transformation and calculated their gradients to order them onto the left or right side (corresponding left or right lane). ALines with too high or too low gradients are discarded. After that i calculated the borders of a left and right rectangle (min/max for x and y) and interpolated a linear function > f(x)  onto the diagonal of these rectangles. The lanes are now drawn via the intersection of these two linear functions and their corresponding lower image-borders.



### 2. Identify potential shortcomings with your current pipeline

The interpolation is linear, which means that this algorithm won't work in curved road scenarios.
The borders jump, because the lane information differs from frame to frame as the image brightness / road surface changes.

### 3. Suggest possible improvements to your pipeline

A possible improvement would be to do a clothoid interpolation / segmentation of the road and it's lane markings. This would result in better approximation in curved scenarios.

Another important thing wuld be to use more statistical analysis of the image (especially in specific image segments) to better compensate the different image brightnesses and contrasts while moving. 

The most important improvement wuld be to filter (for example "kalmann") the results of the interpolated lanes on a frame by frame basis. This would solve the "flickering" and result in a much more reliable (and stable) driving corridor.
