# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Summarize results in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 7 steps. First, I converted the images to grayscale, blured slightly with gaussian blur, and ran a canny threshold to calculate the derivatives of the edge gradients. Then I masked a region of interest from the bottom corners to a vanishing point near the camera horizon and used this to remove edges from consideration. After that I used the HoughLinesP transformation to convert and find line segments in Hough space using polar coordinates. After seperating the left and right sloped lines, I sampled them into bins of points. Then I ran linear regression to find the best fit lines through those points. I encapsulated this into a python class LaneDetector.

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by using the LaneDetector class to process the image and produce the needed output. 

If you'd like to include images to show how the pipeline works, here is how to include an image: 

![alt text][image1]


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming could occur when multiple lane lines with the same slope occur within the masked area. One could try a kmeans seperation to bin the lines by slope to account for this.

Another shortcoming could be extra lines created from other objects in the scene, like cars and edges of shadows. This is not able to differentiate between lines of objects and lines on the road.

This is also subject to contrast levels between the lane marking and the surrounding pavement. Lines with overly smudged or faded edges might fail to yeild a strong enough gradient for canny to produce an edge.

### 3. Suggest possible improvements to your pipeline

A possible improvement would be to run this algorithm over a large dataset and parameterize the settings that work best for different lighting and road conditions. The choice of blur size, canny threashold high, low value, and hough line transoform could vary for different conditions.

Another potential improvement could be to stabilize output over multiple frames by comparing new results to old. Ransac or some similar filter could help ignore outliers. Or a PID filter could smooth otherwize sudden transitions.

Another improvement would be to train a semantic neural network that could label the pixels of the image which which lane lines instead of masking. This would help ignore pixels that come from other objects.

