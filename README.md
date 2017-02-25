

# Traffic signs classification with a convolutional network

Here is my submission for the Udacity SDCND P2 - Traffic sign classifier. 

## Dataset

The dataset used is the German Traffic Sign Dataset which consists of 39,209 32×32 px color images for training data, and 12,630 images for testing. There are 43 classes.



The data set is provided as a pickle file with 32×32×3 array of pixel intensities, represented as [0, 255] integer values in RGB color space. Class of each image is encoded as an integer in a 0 to 42 range. Dataset is very unbalanced, and some classes have more train/test examples than the others. The images also differ significantly in terms of contrast and brightness, so we will need to apply some kind of normalization to improve feature extraction.

## Augmentation

The amount of data we have is not sufficient for a model to generalise well. It is also fairly unbalanced, and some classes are represented to significantly lower extent than the others. The data augmentation is done by generating 200000 new images. These generated images are generated on the fly and not stored in the disk. 
Random amunt of shearing, rotation and translation. This function was taken from a post by Vivek yadav. Finally the original tranining data set was added to the augmented data. 
20% of the training data was used for cross validation. 

## Preprocessing

The usual preprocessing in this case would include scaling of pixel values to `[-0.5, +0.5]` (as currently they are in `[0, 255]` range), representing labels in a one-hot encoding and shuffling. Therefore, each pixel is normalized by subtracting 127.5 from it and then dividing by 255. 

## Model 

### Architecture

The model used is inspired by http://navoshta.com/traffic-signs-classification/. It is fairly simple and has 4 layers: **3 convolutional layers** for feature extraction and **one fully connected layer** as a classifier.

<p align="center">
  <img src="model_architecture.png" alt="Model architecture"/>
</p>

As opposed to usual strict feed-forward CNNs I use **multi-scale features**, which means that convolutional layers' output is not only forwarded into subsequent layer, but is also branched off and fed into classifier (e.g. fully connected layer). Please mind that these branched off layers undergo additional max-pooling, so that all convolutions are proportionally subsampled before going into classifier.

### Regularization

No regularization was used. 

## Training

Training is done with 20 epochs and batch size of 256 with an Nvidia GTX 1060 GPU. The validation accuracy of 0.995 was achieved. 

## Results

The test results were rather poor, only reaching 0.930 accuracy on the test data set of 12630 examples. Many test images taken from the internet were tested. 4 out of 5 predictions were correct. The network could not detect "80kph" sign. It was detected as a 20kph sign. 
The results could be greatly improved by the following. 
1. Better preprocessing like histogram equalization
2. Adding regularization like dropout, l2 regularizer etc. 
