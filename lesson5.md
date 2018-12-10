﻿# Convolutional Neural Networks

**Q1: Why use a pooling layer?**

To progressively reduce the spatial size of the representation to reduce the amount of parameters and computation in the network, and hence to also control overfitting.

Resource:

-  [cs231n - Pooling layer](http://cs231n.github.io/convolutional-networks/#pool)

**Q2: When trying to run the conv_visualization in my notebook, I'm getting the error `ModuleNotFoundError: No module named 'cv2'`**

Possible solutions:

-  `pip install opencv-python`

-  `conda install -c conda-forge opencv`

**Q3: Why am I getting this error: `Invalid argument 0: Sizes of tensors must match except in dimension 0. Got 499 and 442 in dimension 2 at...` ?**

Your images are not all the same size.

Possible solutions:

-  `transforms.RandomCrop(224)`

-  `transforms.RandomResizedCrop(224)`

- `transforms.Resize(224)`

- `transforms.CenterCrop(224)`

**Q4: While using `transforms.Normalize`, we pass in a list of means and a list of standard deviations (std) to normalize the input color channels. How do we define means and std values and why is it 0.5 in some cases?**

Ideally, you should use the mean and standard deviation for each channel. In the case of Imagenet, you use these precalculated values `normalize = transforms.Normalize(mean=[0.485, 0.456, 0.406], std=[0.229, 0.224, 0.225])`. The reason for using 0.5 in the case of mnist is to reduce complexity for the readers. Check Soumith Chintala’s comment [here]( https://discuss.pytorch.org/t/normalization-in-the-mnist-example/457/7).

**Q5: In `weight=torch.from_numpy(filters).unsqueeze(1).type(torch.FloatTensor)` what does `unsqueeze(1)` mean?**

Answered by @Clement:

>One of the reasons why we need to unsqueeze which adds in an additional dimension is due to how PyTorch passes in the CNN parameters.`(N,Cin,H,W)` is the input format to all CNNs defined in PyTorch, where N is the batch size, Cin is the number of channels i.e. `grayscale = 1 and color = 3`, H is the Height of the image, and W is the Width of the image. It is a neat trick to introduce a dimension of size 1 (using unsqueeze) and since if we are dealing with grayscale images where the number of input channels of grayscale is 1, we can just unsqueeze index 1 (which is the second position/dimension) of the tensor. With the help of @Mitch Deoudes for clarification on this particular question: In this particular case, if you look at the __init__() function, the "weight" variable is actually being used to directly set the weights of the conv layer. The weight variable is a (4,1,4,4) matrix defined as [output_channels, input_channels, height_of_filter, width_of_filter] as our input provided to the nn.Conv2d is in the format of [input_channels, output_channels, height_of_filter, width_of_filter]. Remember to multiply 2 matrices we have to ensure that the dimensions of the first column and first row to be matched i.e. (NxM x MxO) etc. Therefore, since we are dealing with grayscale we can use a .unsqueeze(1) to introduce a 1 in the second dimension - yet another neat trick.


