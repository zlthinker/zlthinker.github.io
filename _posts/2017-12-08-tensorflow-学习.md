---
title: Tensorflow study
updated: 2017-12-08 14:00
category: 
tag: [deep learning]
---

# Document


### Tensorflow building blocks

1. Data tensors `node = tf.constant(3.0, dtype=tf.float32)`
3. Variables `a = tf.placeholder(tf.float32)`
2. Dependent variables `adder_node = tf.add(a, b)`
4. Executive session `sess = tf.Session()`, `sess.run(adder_node, {a: 3, b: 4.5})`
5. Trainable parameters `W = tf.Variable([.3], dtype=tf.float32)`
6. Parameter initialization `init = tf.global_variables_initializer()`, `sess.run(init)`
7. Parameter assignment `fixW = tf.assign(W, [-1.])`, `sess.run(fixW)`
8. Optimizer `optimizer = tf.train.GradientDescentOptimizer(0.01)`
9. Trainer=optimizer+loss `train = optimizer.minimize(loss)`
10. High-level training toolkit `tf.estimator`

### Using GPUs

Device mapping management:
1. `with tf.device('/device/GPU:0'):` Specify computing devices for operators.
2. `sess = tf.Session(config=tf.ConfigProto(log_device_placement=True))
` Output device mapping into log.

GPU memory management:
3. `config = tf.ConfigProto() config.gpu_options.allow_growth = True `
allocate only as much GPU memory based on runtime allocations
4. `config.gpu_options.per_process_gpu_memory_fraction = 0.4` bound the amount of GPU memory available to the TensorFlow process

### Network layers

```python
tf.layers.conv2d()	# conv2d
tf.layers.conv3d()	# conv3d
tf.layers.conv2d_transpose()	# deconv2d
tf.layers.conv3d_transpose()	# deconv3d
tf.layers.max_pooling2d()	# max pooling
tf.layers.average_pooling()	# avg pooling
tf.layers.dense()	# fc
tf.layers.batch_normalization()	# bn
tf.layers.dropout()	# dropout

tf.nn.relu()	# relu
tf.nn.local_response_normalization()	#lrn
tf.nn.softmax()	#softmax
tf.nn.l2_normalization()	# l2norm

tf.concat()	# concat
tf.add_n()	# add
tf.squeeze()	# Removes dimensions of size 1 from the shape of a tensor
tf.maximum(x, y, name)		# element-wise maximum
tf.tanh(x, name)
tf.reshape(tensor, shape, name)
```
