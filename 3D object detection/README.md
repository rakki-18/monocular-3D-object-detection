## Monocular 3D object detection 
We can classify the approaches used into 4 types- Representation transformation, Keypoints, and shape, Geometric reasoning based on 2D/3D constraints, and direct generation of the 3D bounding boxes.

### Representation transformation

In this method, the perspective images are converted to Birds-Eye-View images using Inverse perspective Mapping to reduce the problem of scale variation.


BEV IPM OD
This paper makes use of this to generate a 2D bounding box.

Orthographic Feature transformation
Perspective images are converted to BEV by using a deep learning framework. ( ResNet extracts perspective features, voxel-based features are generated and another ResNet is used to get BEV).

BirdGAN
This uses a GAN to convert perspective to BEV and then detects 3D objects. 

MultiLevel Fusion
This uses the idea of pseudo-lidar, where RGB images are projected to 3D space.
// insert image2.

Pseudo-lidar
Generates Pseudo-lidar from monocular images inspired by MultiLevelFusion and then applies state-of-the-art lidar-based 3D detectors to find 3D boxes.

Pseudo-lidar++
This uses dense 3D representations and spares depth measurement. The problems in this model are edge-bleeding and local misalignment caused by accuracies in depth estimation.

Refined MPL
Solves the problem of point-density in pseudo-lidar i.e point-density has one magnitude higher than point-cloud.
Paper performs structured sparsification by first identifying foreground points.
Foreground points are identified by the supervised and unsupervised method.
The supervised method trains 2D object detection and uses the union of 2D bbox mask as foreground mask.

### Keypoints and shapes
Finding keypoints by extending 2D classification like YOLO, RetinaNet. The size of the object is used to determine the distance to the egocentric object.

DEEP MANTA
First uses a cascaded faster RCNN to find the 2D bounding box, to get the 2D keypoint, to classify, etc.
The best 3D CAD model is selected by 2D/3D matching using the EPnP algorithm.

Earth Ain’t Flat
For 3DOD of vehicles.
3D shape estimation and  6DoF pose from the monocular image. Uses deep MANTA’s formulation to find keypoints. But instead of picking from all the possible 3D images, it uses a basis vector and deformation co-efficient which captures the shape of the object.


3D R-CNN
Estimate the size and shape and renders the scene. A mask and depth map is used to compare with ground truth and generate “render-and-compare” loss.
RoI pooled features with classification-based regression used to estimate the shape and pose.
PCA is used to reduce dimension.

RoI-10D
Learns a low-dimension(6-d) representation of the shape space. RGB features are enhanced by estimated depth. Then pooled to regress rotation,  3D centroid, depth and {w,h,l}. 8 vertices of the 3D bounding box are formed. A corner loss is used between ground truth and predicted labels.

Mono-3D++
It is based on 3D and 2D consistency. Learns low-dimensional representation of shapes via a 2-D landmark using the EM-Gaussian method. Estimates depth and ground plane to form consistency losses.

MonoGRNet
Regresses the projection of 3D center and instance depth and estimates the 3D location. Basically, regresses the offset of the 8 vertices relative to the 3D center. Finds the difference between the 2D bounding box center and the projection of the center of the 3D bbox in the image.

MonoGRNetv2
Regresses the keypoints in the 2D image and uses a 3D CAD model to get depth. Based on mask RCNN, with 2 additional heads. One head regresses 2D keypoints and the other regresses 3D metric dimension.

GPP
Generates virtual 2D keypoints with 2D bbox annotation. Predicts more attributes and forms a maximum consensus set of attributes.

RTM3D
Detects 2D projections of all 8 cuboid vertices and cuboid center. Directly regresses distance, orientation, and size.

SMOKE
Directly predicts the 3DBbox. Loss used is 3D corner loss optimized using distengaled L1 loss.

Monocular 3DOD  with decoupled structured polygon estimation and height-guided depth estimation
Proposes that finding 2D projections of 3D vertices is independent of finding depth.
Finds 8 projected points of the cuboid. Uses vertical edge height for distance estimation. 
Generates 3D cuboid, which is used as seed position in a BEV image for finetuning.

Monoloco
Extracts keypoints, regresses depth with the keypoint segment lengths by using an MLP.
Also uses the approximate height of the object.

### Distance estimation through 2D/3D constraints

Lifting 2D to 3D by adding a branch regressing local yaw. Using geometric hints to solve an over-constrained optimization problem to obtain the 3D location, pioneered by Deep3DBox.

FQNet
Instead of tight-fitting, adds a refinement stage by densely sampling around the 3D seed location and then scores the 3D frames.

Shift-R-CNN
Regresses the offset from the deep3DBox proposal and inputs all 2D and 3D bbox to a fully connected network to get the 3D location.

Multiview Reprojection Architecture
To lift 2D to 3D, instead of solving an over-constrained equation, it introduces a 3D reconstruction layer with IoU loss in perspective view, between projected 3D bbox and 3D bbox and L3 loss in BEV. Uses iterative method to refine cropped cases.

MonoPSR
Generates 3D proposal first and constructs local point cloud. The centroid proposal uses 2D box height and regresses 3D object height to infer depth.
Regresses a local point cloud of the object instead of regressing large depth ranges.

GS3D
Based on Faster RCNN with additional regression head. Generates a rough 3D bounding box. Uses the fact that the height of the projected 3D bbox is around 0.93 times 2D bbox height, from the training data.
Surface extraction module to refine the 3D location.

### Direct generation of 3D proposal

MonoDIS
Directly regresses the 2D bounding box and 3D bbox based on RetinaNet architecture. Uses 2D IoU loss and 3D corner loss. It proposes a disentanglement technique by fixing all elements except a few to ground truth and training. This is rotated to cover all elements and total loss is a sum of all losses.

CenterNet
Regresses a heat map indicating the confidence of object center location and regresses other object properties. Modeling an object as a single point-center of its bounding box.

SS3D
Uses CenterNet to find the center. Then regresses 2D and 3D boxes. Regresses more parameters for 2D and 3D bbox optimization.

M3D-RPN
Regresses 2D and 3D bbox by precomputing 3D mean stats for each 3D anchor. Proposes to use separate convolutional filters to different bins ( depth-aware convolution).

D4LCN
Uses depth-aware convolution from M3D-RPN, Introduces a dynamic filter that inputs depth prediction, and generates a filter-feature volume that generates different filters for each specific location. 
Regresses 2D and 3D bbox simultaneously.

In general, direct 3D generation is difficult.

