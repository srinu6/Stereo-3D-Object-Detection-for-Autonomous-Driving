# Stereo-RCNN-based 3D-Object-Detection-for-Autonomous-Driving
It is implemented from Faster RCNN.
<b>R-CNN -> Fast R-CNN -> Faster R-CNN</b>

# RCNN (Region Convolutional Neural Network):
it has three seperate models, first for extracting image features, second for classify with SVM(Support Vector Machine), third for tighten boundbox with regressor.
1. (a) Input images
   (b) extract Region proposals
   (c) compute CNN features
2. Classify Regions(using SVM)
3. Bounding box(Runs simple linear regression in regional proposal to generate a tighter bounding box coordinates)

# Fast RCNN
This approach is similar to the R-CNN algorithm. But, instead of feeding the region proposals to the CNN, we feed the input image to the CNN to generate a convolutional feature map. From the convolutional feature map, we identify the region of proposals and warp them into squares and by using a RoI pooling layer we reshape them into a fixed size so that it can be fed into a fully connected layer. From the RoI feature vector, we use a softmax layer to predict the class of the proposed region and also the offset values for the bounding box.

#  Faster R-CNN
the image is provided as an input to a convolutional network which provides a convolutional feature map. Instead of using selective search algorithm on the feature map to identify the region proposals, a separate network is used to predict the region proposals. The predicted region proposals are then reshaped using a RoI pooling layer which is then used to classify the image within the proposed region and predict the offset values for the bounding boxes.

1. In the first step, the input image goes through a convolution network which will output a set of convlutional feature maps on the last convolutional layer:
<img href="https://qph.fs.quoracdn.net/main-qimg-f49479a81ab58ebe7dad9f1277d2788c-c">
2. Then a sliding window is run spatially on these feature maps. The size of sliding window is n×n (here 3×3). For each sliding window, a set of 9 anchors are generated which all have the same center (xa,ya) but with 3 different aspect ratios and 3 different scales as shown below. Note that all these coordinates are computed with respect to the original image.


Furthermore, for each of these anchors, a value p∗ is computed which indicated how much these anchors overlap with the ground-truth bounding boxes.
<img href="https://qph.fs.quoracdn.net/main-qimg-254d27efab5509cdd90fff7221863066">

    ⎧  1 if IoU>0.7 
       0 if IoU<0.3
p∗= ⎨ −1 if otherwise
    ⎩
    
where IoU is intersection over union and is defined below:

IoU= (Anchor ∩ GTBox)/ (Anchor ∪ GTBox)

3. Finally, the 3×3 spatial features extracted from those convolution feature maps (shown above within red box) are fed to a smaller network which has two tasks: classification (cls) and regression (reg). The output of regressor determines a predicted bounding-box (x,y,w,h), The output of classification sub-network is a probability p indicating whether the predicted box contains an object (1) or it is from background (0 for no object).


The loss function is defined over output of both sub-networks, with 2 terms and a balancing factor λ.
