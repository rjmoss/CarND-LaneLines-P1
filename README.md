# **Finding Lane Lines on the Road** 

## Robert Moss LaneLines-P1 submission

[//]: # (Image References)

[image1]: ./debug_output/solidYellowCurve2Result.jpg "Grouped Hough Lines"

[image2]: ./test_image_results/solidYellowCurve2Result.jpg "Full lane boundaries"

### 1. Pipeline

The pipeline consists of the following steps (draw_lines() has been modified to include steps 6-8):
1. Greyscale the image
2. Apply a guassian blur
3. Find the edges of an image using canny edge detection
4. Apply an image mask to limit the ROI of the image to a quadrilateral roughly encompassing the road.
5. Run hough lines to determine lines on the image
6. Split the lines into two groups: left lane boundary and right lane boundary. 
7. Create a full lane boundary by joining the lines in a group together and extrapolate within the ROI.
8. Draw the lane boundaries on the image.

For the left-right split: lines with negative gradients are assumed to be part of the left lane boundary (and vice versa).

For the line grouping: the first order polynomial line of best fit of the points defining the lines is found (with a higher weighting on the longer lines).

There's also a DEBUG variable which, when True, draws the hough lines on the image in either blue (right lane) or green (left lane). This is very useful when debugging to check that the canny/hough line detection and left-right splitting are functioning as expected. For example:

![alt text][image1]

After the lines have been averaged and extrapolated:

![alt text][image2]


### 2. Shortcomings

The pipeline has various shortcomings due to the various assumptions made when constructing it. These include:
1. The assumption that the lane boundaries can be described by just 2 straight lines, on a straight(ish) road this is valid but does not allow for stronger curves (for example in the challenge.mp4).
2. The pipeline falls down in challenge.mp4 due to the shadows on the road which are detected as edges (see image)


### 3. Improvements

A possible improvement to the pipeline could be to take the colour of the lines into account. For example the lanes are always a lighter shade (yellow or white) than the road (at least in the example images/videos) but the shadows are darker. Filtering for lighter colours could remove the noise from the dark shadows without losing the lines.

Another possible improvement is a tighter range of accepted line gradients (e.g. lines nearly horizontal should never be considered, but maybe only lines with gradients between say 30-60 degrees (or -30 to -60) from the horizontal should be considered to be lane boundaries.

Another way to distinguish between left and right lane boundary Hough lines could be to assume that lines in the left half of the ROI are from the left lane boundary (and vice versa).

Another possible improvement could be to fit a higher degree polynomial as the lane boundary, to take into account the curve in the road.
