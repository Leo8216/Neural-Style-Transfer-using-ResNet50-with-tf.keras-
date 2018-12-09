# Neural Style Transfer using ResNet50 with tf.keras
#### December 9th, 2018
#### By: Leo Barbosa 
---

After doing a bit of research on neural style transfer, I noticed that it was always implemented using pre-trainned VGG16 or VGG19.  I was curious to see what it would look like if implemented using a different deep convolutional neural network (DCNN). I reviewed the 2015 paper, [A Neural Algorithm of Artistic Style](https://arxiv.org/abs/1508.06576), one more time and I did not find anything indicating this could not be done using a different DCNN. So, I went ahead and implemented neural style transfer using a pre-trained ResNet50 http://ethereon.github.io/netscope/#/gist/db945b393d40bfa26006. Why ResNet50?  I wanted to explore a structure that was proven to be great for image classification, and ResNet152 (deeper model with 152 layers instead of 50) won the 2015 ImageNet challenge with a top-5 error rate of 3.57%.

# Neural Style Transfer

This diagram gives a nice overview of Neural Style Transfer. It is a generic sketch => We could use more than 1 content layer and only one style layer. 

In summary, we take a content image, a style image, and a generated image (initially the content image), then we feed them into a pre-trainned neural network.  We then calculate the loss for the content image and for the style image (with respect to the generated image), and add them together for a total loss function - these could have different weights. Then, we calculate our gradients and update the generated image to minimize the total loss function. 

![Image of Neural Style Transfer Diagram](https://github.com/Leo8216/Neural-Style-Transfer-using-ResNet50-with-tf.keras-/blob/master/images/Neural_Style_Transfer.JPG)

# Content and Style Images Used

![Content and Style Images](https://github.com/Leo8216/Neural-Style-Transfer-using-ResNet50-with-tf.keras-/blob/master/images/content_and_style_images.JPG)

# Defining content and style representations

For style transfer, we need to identify the intermediate layers whose output we will use for calculating our content and style loss functions. This was a bit more challenging than when using a VGG16 or VGG19 network.  Below a bit of guidance:

* **For content layers:** Generally, one layer for content seems to work fine.
  
* **For style layers:** Identify the layers that work best for style transfer. I started using a single style layer to see if that layer, after running say 500 iteration, would translate any stlyle to the generated image, and if so how apealing that style was. After doing this with several layers I selected the ones I liked best. The more layers are selected the longer it takes the optimization to run. For this example we use 4 style layers.
  
Please note that when layer 'conv1' was included as a style layer, it dominated the optimization leading to a generated image that had all the colors of the style image but none of the distinctive patterns. For this reason, it is left out in the code block below. This may be useful if there is an application where we want to tranfer the color only.

# Generated Images

![Image of Output Names](https://github.com/Leo8216/Neural-Style-Transfer-using-ResNet50-with-tf.keras-/blob/master/images/output_names.JPG)

![Image of Generated Images](https://github.com/Leo8216/Neural-Style-Transfer-using-ResNet50-with-tf.keras-/blob/master/images/generated_images.JPG)

## Closing Remarks
ResNet50 is different to VGG19 which is used in the paper. It seems that due to the complexity of ResNet50, the feature maps do not work as well as those of VGG16 or VGG19. In order to obtain a better generated image, trial and error using different combinations for content and style layers as well as hyperparameters (learning rate, content weight, and style weight) is needed.

I want to point out here that Neural Style Transfer is very much an art rather than a science. I have determned the generate image is the most appealing, but someone else may say it is not because they find one of the other generated images more appealing.  In addition, if using a diferent content or style image, it is likely these paramers will not yield be the best generated image (for me).

## References
For this work, I leverage a tensor flow tutorial developed by Raymond Yuan https://medium.com/tensorflow/neural-style-transfer-creating-art-with-deep-learning-using-tf-keras-and-eager-execution-7d541ac31398.
