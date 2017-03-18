# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[result]: ./test_images_output/result.jpg "Result"
[gray]: ./test_images_output/gray.jpg "Grayscale"
[gauss]: ./test_images_output/gauss.jpg "Gaussian filter"
[canny]: ./test_images_output/canny.jpg "Canny Edge Detector"
[mask]: ./test_images_output/mask.jpg "Masked image"
[roi]: ./test_images_output/roi.jpg "Region of Interest"
[hough]: ./test_images_output/hough.jpg "Hough Transform"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. 
 1. First, I converted the images to grayscale,
 
 ![gray][gray]
 
 2. then I used a Gaussian filter to reduce noise in the image. 
 
 ![gauss][gauss]
 
 3. Next, I detected the edges using the Canny edge detector with the following thresholds:
     - low_threshold = 50
     - high_threshold = 150
 
 ![canny][canny]
 
 4. To consider only the region of interest, I applied a polyline mask using "bitwise and" operation between the mask and the edge detected image to get a region of interest.
 
 ![mask][mask]
 
 here is the image of the region of interest
 
 ![roi][roi]
 
 5. In the last step of the pipeline, I applied the Hough transform to get the final image overlayed with the lane lines.
 
 ![hough][hough]
 
 The following settings were chosen for the Hough transform:
    - **rho = 2**
    - **theta = pi/180**
    - **threshold = 15** specifies the minimum number of votes (intersections in a given grid cell) a candidate line needs to have to make it into the output.
    - **min_line_length = 40** is the minimum length of a line (in pixels) that will be accepted in the output
    - **max_line_gap = 20** is the maximum distance (again, in pixels) between segments that will allowed to be connected into a single line

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by using a simple linear least squares method. Using the following equation this yields the parameters ($m$ and $b$) for the straight lines $y = mx + b$ 

$$
\hat{\beta} = (X^{T} X)^{-1} X^{T} y;
$$

where $\hat{\beta} = [b, m]$


The following image shows the final result:

![result][result]


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when light conditions change. This will lead to wrong threshold settings.

Another shortcoming could be curvy roads or different camera angles. This would lead to wrong region of interest selection.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to normalize the images to avoid wrong parameters.

Another potential improvement could be to adjust the region of interest.