# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[raw]: output_images/solidWhiteRight.jpg "raw"
[Grayscale]: output_images/grayscale_solidWhiteRight.jpg "Grayscale"
[Blured]: output_images/blur_gray_solidWhiteRight.jpg "Blured Grayscale"
[Canny]: output_images/edges_solidWhiteRight.jpg "Edges"
[Masked]: output_images/region_solidWhiteRight.jpg "Masked edges"
[Lines]: output_images/lines_solidWhiteRight.jpg "Lines"
[Annotated]:  output_images/ann_solidWhiteRight.jpg "Annotated image"




---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consist of the following 6 steps. 

    1. Convert the images to grayscale.
    2. Apply  Gaussian smoothing.
    3. Apply Canny edge detector.
    4. Apply color mask to remove unwanted edges.
    5. Apply Hough transform to detect lines
    6. Annotated the orignal image with lines detected by setp (5).

    ![alt text][raw]

1. We start by scaling down to different shade of gray instead of having to deal with full colors

![alt text][Grayscale]

2. Now we apply Gaussian smoothing to  suppress noise and spurious gradients.

![alt text][Blured]

3. Apply canny edge detector which works by computing the gradients. The thresholds were chosen based on the assumption that lanes lines color white or yellow.

![alt text][Canny]

4. Now we can apply region mask to discard edges out side th region so we can focus on lanes lines. in this stage we assumed the camra in fixed position and road marking can be found withen given region.

![alt text][Masked]

5. Now we can detect lane left and right lines by extrapolate the line from segments detected by Canny algorithm. we assume the shape of the lanes line can be modeled as lines.

![alt text][Lines]

6. We can now annotated  the original image with lines by ovelying the original image with lines image.


![alt text][Annotated]

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by first looping throght list of line segements to seprate them two lists one for the left side and one for the right side using the sign of the slope and  threshold to discard false positive. Then I used numpy polyfill with  the list of points to fit line and compute line end point using equation (y = x . m + b).


### 2. Identify potential shortcomings with your current pipeline

1. THe pipe line  rely on  lane  been with a given region image size fixed.
2. The pipe line donsn't handle change of light because of the fixed thresholds, which can change  because with condition or object shadow like trees.
3. The pipe line assumes the lanes has high contrast, which not allows true in case of old or dirty road.
4. Modeling the lanes  as stright line instead of courve


### 3. Suggest possible improvements to your pipeline
1. Fit higher degree of polynomial (Curves) instead of a line.
2. For light and contrast we could use anther color space.
3. We could use the percentage based in the image sise instead of fixed value.