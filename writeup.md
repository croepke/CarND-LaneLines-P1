#**Finding Lane Lines on the Road** 

---
### Reflection

###1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 6 steps. First, I converted the images to grayscale (1), then I applied gaussian blur (2) with a kernel size of 3x3 to smooth the image, i.e. to make it easier to detect gradient differences. Afterwards I conducted edge detection using the Canny-algorithm (3) with a min-threshold of 50 and a max-threshold of 150. Afterwards I applied a regional masking (4) in order to limit the subsequent hough transform (5) to the lane area in front of the car.

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by iterating over all line segments that were found by the canny-transform. Depending on the gradient I assigned the found lines either to the left lane (<0) or the right lane, summed up their individual values, and afterwards averaged them to find a mean line left and right. Afterwards I interpolated the lines to reach from the bottom of the image to the center.


###2. Identify potential shortcomings with your current pipeline


One shortcoming of the solution is that lane lines are in fact curved and not straight lines. Hence, it is difficult (and even inappropriate) to fit a linear function to a lane. 

In addition, another shortcoming might be that simply averaging the line results (on the left and on the right) considers all lines equally strong, whereas possible outliers could for instance skew the results.

Also, so far parameters were only tuned and chosen by visual inspection of the results.

Lastly, I had to implement lane imputation because in the second video partly no lane segments were detected (which led to null pointer failures). In these cases I imputed the lane by a reasonable average over previous results.


###3. Suggest possible improvements to your pipeline

In order to adequately represent non-linear lane lines, it might make sense to think about modifying/replacing the Canny-algorithm to also detect curved lanes. 

In order to make the line detection more robust it could make sense to find the median line (i.e. the line that is in the middle of, say, three lines), and give more weight to its line coordinates and then incrementally decrease weights when moving outward.

In order to ensure more analytical rigor, test sets could be defined and  grid search performed on reasonable parameter values (and their combinations) in order to overcome the potential weaknesses of manual inspection.

Lastly, in order to overcome the difficulty of detecting lane segments, it might make sense to pre-process the incoming image even more. E.g. to flatten the image in order to reduce camera/perspective distortions.