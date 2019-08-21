# Stereo-RCNN-based 3D-Object-Detection-for-Autonomous-Driving
It is implemented from Faster RCNN.
<b>R-CNN -> Fast R-CNN -> Faster R-CNN</b>

 <b><li>RCNN (Region Convolutional Neural Network):</li></b>
it has three seperate models, first for extracting image features, second for classify with SVM(Support Vector Machine), third for tighten boundbox with regressor.
1. (a) Input images
   (b) extract Region proposals
   (c) compute CNN features
2. Classify Regions(using SVM)
3. Bounding box(Runs simple linear regression in regional proposal to generate a tighter bounding box coordinates)

 <b><li>Fast RCNN</li></b>
This approach is similar to the R-CNN algorithm. But, instead of feeding the region proposals to the CNN, we feed the input image to the CNN to generate a convolutional feature map. From the convolutional feature map, we identify the region of proposals and warp them into squares and by using a RoI pooling layer we reshape them into a fixed size so that it can be fed into a fully connected layer. From the RoI feature vector, we use a softmax layer to predict the class of the proposed region and also the offset values for the bounding box.

<b><li> Faster R-CNN</li></b>
the image is provided as an input to a convolutional network which provides a convolutional feature map. Instead of using selective search algorithm on the feature map to identify the region proposals, a separate network is used to predict the region proposals. The predicted region proposals are then reshaped using a RoI pooling layer which is then used to classify the image within the proposed region and predict the offset values for the bounding boxes.

1. In the first step, the input image goes through a convolution network which will output a set of convlutional feature maps on the last convolutional layer:
<img src="https://qph.fs.quoracdn.net/main-qimg-f49479a81ab58ebe7dad9f1277d2788c-c">
2. Then a sliding window is run spatially on these feature maps. The size of sliding window is n×n (here 3×3). For each sliding window, a set of 9 anchors are generated which all have the same center (xa,ya) but with 3 different aspect ratios and 3 different scales as shown below. Note that all these coordinates are computed with respect to the original image.

Furthermore, for each of these anchors, a value p∗ is computed which indicated how much these anchors overlap with the ground-truth bounding boxes.
<img src="https://qph.fs.quoracdn.net/main-qimg-254d27efab5509cdd90fff7221863066">
p* is 1 if IoU > 0.7
p* is 0 if IoU < 0.3
  
where IoU is intersection over union and is defined below:

IoU= (Anchor ∩ GTBox)/ (Anchor ∪ GTBox)

3. Finally, the 3×3 spatial features extracted from those convolution feature maps (shown above within red box) are fed to a smaller network which has two tasks: classification (cls) and regression (reg). The output of regressor determines a predicted bounding-box (x,y,w,h), The output of classification sub-network is a probability p indicating whether the predicted box contains an object (1) or it is from background (0 for no object).

<b><li>Stereo RCNN</li></b>
With Stereo RCNN there are two other popular methods for 3D object detection but why stereo then ?
1. LiDAR-based 3D Object Detection : Currently, most of the 3D object detection methods heavily rely on LiDAR data for providing accurate depth information in autonomous driving scenarios. However, LiDAR has the disadvantage of high cost, relatively short perception range (∼100 m). Gaint companies like Tesla uses this method, this costs 50% of car's cost. But Stereo RCNN method costs only  5% of car's cost.

2. Monocular-based 3D Object Detection :  monocular camera provides alternative low-cost solutions for 3D object detection. The depth information can be predicted by semantic properties in scenes and object size, etc. However, the inferred depth cannot guarantee the accuracy, especially for unseen scenes.

Stereo RCNN based 3D Object Detection :  Comparing with monocular camera, stereo camera provides more precise depth information by left-right photometric alignment. Comparing with LiDAR, stereo camera is low-cost while achieving comparable depth accuracy for objects with non-trivial disparities. The perception range of stereo camera depends on the focal length and the baseline.   

Lets look how it works:
<img src="Network Architecture of Stereo R-CNN.png">
Main part we are going to discuss in this network are:
<b><li>Stereo R-CNN Network </li>
<li>Stereo RPN</li>
<li>Stereo R-CNN 
         <li>Stereo Regression</li>
         <li>Keypoint Prediction</li>
</li>       
<li>3D Box Estimation</li>
<li>Dense 3D Box Alignment</li>
</b>
