﻿Road_Lane_line_Detection
The project is done as part of Udacity self Driving Car Nano Degree Program.
Goal of the project is to find road lane lines , using Opencv functions.

Pipeline for achieving the goal:

1. Convert the image into grayscale using opencv function.
2. Using Gaussian Blurring with a kernel size of 5 to blur the image to reduce the noise.
3. Use canny edge detection algorithm m on the blurred image to generate a edged image.
4. From the edged image select the region of interest , since its a fair assumption to  make that the camera is in the center of the the car, I smartly selected a area which looks only around a specific range around the center of the image. In actual practice this value can be determined based upon the knowledge of width the lane and field of view of the camera. Selection a optimal region of interest reduces the computational load on process later in the pipeline. 
5. Next we use opencv HoughLinesP function to find straight lines in the image. We can specific different parameters for the function which determines the output of the function. 
6. The output of HoughLinesP is set of lines, we separate the lines based upon the slope of the lines.
7. once the lines have been separated the lines are the extrapolated over all lines in the separated set  to find a optimal line which reduced the squared error for each line. This a achieved by using polyfit function, and we only try to fit a straight line. 
8. We generate equally spaced points between the base of the image an 0.6 times the height of the image and then  use the equation of the line generated by polyfit to fit to these points.
9. The extrapolated line is drawn on a dark image of the same size as of the original image and finally the images are combined to which put the lines on the original image.

The image sequence below decribe the steps above:

![alt text](pipeline_images/original.jpg  )

![alt text](pipeline_images/gray.jpg  )

![alt text](pipeline_images/blurred.jpg  )

![alt text](pipeline_images/ocanny_output.jpg  )

![alt text](pipeline_images/masked.jpg  )

![alt text](pipeline_images/line_img.jpg  )

![alt text](pipeline_images/output.jpg  )





Short comings in the current pipeline described:

1. RGB to grayscale conversion is not the best way to go about the problem, they suffer from lighting conditions , shadows and other similar factors. It can be clearly seen in the challenge video were the lane prediction flickers a bit under shadow cast by tree. 
 I tried using HSV space but was not able to decide on the range of hue space I wanted to cover to get all the lanes.(work still in progress)

2.Secondly hough lane detection is a expensive operation as it works on each pixel and corresponds to a cosine cure in ro, theta space, for large images in scenarios where lane detection is not the only task hough transform can be computationally expensive. Moreover they tend to produce error-some results in case of darker shadows. 

3. Lastly in this project we extrapolated the lines, which as seen in challenge video doesn't work when there are splines. As the extrapolated lines overshoot.

Suggested Improvements:

 one way to put it is to tackle all above listed short comings.

But use of a different color space like HSV can improve the results.

Averaging over n frames for slope and avg line equation  (where n depends on camera's frame rate of the camera ) can tackle a bit the problem of jitters between the frames as well as correcting a bit of the spline.

Lastly look for other alternate algorithms , which can better fit splines. Use algorithms like RANSAC to reject outliers which can improve the accuracy a bit. 



