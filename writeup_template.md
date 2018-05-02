# **Finding Lane Lines on the Road** 


---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./test_images/solidWhiteCurve.jpg "solidWhiteCurve"
[image2]: ./test_images_gray/solidWhiteCurve.jpg  "solidWhiteCurve"
[image3]: ./test_images_blur/solidWhiteCurve.jpg  "solidWhiteCurve"
[image4]: ./test_images_canny/solidWhiteCurve.jpg  "solidWhiteCurve"
[image5]: ./test_images_region/solidWhiteCurve.jpg  "solidWhiteCurve"
[image6]: ./test_images_merge/solidWhiteCurve.jpg  "solidWhiteCurve"

---

## Reflection

## 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 7 stages.

### 1  Covnvert  images to grayscale
  convert image to grayscale image by calling grayscale(image). The stage converts input image into single channel varying degree of gray shades. 
### 2.  Apply gaussian blur to grayscale image
  Apply gaussian smoothing function to graysccall image to reduce image noise and reduce detail. Canny edge detection is susceptible to  Image noise and this stage is used to reduce noise.
### 3.  Apply canny edge detection algorithm.
  Use canny edge detection algorithm to detect edges in the image. three parameters are important to quality of edges found by algorithm, kernel_size, low_threshold, high_threshold. played with differnet kernel sizes , low_threshold and high_threshold to arrive at value that works good for the images/videos. 
### 4. Generate Region of Interest
  Next find region of interest in image/frame. I have used scale instead of hard -coded value to work for varying image/frame sizes. This involves two steps. First using scaling factor, generate vertices boundary of image that of interest to us. second step using vertices boundary mask out all other pixels outside of region.
### 4. Apply Hough lines  algorithm to detect lane lines
  Apply HoughLinesP algorithm to detect left and right lines of lane. Used different Hough parameters to arrive out values that works for the image/frames
### 5. Draw LaneLines
  HoughLinesP generates all line vertices (x1,y1,x2,y2) in the image. using positive slope (right line) , negative slope(left lane) sort line parameters in right and left line containers. For videos, drawing lines of lanes for each frame using just current frame data would not work if lane lines are curvy or changes the position. having average slope calcualted across all frames helps to produce smoother lane detection.  This is definitley true for challenge video. in Challenge video, lines are not straight and they are curvy in later frames and average slope would help to detect the lanes accurately


Fianally I have defined two python objects LaneParameters and Lane. Laneparameters is object container for system parameters like , Kernel_Size, low_threshold. Lane is object container for holidng left,rght line average slope and other variables for lane detetection


If you'd like to include images to show how the pipeline works, here is how to include an image: 

![alt text][image1]
![alt text][image2]
![alt text][image3]
![alt text][image4]
![alt text][image5]
![alt text][image6]


## 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when lane lines are curved lines. my pipeline works very well to certain degree of curved but if the roads are winding and lanes are mix of straight and curved then my pipeline certainly would fail in detecting them.

 


## 3. Suggest possible improvements to your pipeline

A possible improvement would be to adjust region of interest detection to fit lane width as it varies in successive frames. As you can see in challenge video, as lane width changes in successiv frames, right lane line detection is either too short or outside of actial lane. my possible explanation is region of interest is do not cover the variablitiy in the video. 

Another potential improvement could be to actually draw lane lines to all the way to the top of lane (point at which both left and right lane lines meet). I might have to adjust the region of interest as well as to address the curved lines 
