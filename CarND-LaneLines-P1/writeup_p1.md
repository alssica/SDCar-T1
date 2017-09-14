# **Project Writeup - Lane Finding** 

## Reflection

### 1. Pipeline. 

My pipeline consisted of 5 steps:
1. convert image to grayscale;
2. apply gaussian blur;
3. apply Canny transformation;
4. poly-mask the region of interest on image;
5. convert to Hough space and draw line.

To draw lines that fit the lanes, I modified the draw_lines() function by:

1. averaging and interpolating the small lines identified on the image (through pipeline steps 1-4). This include:
  a. identify if a line segment is on the left (slope<0), or on the right (slope>0);
  b. finding the average slopes(m) and intercepts(b) for both left and right lines;
  
2. Link the drawing boundaries defined by poly-region mask, to map out the begin and end points of the lane lines;

3. use these points and the cv2.line function to draw lines.

---

### 2. Potential shortcomings with the current pipeline

1. Currently the interpolation and averaging are happening per frame of the video, causing the lines "trembling" and "shaky". It probably needs a time-smoothing algorithm to reduce noise and smooth out change over time.

2. Also the current pipeline does not work great with the challenge video. Probably need to implement a method to increase contrast of the yellow lane so it could be detected better?


### 3. Possible improvements to current pipeline

1. It might be worth trying to switch the order of region masking and draw line - draw the lane lines first, then use region to mask them. This way the lines can be drawn all the way across the image (so the drawing boundary parameter in draw_line function could be eliminated)

2. Several ideas to improve the averge & interpolate algorithm: 
  a. assign weight to different line segments detected based on their lengths (give more weight to the longer lines, less to the short ones, as long ones are the more "steady" ones and are less likely to be noises)
  b. or, assign weight based on standard deviation of each segment's slope to the average slope?
 
3. To boost contract & make better detection of lines: try implementing a hue space converstion first, and increase the yellow contrast?
