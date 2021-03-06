# **Finding Lane Lines on the Road**
---
**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report

---

### Reflection

### 1. The pipeline and draw_lines() function.

My lane detection pipeline consisted of the following steps:
1. Change the image to grayscale to reduce the information.
2. Apply Gaussian smoothing to blur the edges of the grayscale image.
3. Run Canny edge detection on the blurred image using low and high thresholds calculated with Otsu's algorithm.
4. Mask the region of interest to eliminate the sky and the peripheral objects.
5. Run the Hough transform to determine the lines within the ROI.
6. Rerun the ROI mask to remove extrapolated line data outside of the region.
7. If in testing, visually confirm the placement of the lines

![Pipeline Testing Image](test_images_output/solidWhiteCurve.jpg)

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by separating then collecting the data points using the slope of the lines.  I then used the polyfit function to calculate the best fit line, which was drawn on the parameterized image.

Overall I dislike empirically derived thresholds/settings.  Therefore I turned to Google to help find alternatives for setting Canny thresholds. See [Application of Otsu Method In Canny Operator](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.402.5899&rep=rep1&type=pdf)

### 2. Pipeline Shortcomings

This method suffers from numerous shortcomings:
- The original images have not been corrected for shadows or intense light changes.
- The extrapolation method draws incorrect lane markers with larger breaks in the lines or sharper curves.  In addition, there are no boundaries surrounding proper placement of the lines.
- The region of interest is arbitrary.  Indeed, with the different camera position and field of view used for the challenge video, the ROI mask breaks down.
- The numerous pipeline steps would need to be heavily optimized to run real-time in a vehicle.


### 3. Possible Pipeline Improvements

A possible improvement would be to use a camera with known characteristics in a fixed position.  In MATLAB the lane finding demonstrations use this camera information to convert images to birds-eye views.  This restricts the region of interest to a planer surface, eliminating noise introduced by walls, barriers, and other straight faces.

The pipeline is also lacking in error/boundary checks and course corrections.  A running calculation of the width of the lane could be used to set boundary conditions for restricting lane placement.  Or averaging of the slopes of the Hough lines could be weighted against the fitted line.

I think substantial improvements could be made using image correction techniques (which are not my forte) for more robust edge detection: replacing yellow lane markers with white prior to converting to grayscale or working to minimize the impact of shadows.  However, looking at these sunny images always leads me to one thought; nothing defeats a lane detection algorithm like a snowstorm.