---
title: 'Pytorch Tutorial'
date: 2017-08-25
permalink: /posts/2017/08/pytorch-tutorial/
tags:
  - Deep Learning
  - Pytorch
  - python
---
# Pytorch Tutorial for Practitioners

The example here is motivated from pytorch examples. Please have a look at github/pytorch to know more.


Summary of steps:
1. Setup transformations for the data to be loaded. 
2. Create dataloader from datasets.
3. Setup network to train.
4. In train phase, set network for training
5. Compute forward pass and output prediction
6. Compute loss and accuracy
7. In train phase do the backprop and update weights of the network.
8. In test phase, compute forward pass and compute precision.

```python
import torch
import torch.nn as nn
from torch.autograd import Variable
import torch.nn.functional as F
import torchvision.models as models
import torchvision.datasets as dset
import torchvision.transforms as transforms
```



Before Loading a dataset, it is better to define what kind of transformation will be applied on each image in dataset.

Here, we perform randomized cropping of image, random horizontal flip , normalize an image.

```python
# example transformation, change as per your task.
transformations = transforms.Compose([transforms.RandomSizedCrop(256),
                                      transforms.RandomHorizontalFlip(),
                                      transforms.ToTensor(),
                                      transforms.Normalize(mean = [0.485, 0.456, 0.406],
                                                           std = [0.229, 0.224, 0.225])])
```



Now go ahead and initialize a dataset and then a dataloader

```python
# datasets
train_cifar10 = dset.CIFAR10('./datasets/', train=True, transform=transformations, download=True )
test_cifar10 = dset.CIFAR10('./datasets/', train=False, transform=transformations, download=True )
# data loader
train_loader = torch.utils.data.DataLoader(train_cifar10, batch_size=128, shuffle=True)
test_loader = torch.utils.data.DataLoader(test_cifar10,batch_size= 128,shuffle=True )
```



Define a model before setting up an optimizer

```python
net = models.resnet18() # pytorch has pre-defined model structure, that can be directly loaded.
net.cuda() # comment this for cpu only
```



Let's setup an optimizer for training

```python
# this takes input model parameters, pass only which you would like to train
optimizer = optim.SGD(net.parameters(), lr=0.002, momentum=0.9, weight_decay=1e-5)
```



Loss function can be defined as

```python
criterion = nn.CrossEntropyLoss().cuda()
# uncomment below for cpu version only
# criterion = nn.CrossEntropyLoss()
```



Setup the model defined above for training.

```python
# first step set network to train value
net.train()
```


Load a batch and create variable object for the given batch

```python
# While iterating over the dataset do training
for idx, (data, targets) in enumerate(train_loader):
  data, targets = data.cuda(), targets.cuda() # comment for cpu version
  data, targets = Variable(data), Variable(targets) # create variables for each
  ....

```



Compute forward pass along the network

```python
# While iterating over the dataset do training
for idx, (data, targets) in enumerate(train_loader):
  data, targets = data.cuda(), targets.cuda() # comment for cpu version
  data, targets = Variable(data), Variable(targets) # create variables for each

  output = net.forward(data) # forward pass
  ...
```



Once we have outputs, we can compute losses for the batch

```python
# While iterating over the dataset do training
for idx, (data, targets) in enumerate(train_loader):
  data, targets = data.cuda(), targets.cuda() # comment for cpu version
  data, targets = Variable(data), Variable(targets) # create variables for each

  output = net.forward(data) # forward pass

  loss = criterion(output, targets) # loss
```



After getting outputs, we can have precision  values also.

```python
# While iterating over the dataset do training
for idx, (data, targets) in enumerate(train_loader):
  data, targets = data.cuda(), targets.cuda() # comment for cpu version
  data, targets = Variable(data), Variable(targets) # create variables for each

  output = net.forward(data) # forward pass

  loss = criterion(output, targets) # loss

  # implement this as per the task
  precision = compute_accuracy(output.cpu().data, target.cpu().data)

```



Let's do the backprop and update weights

```python
# While iterating over the dataset do training
for idx, (data, targets) in enumerate(train_loader):
  data, targets = data.cuda(), targets.cuda() # comment for cpu version
  data, targets = Variable(data), Variable(targets) # create variables for each

  output = net.forward(data) # forward pass

  loss = criterion(output, targets) # loss

  # implement this as per the task
  precision = compute_accuracy(output.cpu().data, target.cpu().data)

  # set zero to previous gradients in optimizer otherwise the weights will be updated according to old values
  optimizer.zero_grad()

  # run back prop and compute new gradients
  loss.backward()

  # update the parameters defined in optimizer before
  optimizer.step()
```



That's all in training. But we would like to do validation also.

During validation, there are only a few steps different than training. Firstly, set the model for evaluation phase and load dataset from `test_loader` instead of `train_loader`.

```python
net.eval()
# While iterating over the dataset do training
for idx, (data, targets) in enumerate(test_loader):
  data, targets = data.cuda(), targets.cuda() # comment for cpu version
  data, targets = Variable(data), Variable(targets) # create variables for each

  output = net.forward(data) # forward pass

  loss = criterion(output, targets) # loss

  # implement this as per the task
  precision = compute_accuracy(output.cpu().data, target.cpu().data)
  print(precision) # this is per-batch precision. Average over all batches to get overall precision.

```



Some FAQs.

1. For training on GPU, if you are getting error for gpu, first check if the data is accessed is on the same device as that of function. In above, tutorial we set loss as `criterion = nn.CrossEntropyLoss().cuda()` . This will be executed on gpu and the returned value will be on GPU only, so to read or print this first bring the data on `cpu` as `print(loss.cpu())` .
2. Batch size for the data is set in `train_loader`  and `test_loader`. You can set different batch sizes for train and validation.  
