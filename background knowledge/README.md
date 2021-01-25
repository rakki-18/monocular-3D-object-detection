### Background knowledge before diving into the papers
CNN is still SOTA in 3D object detection but they do not scale well for 3D object detection.
The dataset generally used for 3D object detection is the KITTI benchmark, which gives the global yaw and local yaw.

Average orientation similarity is generally used as the evaluation metric for KITTI or Average Angular Error.
They are given by,
![image](https://github.com/rakki-18/monocular-3D-object-detection/blob/main/image.png)

         

Egocentric orientation- relative to the camera.<br>
Allocentric orientation- relative to the objects.

High precision lidar(Light Detection and Ranging) point cloud is used for accurate 3D object detection. The paper PointNet showed how to manipulate the point cloud with neural networks.



#### Voxel Net paper (which uses lidar data) overview

Voxel partition divides the 3D space into equally spaced voxels. Then feature encoding layer transforms groups of points in each voxel into a 4D tensor.<br>
3D convolutional layer to the feature encoding layer aggregates voxel-wise features.
Region Proposal Network produces a probability score map and a regression map.

There is a huge gap between lidar-based methods and image-based methods.

### What is MultiBin Loss?
When the graph is not unimodal, it is not advised to use L1 loss or L2 loss.
In the L1 loss, we add an extra term and in the L2 loss, we add the square of the extra term.
When we try to minimize L2 loss, we are maximizing the log-likelihood of Gaussian i.e we are assuming that the graph is unimodal.

In the multi-bin loss, we find out in which bin the target belongs to, and regress from that mode enter. This method is used in FQNet to regress the size of cars where k-means clustering is initially done.

Multi-bin loss predicts the possibility of the target being in the ith bin and the total loss is a weighted average of a classification loss term and a regression term which represents the difference of target from the bin center.

In 3D RCNN, instead of finding the exact bin the target belongs to, a weighted average of all the bins is taken.

IoU score- it is the ratio of the intersection of produced output and the ground truth and the union of produced output and the ground truth.

### K-means clustering
Clusters are homogeneous subgroups such that the elements in one cluster are very similar to all the other elements in the same cluster but very different from the elements in the other clusters.<br>
Algorithm:<br>
- select random k centroids.
- Add all the data points to the closest cluster. ( expectation)
- From each cluster, evaluate the best centroid (maximization)
- Repeat until no change in the assignment of data points to any cluster.
This follows the expectation-maximization method.

#### K-Nearest neighbor for 2D object detection.
Matching an image with the k-nearest neighbors and selecting the top class from it.

#### Support-Vector Machines
Finding a hyperplane with the greatest possible margin between the hyperplane and any point, which will lead to the correct classification of the test data.
Margin is the shortest distance between a point and a hyperplane.
Keypoints are important points like the projection of the center of a 3D bbox or projection of a surface etc.

#### Multi-task Learning
Having a shared encoder to learn low-level features and task-specific decoders to predict the output. The total loss is a weighted loss of all the losses.

#### Mask R-CNN
Used for instance segmentation.
Instance segmentation integrates object detection tasks and semantic segmentation( classifying each pixel into a pre-defined category).<br>
Mask R-CNN is an extension of Faster R-CNN with an additional branch for predicting segmentation masks on each RoI. <br>
RoI pool in Faster R-CNN is replaced by RoIAlign which preserves spatial information.<br>
The output from RoIAlign is fed to 2 convolutional layers to generate a mask for each RoI.


