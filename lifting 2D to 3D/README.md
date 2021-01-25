### Lifting 2D bounding boxes to 3D bounding boxes

There are 4 degrees of freedom in 2D detection (center coordinates, length, and breadth).
There are 7 degrees of freedom in 3D detection. (center coordinates, length, breadth, height, and yaw).
Detecting the 3D bbox from 2D is done by regressing the yaw angle and the center with the constraint that perspective projection of 3D should fit tightly into the 2D box.

We can get 64 possible combinations of the 3D boxes from this method which fits into the 2D box. Among these 64, the best one can be selected by IoU score.

In the FQNet paper, after finding the best fit, that is used as a seed and a lot of dense proposals are generated and a neural network tells the best fit from the proposals.

In shift-R-CNN, all 3D bounding boxes are fitted into a fully connected network called shiftNet.

Some other quick methods use the distance between known keypoints. Some other methods involve directly regressing on the depth d.

