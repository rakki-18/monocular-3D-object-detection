Let us also try to see some methods used to perform 2D object detection as they are integral to 3D object detection.

## 2D object detection methods

### Approaches without using Deep learning:

Divide the image into some subparts and output the subparts that have the object in it.

Divide the image into many different parts of different sizes and output all the parts that have an object in it.

Divide the image into say 10*10 grid, for every grid, find the centroid and make say 3 new images of different ratios from the centroid and find if the object is present.<br>
This method can be optimized by increasing the grid division and increasing new images generated from the centroid and by including one more model which predicts if 2 bounding boxes predicted are the same.




#### RCNN
Region-based CNN. finds regions by using selective search. This is done by generating initial sub-segmentations so that we have multiple regions from images based on scale, color, texture, and enclosure. Then it combines similar regions to form a larger region and these regions are the RoI.
It initially takes a pre-trained CNN. It then retrains the last layer of CNN based on the number of classes.
Using selective search gets RoI. Reshapes all RoI to input size of CNN.
Inputs the region to an SVM to classify.
Trains a linear regression model to generate tighter bounding boxes.

#### Fast RCNN
Convolutional feature maps are used to make sure that the image is passed only once instead of passing all RoI through CNN.
Image is initially passed through CNN, generates RoI from a proposal method.
RoI pooling layer is used to reshape RoI to input size of CNN. then it is passed onto a fully connected layer.
From the fully connected layers, it is simultaneously passed through a linear regressor for bounding and softmax to predict class.

But this method takes time to generate RoI using the selective search method.

#### Faster RCNN
Instead of using selective search to generate RoI, RPN ( region proposal network) is used which generates RoI from convolutional network feature maps and generates anchor boxes of different shapes.
For each anchor, it predicts if the anchor is an object and a bounding bo regressor to better fit the object.

#### YOLO
Given the input image, it directly tries to find a y label. 
The input image is divided into many grids. For each grid, a y label is calculated.
The y-label consists of the probability of the object being present in the grid, coordinates of the center of the object in the grid, size of the bounding box, classes of the types of object.

If there is more than one object center in the grid, then more than 1 anchor box is used and the size of the y-label increases with the number of anchor boxes used.

Non-max suppression is used to remove multiple bounding box detections for the same object. First, the box with the highest probability is selected. All the bounding boxes with high IoU score with this bounding box is compressed. This process is repeated until there is no more bounding box left.


#### RetinaNet
Makes use of feature pyramid networks. It has a bottom-up pathway(ResNet) which calculates feature maps at different levels. 
It has a top-down pathway and lateral connections. Top-down pathway upsamples the spatially coarser feature maps from higher pyramid levels and lateral connections merge the top-down layers and bottom-up with the same spatial size.
Classification subnetworks- predicts the probability of an object being present at each spatial location for each anchor box and object class.
Regression Subnetwork - regresses the offset for bounding boxes from anchor boxes.
