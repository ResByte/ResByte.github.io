---
title: 'Tensorflow-Densenet'
date: 2017-05-21
permalink: /posts/2017/05/tf-densenet/
tags:
  - Deep Learning
  - Tensorflow
  - python
---

# Densenet 
arxiv: [Paper](https://arxiv.org/abs/1608.06993)
![fig](https://d2mxuefqeaa7sj.cloudfront.net/s_8C67F3B1EB0F5ABE74BEB3BC706769E48F9A2C9CE88212D04817755A2125A582_1495365425569_image.png)

```python
import tensorflow as tf
import tensorflow.contrib.slim as slim

def conv_layer(input, filters,kernel_size,stride=1, layer_name="conv"):
    with tf.name_scope(layer_name):
        net = slim.conv2d(input, filters, kernel_size, scope=layer_name)
        return net

class DenseNet():
    def __init__(self,x,nb_blocks, filters, sess):
        self.nb_blocks = nb_blocks
        self.filters = filters
        self.model = self.build_model(x)
        self.sess = sess
    
    def bottleneck_layer(self,x, scope):
        # [BN --> ReLU --> conv11 --> BN --> ReLU -->conv33]
        with tf.name_scope(scope):
            x = slim.batch_norm(x)
            x = tf.nn.relu(x)
            x = conv_layer(x,self.filters,kernel_size=(1,1), layer_name=scope+'_conv1')
            x = slim.batch_norm(x)
            x = tf.nn.relu(x)
            x = conv_layer(x,self.filters,kernel_size=(3,3), layer_name=scope+'_conv2')
            return x 
    def transition_layer(self,x, scope):
        # [BN --> conv11 --> avg_pool2]
        with tf.name_scope(scope):
            x = slim.batch_norm(x)
            x = conv_layer(x,self.filters,kernel_size=(1,1), layer_name=scope+'_conv1')
            x = slim.avg_pool2d(x,2)
            return x 
    
    def dense_block(self,input_x, nb_layers, layer_name):
        with tf.name_scope(layer_name):
            layers_concat = []
            layers_concat.append(input_x)
            x = self.bottleneck_layer(input_x,layer_name +'_bottleN_'+str(0))
            layers_concat.append(x)
            for i in xrange(nb_layers):
                x = tf.concat(layers_concat,axis=3)
                x = self.bottleneck_layer(x,layer_name+'_bottleN_'+str(i+1))
                layers_concat.append(x)
            return x
        
    
    def build_model(self,input_x):
        x = conv_layer(input_x,self.filters,kernel_size=(7,7), layer_name='conv0')
        x = slim.max_pool2d(x,(3,3))        
        for i in xrange(self.nb_blocks):
            print(i)
            x = self.dense_block(x,4, 'dense_'+str(i))
            x = self.transition_layer(x,'trans_'+str(i))        
        return x
```
