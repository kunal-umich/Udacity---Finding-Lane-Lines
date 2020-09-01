# **Finding Lane Lines on the Road** 

## Writeup for my implementation

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of the following steps:

1) First, to detect edges in the image, I converted the image to grayscale and passed it to the Gaussian blur function to smooth out the image.

2) Then, using a canny edge detector with a low threshold of 100 and a high threshold of 200, I detected the edges in the image.

3) After applying the Canny edge detector, using the cv2.polylines functions I defined a quadrilateral to select the region of my interest, which in this case were the lane boundaries within which the vehicle is driving.

4) Then, I applied the Hough transform on the image to identify the lane line segments are tuning the parameters.

Example of the pipeline working on the image - 'solidWhiteCurve' is shown below:

![Orignal Image](https://view54dc5dc0.udacity-student-workspaces.com/view/CarND-LaneLines-P1/test_images/solidWhiteCurve.jpg)

![Image after edge detection](https://view54dc5dc0.udacity-student-workspaces.com/view/CarND-LaneLines-P1/test_images_output/solidWhiteCurve_canny.png)

![Orginal Image with region of interest highlighted](https://view54dc5dc0.udacity-student-workspaces.com/view/CarND-LaneLines-P1/test_images_output/solidWhiteCurve_region_selection.png)

![Image after hough detection on region of interest](https://view54dc5dc0.udacity-student-workspaces.com/view/CarND-LaneLines-P1/test_images_output/solidWhiteCurve_hough.png)



After I was satisfied with the results for the individual line segments in the video, I modified the draw_lines(), function in the following manner:

1) First I calculated the slope of the lines selected using Hough transform.

2) If the slope was negative, I appended the x and y values from the line locations to the lists left_lane_x and left_lane_y respectively.

3) If the slope was positive, I appended the x and y values from the line locations to the lists right_lane_x and right_lane_y respectively.

4) Using np.polyfit function, I calculated the average slope and the intercept for both the left and right lanes.

   Example for right lane : right_lane_coeff = np.polyfit(right_lane_y,right_lane_x,1)
   
5) Then I calculated the minimum pixel value in the y-axis using the min function on the lists left_lane_y and right_lane_y. This is done to    find the top portion of the image for the lane line.

   Ex - right_lane_ymin = min(right_lane_y)
   
6) For finding the bottom portion of the line, the maximum pixel value in y axis is set to the image height using imshape[0]
   This is done individually for left and right lanes
   
   Ex - right_lane_ymax = img.shape[0]
   
7) To find the min and max pixel values in x-axis the function np.polyval was used.

   The x-axis values corresponding to y_min and y_max is found using the slope and intercept values found in step 4.
   
   Ex - right_lane_xmin = int(np.polyval(right_lane_coeff,right_lane_ymin))
   
8) Finally two lines are drawn - One for left lane and one for right lane


The 'solidWhiteCurve' iamge is shown below after modifying the draw_lines() function:

![Image after hough detection](https://view54dc5dc0.udacity-student-workspaces.com/view/CarND-LaneLines-P1/test_images_output/solidWhiteCurve_full_hough.png)

### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming I found was that in the video - solidYellowLeft.mp4, the complete lane lines for some sections of the video are not in the correct location.

Also, the pipeline is unable to identify the curved lanes lines in the optional challenge video file.


### 3. Suggest possible improvements to your pipeline

The first shortcoming mentioned above can be fixed by tuning the canny edge detection and Hugh transform parameters.

The curved line may also if detected if we try to modify the Hough transform parameters, maybe through the theta parameter.
