#**Finding Lane Lines on the Road** 

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

###1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 6 steps:
1. Converting the image to grayscale
2. Using Gaussian blur for removing some noise
3. Detecting edges using Canny algorithm
4. Specifing the region for searching lines
5. Finding lines using Hough algorithm on specified region
6. Drawing found lines on the original image

In order to draw a single line on the left and right lanes, I made several modification of the draw_lines() function:

1. Separated lines segments by its slope to left and right
2. Filtered out lines which are close to horizontal axis
2. Calculated average for left and right lines separately

I tested several ways to find the average:

1. Just mean of slope and intercept of all line segments
2. Use all lines' end points instead of line segments and calculate a line which better fits all points using linear regression (scipy.stats.linregress)
3. Calculate average of slope and intercept of all line segments using line length as weight, i.e. longer line segment gives more to the final average value.

The 3rd way was the best one and I kept it as a final solution.

After I got nice videos for main challenge I found that the pipeline works too bad with the optional challenge. I found that it doesn't work due to the board left to the road, the car right to the road and tree branches' shadows.
I played a bit with parameters, updated the region of interest, made more aggressive bluring and filtering of line segments and video became much better.

###2. Identify potential shortcomings with your current pipeline

Potential shortcomings I can imagine:

1. Too curvy road when lane lines are not strate lines at all. Pipeline will not find lines.
2. All lines on test images and videos form a triangle. If one lane line will be vertical it will be ingored in draw_lines function.
3. Parameters are too strictly connected to the image/video. Pipeline should be more flexible.

###3. Suggest possible improvements to your pipeline'

The most important improvement I think is remembering previous lane lines as they can't change significantly from frame to frame. Maybe to limit the change of the line slope and don't make changes bigger than that limit from the previous to the next frame. This will improve the stability of finding lane lines.

Such improvment would help with my solution for optional challenge because the most frequent problem I saw was jumping lane line for several frames and then returning to correct position.
