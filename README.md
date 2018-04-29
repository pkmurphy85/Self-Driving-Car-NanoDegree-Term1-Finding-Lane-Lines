<<<<<<< HEAD
# Self-Driving-Car-NanoDegree-Term1-P1
# Self-Driving-Car-Nanodegree-Term1-Advanced-Lane-Finding
=======
# **Finding Lane Lines on the Road**

File/directory descriptions:

1. examples - examples images and videos
2. output_images - image outputs at different stages of pipeline
3. test_images - images to test pipeline on
4. test_videos - videos to test pipeline on
5. test_videos_output - video output from pipeline
6. video_thumbnails - video thumbnails used in writeup
7. P1.ipynb - jupyter notebook with project code
8. instructions.md - project instructions


When we drive, we use our eyes to decide where to go.  The lines on the road that show us where the lanes are act as our constant reference for where to steer the vehicle.  Naturally, one of the first things we would like to do in developing a self-driving car is to automatically detect lane lines using an algorithm.

In this project you will detect lane lines in images using Python and OpenCV.  OpenCV means "Open-Source Computer Vision", which is a package that has many useful tools for analyzing images.  


The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg

[original]: ./test_images/solidWhiteRight.jpg

[grayscale]: ./output_images/gray.jpg

[blur]: ./output_images/gray.jpg

[canny]: ./output_images/canny.jpg

[masked]: ./output_images/masked_canny.jpg

[hough]: ./output_images/hough.jpg

[hough+original]: ./output_images/hough+original.jpg

[extrapolate]: ./output_images/extrapolate.jpg

[final]: ./output_images/final_output.jpg



---

### Reflection

My pipeline consisted of 6 steps.

1. First, the image is coverted to grayscale.

2. Next a Gaussian blurring is applied to the gray scale image. This helps supress noise in the image through averaging. This is already done within openCV's canny edge detection function but we perfrom this because:
    i. This allows us to perform additional smoothing and
    ii. The gaussian blur pefromed in the canny function is not a changeable parameter.  

3. Then canny edge detection is applied to the image. Canny edge detection essentially computes the gradient on the image. The brightness of each pixel corresponds the the gradient at that point. Pixels with the strongest gradients represent the edges in the image. OpenCV describes the Canny edge detection in further detail: https://docs.opencv.org/2.4/doc/tutorials/imgproc/imgtrans/canny_detector/canny_detector.html

4. Next the edge detected image is masked with a region of interest. In our case we are interested in the road line, so we define a trapezoid near the bottom of the image as a mask. This helps emsures we are only evaluating lines that are of potential interest.

5. Then we pass the masked image through a hough transformation. A hough transform is a method of detecting lines in an image. After passing our edge detected image through the hough transform, we will get a set of equations that define the lines in the image.   OpenCV documentation describes the hough transform in further detail: https://docs.opencv.org/2.4/doc/tutorials/imgproc/imgtrans/hough_lines/hough_lines.html

6. Finally we need to take this set of lines and boil them down to two lines, one for each lane marker on the road. we do this by
    i. Converting the lines from a point to point representation (line segments defined by two (x,y) pairs representing the end points) to a slope/intercept representation.
    ii. Separating the set of lines based on there slope. for an image the origin is in the top left corner with positive x values extending to the right and positive y values extending downward. Positive slopes will correspond to the right lane marker while negative slopes will correspond to the left lane marker. In this step we also filter out slopes close to 0. This will prevent lines that are close to horizontal (which likely do not represent an edge on the lane marker) from being included.
   iii. After separating the lines by slope we average them to get a single line representing each lane marker. Longer line segments are given more weight, the assumption being that longer lines are more likely to represent the lane maker. The result is a single line for each lane marker represented in slope intercept form.
    iv. Finally the lines are converted from slope/intercept form back to a point to point representation so we can draw them on our original image.

Below is a visualization of a single image as it moves through the pipeline.  


Original
![alt text][original]  

Grayscale
![alt text][grayscale]  

Blurring
![alt text][blur]  

Canny Edge Detection
![alt text][canny]  

Masked by Region of Interest
![alt text][masked]  

Hough Transformation
![alt text][hough]  

Hough Lines on Original Image
![alt text][hough+original]  

Averaging/Extrapolation
![alt text][extrapolate]  

Final Output
![alt text][final]  



## Processing Video

An additional step was added for processing video. The output of the final step in the pipeline results in two lane lines for that image. For video that output is averaged with the outputs for the previous 10 frames to determine the best lane lines. This helps reduce jitter in our lane lines in the resulting output video. Below are the results on the pipeline in two test videos.

Click to watch!
[![Video 1](http://i.imgur.com/CWucwRa.png)](https://www.youtube.com/watch?v=0PPlycpj68w&feature=youtu.be "Finding Lane Lines, Video 1 - Click to Watch!")

Click to watch!
[![Video 2](https://imgur.com/EcoU3RV.png)](https://www.youtube.com/watch?v=qnQRJvRumnE&feature=youtu.be "Finding Lane Lines, Video 2 - Click to Watch!")

### 2. Shortcomings


One short coming is the region of interest mask. The mask assumes the camera in a certain position, resulting in the lane lines being in a predictable place in the frame.

Another shortcoming is while the pipeline seems to perfrom well for relatively straight lane markers, it does not perform as well on curved ones. This is likely because we are trying to fit a single straight line to the lane markers, which will perform poorly if the lane markers have a more complex shape.



### 3. Potential Improvements

Right now the pipeline uses a few global variables to process image files. I would like to move away from using global variables that need to be reset when presented with a new video.

Another potential improvement is a more dynamic creation of the region of interest mask that is not as dependent on camera placement.
>>>>>>> 4db520f4a0f68226f7b34d8ab1b96ea764557ccc
