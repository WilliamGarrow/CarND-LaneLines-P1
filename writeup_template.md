# **Finding Lane Lines on the Road** 

## Writeup 

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report

[//]: # (Image References)

[image1]: ./writeup_images/gray_solidWhiteCurve.jpg "Grayscale"
[image2]: ./writeup_images/edges_solidWhiteCurve.jpg "Canny edge detection"
[image3]: ./writeup_images/target_solidWhiteCurve.jpg "Region of interest"
[image4]: ./writeup_images/segments_solidWhiteCurve.jpg "Line Segments"
[image5]: ./writeup_images/output_solidWhiteCurve.jpg "Lines extrapolated"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. The images were first converted to grayscale and then gaussian blur was applied. Canny edge detection was then employed to extract useful structural information. An image mask was then applied to the specified region of interest with a Hough line transformation used for feature extraction to isolate the lane markings. 

1. Apply grayscale and gaussian blur to the image
2. Implement the Canny edge detection
3. Assign an image mask to the specified region of interest
4. Administer a Hough line transformation
5. Merge original image with image of Hough lines

#### Grayscale and gaussian blur
![Grayscale image][image1]

#### Canny edge detection
![Canny edge detection][image2]

#### Image mask of the specified region of interest
![Mask the region of interest][image3]

#### Hough line transformation
![Hough line transformation][image4]


That process provided line segments that I then modified in order to draw a single line on the left and right lanes by modifying the draw_lines() function to extrapolate the line segments into a consistent left and right line. The process decides which segments are part of the left line as well as the right line. The lines segments are separated by their slope with outliers beyond 25 - 70 degrees being tossed out. Finally, the (x, y) sample point coordinates are fitted at once using np.polyfit passing the results through cv2.line by passing starting and ending coordinates of the line.

#### Final pipeline result
![Final pipeline result][image5]



### 2. Identify potential shortcomings with your current pipeline


Generally speaking, this pipeline works on these and visually similar images. The pipeline starts to fall apart on the challenge image when the road conditions change. When the roadway changes color the left line jumps off course and then re-acquires after a moment. Also, I would imagine that in the absence of any lane markings the pipeline would also rapidly degrade. 


### 3. Suggest possible improvements to your pipeline

I have a suspicion that using a neural network would take this pipeline to another level. In the absence of a deep learning methodology, I would like to experiment with a parametric model that correlates response data to predictor data using a least-squares fitting. Additionally, an array of the previous fit line to model to would help the lines to stay constant when the road conditions change. For instance, if new data is wildly different such as a missing lane marker, rather than immediately searching for markings the line would stay within some threshold until requiring the next marking. 
