How tf allocate different ops and variables to devices? Basically, there are three principles (see [tensorflow tutorial][link:tfgpututorial] for full reference):

1. If devices are set manually using `with tf.device('/cpu:0')`, the ops and variables within the scope will be on the device you define.
2. If no explicit assignment is defined, tf will automatically do the allocation. Note that GPU has higher priority, which means that if an op has an GPU implementation, GPU will be used.
3. If you explicitly assign a device while it doesn't exist or the op has no corresponding implementation, exceptions will be raised. You can set `allow_soft_placement` to `True` to avoid such error.

You can use `log_device_placement` to log the placement. You can use `timeline` object to track the runtime of each op in the graph ([example][link:tftimeline]).

From [this thread][link:tfw2vissue] we know that due to some complex reason, ops involving integer (word2vec) can't be implemented on GPU in the near future.
From [this thread][link:tfopdevice] we know that it is hard to know which backend each op supports, because the kernel is implemented and registered in C++ which is hard to be exposed to python.

[link:tfgpututorial]: https://www.tensorflow.org/tutorials/using_gpu
[link:tftimeline]: https://stackoverflow.com/questions/37751739/tensorflow-code-optimization-strategy/
[link:tfw2vissue]: https://github.com/tensorflow/tensorflow/issues/2849
[link:tfopdevice]: https://github.com/tensorflow/tensorflow/issues/2502