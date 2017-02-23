**Finding Lane Lines on the Road**

In this project we try to detect the lane markings on a given videostream.

[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"
---

### Reflection

###1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipline consists of the following steps:
- Convert image to grayscale 
- Smoothen the image using gaussian blur
- Filter out non-lane colored parts of the image
- Edge detection using canny operation
- Reduce to area of interest
- Detecting lanes using hough transformation

After this processing pipeline, my output has many lines that define where the potential lane lines are.
Inside the draw_lines() function I calculated the slopes for each potential lane definition and rejected the ones
that are outside of the slope angle threshold.

And the final step is the clustering of the found lines. I calculated the intersection of these lines with the x-axis and
used k-means clustering the separate the results into two groups (left lane, right lane). The idea with x-axis intersection is to keep the pipeline more general and the found lanes independend from a given definition (i.e curved lanes into same direction might make them look similar and difficult to keep apart).

Finally I did overlayed the found average lines for each result group and overlayed on the original image that is being displayed.


###2. Identify potential shortcomings with your current pipeline
My implementation assumes a set color range for the lane markings. It does not consider the light conditions so I assume in the night setting the results would be much different. Also in case of a lane change, there might be 3 valid lanes on the screen, that my pipeline doesn't expect (assumes 2 lanes).

###3. Suggest possible improvements to your pipeline

Possible improvement could be to use a different color space to deal with the light conditions better.
Also clustering directly in the hough space might be a better and more stable approach.
Another improvement could be to look at the color histogram. Since the lane markings are always a very small percentage of the actual image, this could be potentially detected there too.
