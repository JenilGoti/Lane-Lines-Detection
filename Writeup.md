# **Lane Lines Detection Using** 
### In this project, I used Python and OpenCV to detect lane lines on the road. I developed a processing pipeline that works on a series of individual images, and applied the result to a video stream.

![jpg](output_example.jpg)

Pipeline:
---
1. Load test images.
2. Apply Color Selection
3. Apply Canny edge detection.
   - Apply gray scaling to the images.
   - Apply Gaussian smoothing.
   - Perform Canny edge detection.
4. Determine the region of interest.
5. Apply Hough transform.
6. Average and extrapolating the lane lines.
7. Apply on video streams.


### **1. Loading test images:**
----
We have 6 test images. I wrote a function called `list_images()` that shows all the test images we're working on using matplotlib.


### **2. Color Selection:**
----
Lane lines in the test images are in white and yellow. We need to choose the most suitable color space, that clearly highlights the lane lines.
I applied color selection to the original RGB images, HSV images, and HSL images, and found out that using HSL will be the best color space to use.

### **3. Canny Edge Detection**
----
We need to detect edges in the images to be able to correctly detect lane lines.
The Canny edge detector is an edge detection operator that uses a multi-stage algorithm to detect a wide range of edges in images.
The Canny algorithm involves the following steps:
- Gray scaling the images: The Canny edge detection algorithm measures the intensity gradients of each pixel. So, we need to convert the images into gray scale in order to detect edges.
- Gaussian smoothing: Since all edge detection results are easily affected by image noise, it is essential to filter out the noise to prevent false detection caused by noise. To smooth the image, a Gaussian filter is applied to convolve with the image. This step will slightly smooth the image to reduce the effects of obvious noise on the edge detector.
- Find the intensity gradients of the image.
- Apply non-maximum suppression to get rid of spurious response to edge detection.
- Apply double threshold to determine potential edges.
- Track edge by hysteresis: Finalize the detection of edges by suppressing all the other edges that are weak and not connected to strong edges.
If an edge pixel’s gradient value is higher than the high threshold value, it is marked as a strong edge pixel. If an edge pixel’s gradient value is smaller than the high threshold value and larger than the low threshold value, it is marked as a weak edge pixel. If an edge pixel's value is smaller than the low threshold value, it will be suppressed. The two threshold values are empirically determined and their definition will depend on the content of a given input image.

### **4. Region of interest**
----
We're interested in the area facing the camera, where the lane lines are found. So, we'll apply region masking to cut out everything else.

### **5. Hough Transform**
----
The Hough transform is a technique which can be used to isolate features of a particular shape within an image. I'll use it to detected the lane lines in `selected_region_images`.


### **6. Averaging and extrapolating the lane lines**
----
We have multiple lines detected for each lane line. We need to average all these lines and draw a single line for each lane line.
We also need to extrapolate the lane lines to cover the full lane line length.

The left lane should have a negative slope (given that `y` coordinate is reversed in the images), and the right lane should have a negative slope. Therefore, we'll collect positive slope lines and negative slope lines separately and take averages.
After calculating the average slope and intercept of each lane line, we'll convert these values to pixel points, and then we'll be able to draw the full length lane line.

### **7. Apply on video streams**
----
Now, we'll use the above functions to detect lane lines from a video stream.
The video inputs are in test_videos folder. The video outputs are generated in output_videos folder.

## **Conclusion:**
----
The project succeeded in detecting the lane lines clearly in the video streams.
This project is intended to only detect (mostly) straight lines. A possible improvement would be to modify the above pipeline to detect curved lane line.
Another potential improvement could be to identify the lane boundaries and detect the full lane, not just the lane lines.