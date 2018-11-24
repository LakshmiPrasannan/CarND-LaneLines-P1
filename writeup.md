# **Finding Lane Lines on the Road**
---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)



---

### Reflection
My pipeline consisted of 5 steps.

[image1]: ./image_output/Grayscale.jpeg "Grayscale"
Step 1. Convert the given image to Grayscale
![alt text][image1]

[image2]: ./image_output/BlurGray.jpeg "BlurGray"
Step 2. Smoothen the image using Guassian Blur
![alt text][image2]

[image3]: ./image_output/CannyEdgeDetected.jpeg "CannyEdged"
Step 3. Detect the edges using Canny Detection method
![alt text][image3]

[image4]: ./image_output/MaskedImage.jpeg "MaskedImage"
Step 4. Find the region of interest for lane detection
![alt text][image4]

[image5]: ./image_output/HoughLines.jpeg "HoughLines"
Step 5. Detect the Hough Lines using Hough Transform
![alt text][image5]

[image6]: ./image_output/LanedDetected.jpeg "LaneDetected"
Step 6. Detect the lane on color image after averaging and extrapolation of Hough lines
  ![alt text][image6]

Pipeline worked well for the first two videos



Video 1:
https://www.youtube.com/watch?v=_VQuHODtIMo&feature=youtu.be

Video2:
https://www.youtube.com/watch?v=Cv95tlDuumU&feature=youtu.be

Challenge Video: In this video the lane detection wasn't smooth and showed some disturbances with change in light intensities in tree shadows.
https://www.youtube.com/watch?v=5iKRrkJyEHg&feature=youtu.be

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by
1. Considering the tracks to enclose the sides of a polygon, the two sides are considered to be left and right.
2. To distinguish the left and right hough lines, right slope and left slopes are calculated:
  If slope < 0; it is negative slope i.e the line is on the left track , so append the slope and intercepts to left slope and left intercept lists. Here slopes < -(0.3) and < -(infinity) are omitted to prevent underflow.

  If slope > 0; it has positive slope i.e the line is on the right track , so append the slope and intercepts to right slope and right intercept lists. Here slopes > (0.3) are omitted and slopes < infinity are included to prevent overflow.
3. Average mean of the positive slopes , negative slopes , positive intercepts and negative intercepts.

4. The calculated parameters, minimum and maximum co-ordinates of the y co-ordinates in the region is used in the line equation to obtain the other x co-ordinates, which finally enables in drawing lines completing the lanes.


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be that with the change in light intensities the tracks get misaligned and fail to detect the right path.
Another shortcoming could be abrupt change in the structure of the road e.g. like in a gutter can vary the light intensity and the lane detection can fail to detect correctly.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to implement weighted intensities from previous videos frames. i.e. by giving more weight to the previous frame co-ordinate and then extrapolating to the points in the new video frame. This can be done by building a code that handles feedback mechanism.

Another potential improvement could be finding the weighted centroid pixel intensities of each curve, which will help in distributing uniform light intensities to near by curve thereby making the lanes less vulnerable to abrupt light intensity changes like in a shadow or non-uniform road structure.
