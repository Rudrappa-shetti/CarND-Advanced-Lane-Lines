## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

---

**Advanced Lane Finding Project**

The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

[//]: # (Image References)

[image1]: ./output_images/Calibration_Input.png
[image2]: ./output_images/Camera_Calibration.png
[image3]: ./output_images/Undistorted_Image.png 
[image4]: ./output_images/Binary_Image.png 
[image5]: ./output_images/Warped_Image.png 
[image6]: ./output_images/Sliding_Window_Image.png 
[image7]: ./output_images/Detected_lines.png 
[image8]: ./output_images/Processed_Image.png 
[video1]: ./project_video_ouput.mp4

## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points

### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---

### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Advanced-Lane-Lines/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!

### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

Firstly, i loaded a calibration image in which we can observe there are 9 corners in a row and 6 corners in a column. 
![alt text][image1]
I have used glob to iterate over all the camera_cal images to extract the object and image points
![alt text][image2]
The code for this step is contained in the Second code cell of the IPython notebook located in "./CarND-Advanced-Lane-Lines/Advn_lane_lines.ipynb"  

### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.

To demonstrate this step, I will describe how I apply the distortion correction to one of the test images like this one:
![alt text][image3]

#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

I used a combination of color and gradient thresholds to generate a binary image.  Here's an example of my output for this step.

![alt text][image4]

#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

I extracted the vertices manually to perform the perspective transform.i did this first on the sample image and then on the project image.
Below are the 'src' and 'dst' points.


| Source                    | Destination            | 
|:-------------------------:|:----------------------:| 
| bottom left :220, 720     | bottom left  :320, 0   | 
| bottom right:1110, 720    | bottom right :320, 720 |
| top left    :1127, 720    | top left     :960, 720 |
| top right   :695, 460     | top right    :960, 0   |

![alt text][image5]

#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

I then perform a sliding window search, starting with the base likely positions of the 2 lanes, calculated from the histogram. I have used 10 windows of width 100 pixels.

The x & y coordinates of non zeros pixels are found, a polynomial is fit for these coordinates and the lane lines are drawn.

![alt text][image6]
![alt text][image7]

#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

The radius of curvature is computed according to the formula and method described in the classroom material. Since we perform the polynomial fit in pixels and whereas the curvature has to be calculated in real world meters, we have to use a pixel to meter transformation and recompute the fit again.

The mean of the lane pixels closest to the car gives us the center of the lane. The center of the image gives us the position of the car. The difference between the 2 is the offset from the center.



#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

![alt text][image8]

---

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a link to my video 
[link to my video result](./project_video_output.mp4)

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

I had problems with Gradient and Color Thresholding,since i have to play around with a lot to come to final conclusion. Since to make work in all kind of scenarios we have carefully choose 
R&G channel thresholding and L channel thresholding. And also if the frames are bad,i mean the brightness of the pixel is not good then i faced some problems.So i have to use R&G channel thresholds and 
L channel thresholds.

Pipeline will fail if there are sharp turns in road and also if the vehicle is travelling in high speed.

To overcome above problems i have to design better and more optimised perspective transform which works on the smaller section to perform the transform.
And for the second problem i have to do the averaging on more number of frames,since when vehicle is travelling fast then the shape and directions of frame will change very fast.
 
