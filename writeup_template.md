#**Finding Lane Lines on the Road** 

##Writeup Template

###You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

###1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of following steps.

1. First, convert the image to grayscale image
2. Perform Gaussian blurring in order to reduce noise
3. Do canny edge detection on the filtered image
4. Finally, detect lines using HoughP transform on the canny edge detected image
5. Define region of interest based on the image shape
6. Blend the detected lines on the original image

In order to draw a single line on the left and right lanes, I modified the draw_lines() function. Here is the brief outline of my modification:
1. For all the lines detected by HoughP transform, find the slopes and separate positive and negative slopes into separate list
2. Filter each of the positive and negative slope list such that 20deg<=abs(slopes)<=80 deg are included and the rest is discarded. This help to filter out close to horizontal lines being detected.
3. For each filtered slope set (i.e. positive and negative), separate the (x,y) points constituting the negative and positive slopes, separately. While collating the (x,y) points separately for each positive/negative slope set, make sure that they lie on the left and right half of the region of interest. 
4. Do a linear fit separately on (x,y) set collected for positive slope and negative slope set. This will give the overall positive slope and the corresponding intercept, similarly the negative slope and intercept
5. Once the slopes and intercept are known, draw the left and right lines by finding the start and end points appropriately based on region of interest.

Here is the performance of the pipeline on the images: 

![solidWhiteCurve](file:///test_images_output/solidWhiteCurve.jpg)
![solidWhiteRight](file:///test_images_output/solidWhiteRight.jpg)
![solidYellowCurve](file:///test_images_output/solidYellowCurve.jpg)
![solidYellowCurve2](file:///test_images_output/solidYellowCurve2.jpg)
![solidYellowLeft](file:///test_images_output/solidYellowLeft.jpg)
![whiteCarLaneSwitch](file:///test_images_output/whiteCarLaneSwitch.jpg)


###2. Identify potential shortcomings with your current pipeline

One potential shortcoming is that it is difficult to detect the lane marking if the intensity is varying (e.g. shadowing in the image or some sort of occlusions of the lane markings)

I am not sure how the lane detection will work in night in poor lighting conditions

###3. Suggest possible improvements to your pipeline

A possible improvement would be to convert the image to HSV and looks for specific color, separately. Then do the canny edge detection and remaining pipeline on this image. It a bit more robust as shown in the code for Optional Challenge in notebook

