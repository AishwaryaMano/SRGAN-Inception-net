# SRGAN-Inception-net
 Losses
	Content loss 
	The Euclidean distance between the feature representations of a reconstructed image and the reference image gives the VGG loss. We make use of the feature map obtained by jth  convolution in ith max pooling layer in calculating the VGG loss. Instead of calculating pixel-wise MSE loss, which gives high PSNR but lacks content of high frequency, we use loss function closer to a perceptual similarity. 
			   
Adversarial loss 
	In addition to content loss, adversarial loss is a generative component added to the perceptual loss. It represents the generative component of our GAN. The generative loss is defined based on the probabilities of the discriminator over all the training samples. 
             
where, the probability that the reconstructed super resolution image is the real high- resolution image is given by DθD ( GθG ( ILR ) ).

 Perceptual loss function 
	The sum of content loss and adversarial loss is formulated as perceptual loss.
Generator and Discriminator models in the project
Generator
Pre-Inception block
	A convolution layer with kernel size as 9, number of filters as 64 and strides as 1.

Inception block
	The first inception layer, d1 is a convolution layer and receives the output of pre inception layer as its input, and the number of filters is 64, kernel size is 1, the number of strides is set to 1, padding is kept as same and the activation is ‘relu’. 
The second inception layer, d1 is a convolution layer and receives the output of d1 layer as its input, and the number of filters is 64, kernel size is 3, the number of strides is set to 1,  padding is kept as same and the activation is ‘relu’. 
	The third inception layer, d2 is a convolution layer and receives the output of pre inception layer as its input, and the number of filters is 64, kernel size is 1,  the number of strides is set to 1, padding is kept as same and the activation is ‘relu’. 
	The fourth inception layer, d2 is a convolution layer and receives the output of d2 layer as its input, and the number of filters is 64, kernel size is 5,  the number of strides is set to 1, padding is kept as same and the activation is ‘relu’. 
	The next layer, d3 is a max-pooling layer and receives the output of pre inception layer as its input, and the number of filters is 64, kernel size is 3, the number of strides is set to 1, padding is kept as same. 
The last inception layer, d3 is a convolution layer and receives the output of d2 layer as its input, and the number of filters is 64, kernel size is 1, the number of strides is set to 1,  padding is kept as same and the activation is ‘relu’.
 Finally, the layer d takes the merged version of d1, d2 and d3 using the concatenate function of keras.layer and this is the layer that is returned from the inception network.


Post-Inception block
	A convolution layer with kernel size as 3, with 64 filters and stride as 1, followed by a de-convolution block with kernel size as 3 with 256 filters and number of strides as 1.
 
Discriminator network
	Used activation- LeakyReLU with (α = 0.2).
	We also avoid max pooling throughout the network, as the discriminator is designed to solve maximization problem.
	There are 8 convolution layers with 3*3 filter kernels. Each of the convolution layer has a 1 stride and 2 stride-convolution. So, 64 map features will have 2 layers, 128 has 2 layers, 256 has 2 layers and 512 has 2 layers with 1 stride and 2 strides respectively. Here number of map-features the number of filters used. The 8 convolution layers are followed by 2 dense layers. The activation function is sigmoid for the dense layers to obtain the probability for sample classification. 
VGG network 
	It usually refers to a deep convolutional network for object recognition developed and trained by Oxford's renowned Visual Geometry Group (VGG), which achieved very good performance on the ImageNet dataset. It is used in the base paper implementation and this project implementation to calculate the features of the image. 

