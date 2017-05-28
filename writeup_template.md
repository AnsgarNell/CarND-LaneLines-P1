# **Finding Lane Lines on the Road** 

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 7 steps. First, I made a selection based on color in order to filter and retain only yellow or white colored pixels. This was done using HLS color codification as read in other projects found in Internet, because of its excellent results. 

Then I converted the images to grayscale and smoothed the result using Gaussian blur function. After doing that I found the edges on the resulting image using the Canny edge detection function. Then I masked the image in order to retain only a zone of interest, which basically consists in a trapezoid there where we expect lane lines will be, in the bottom of the image.

Finally we call the hough_lines function to detect the lines found in the image, which calls the draw_lines function.

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by first grouping each line in the left or in the right side. then we average each side lines and get a single line. With its slope and coordinates, we extend the line to cover more or less the road.


### 2. Identify potential shortcomings with your current pipeline

One potential shortcoming would be what happens when incorrect lines are detected, as the resulting one does not cover the real lane. this happens occasionally with the second and third videos.

Another shortcoming could be when no line is detected in a frame and we have no information for that moment.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to change the OpenCV HoughLinesP function in order to work directly with the rho and theta values. What we do now is that we transform points to lines in Hough space, then find intersections and recover lines for each of this intersections that meet our criteria. After averaging this lines, we build a new one. I think it would imply less computation to work with hough spaces points, average this points and then finally transform the result to a line in image space. Just calculation with two values (rho and theta) instead with four (x1, y1, x2 and y2). This would be possible nowadays with the OpenCV HoughLines function but not with the probabilistic one.

Another potential improvement could be to maintain a history of previous lines to avoid abrupt changes, as the result look a little bit shaky.
