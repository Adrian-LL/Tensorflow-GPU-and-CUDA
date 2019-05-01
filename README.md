# Tensorflow-GPU-and-CUDA Windows 10
Short procedure to install tensorflow-gpu on my system

#### Updated 2019-05-01

## I. CUDA installation
### From here: https://www.tensorflow.org/install/gpu#hardware_requirements
1. Check graphic card compatibility - here: https://developer.nvidia.com/cuda-gpus (checked)
2. Download and install CUDA Toolkit - here: https://developer.nvidia.com/cuda-toolkit-archive
* NOTES
  * installed version 10.0 - at the document date, CUDA 10.1 is already available. Not tested yet.
  * CUPTI in installed witd CUDA
  * Did not have Visual Studio, just check the "I understand" and go without VS
  * Custom installation:
    * uncheck all except CUDA (i.e. I already had the latest drivers etc.)
3. Install cuDNN SDK (libraries) - here: https://developer.nvidia.com/cudnn

By unzippinng in a folder, e.g.```c:\cuda``` (note that this should be rather named ```cuDNN```)
* NOTES:
 * Developer account is needed for download - done
 * Install the cuDNN that works with the current version of CUDA (that is cuDNN 7.5.1)
 
4. Add paths to environment via the following commands (or via Start -> Edit the system environment variables)
```cmd
C:\> SET PATH=C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v10.0\bin;%PATH%
C:\> SET PATH=C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v10.0\extras\CUPTI\libx64;%PATH%
C:\> SET PATH=C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v10.0\include;%PATH%
C:\> SET PATH=C:\cuda\bin;%PATH%

```
* NOTES
 * the installation add automatically, I manually added ```libvpn``` and the ```cuda``` one,
 ```
 C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v10.0\bin
 C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v10.0\libnvvp
```

5. Do a ```refreshenv``` if you are in CMD.

## II. Tensorflow installation
I use ```pipenv``` so I did:
```cmd
> pipenv shell
> pipenv install tensorflow-gpu
```

## III. Test installation
```cmd
> cd LEARNING-2019
> pipenv shell
(LEARNING-2019-MehCbHCx) λ
(LEARNING-2019-MehCbHCx) λ ipython
Python 3.7.3 (v3.7.3:ef4ec6ed12, Mar 25 2019, 22:22:05) [MSC v.1916 64 bit (AMD64)]
Type 'copyright', 'credits' or 'license' for more information
IPython 7.5.0 -- An enhanced Interactive Python. Type '?' for help.

In [1]: import tensorflow as tf
```
> NOTE - if something is wrong, the first errors should appear here.

```cmd
In [2]: tf.test.is_gpu_available(
   ...:     cuda_only=False,
   ...:     min_cuda_compute_capability=None
   ...: )
2019-05-01 17:33:13.621082: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1433] Found device 0 with properties:
name: GeForce GTX 1060 6GB major: 6 minor: 1 memoryClockRate(GHz): 1.759
pciBusID: 0000:05:00.0
totalMemory: 6.00GiB freeMemory: 4.97GiB
2019-05-01 17:33:13.658909: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1512] Adding visible gpu devices: 0
2019-05-01 17:33:14.760843: I tensorflow/core/common_runtime/gpu/gpu_device.cc:984] Device interconnect StreamExecutor with strength 1 edge matrix: 2019-05-01 17:33:14.781144: I tensorflow/core/common_runtime/gpu/gpu_device.cc:990]      0
2019-05-01 17:33:14.792089: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1003] 0:   N
2019-05-01 17:33:14.804979: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1115] Created TensorFlow device (/device:GPU:0 with 4716 MB memory) -> physical GPU (device: 0, name: GeForce GTX 1060 6GB, pci bus id: 0000:05:00.0, compute capability: 6.1)
Out[2]: True
```
Other variant (after importing tensorflow)

```cmd
In [3]: # Creates a graph.
   ...: a = tf.constant([1.0, 2.0, 3.0, 4.0, 5.0, 6.0], shape=[2, 3], name='a')
   ...: b = tf.constant([1.0, 2.0, 3.0, 4.0, 5.0, 6.0], shape=[3, 2], name='b')
   ...: c = tf.matmul(a, b)
   ...: # Creates a session with log_device_placement set to True.
   ...: sess = tf.Session(config=tf.ConfigProto(log_device_placement=True))
   ...: # Runs the op.
   ...: print(sess.run(c))
2019-05-01 17:35:15.844599: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1512] Adding visible gpu devices: 0
2019-05-01 17:35:15.860375: I tensorflow/core/common_runtime/gpu/gpu_device.cc:984] Device interconnect StreamExecutor with strength 1 edge matrix: 2019-05-01 17:35:15.881228: I tensorflow/core/common_runtime/gpu/gpu_device.cc:990]      0
2019-05-01 17:35:15.892899: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1003] 0:   N
2019-05-01 17:35:15.906267: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1115] Created TensorFlow device (/job:localhost/replica:0/task:0/device:GPU:0 with 4716 MB memory) -> physical GPU (device: 0, name: GeForce GTX 1060 6GB, pci bus id: 0000:05:00.0, compute capability: 6.1)
Device mapping:
/job:localhost/replica:0/task:0/device:GPU:0 -> device: 0, name: GeForce GTX 1060 6GB, pci bus id: 0000:05:00.0, compute capability: 6.1
2019-05-01 17:35:15.958145: I tensorflow/core/common_runtime/direct_session.cc:317] Device mapping:
/job:localhost/replica:0/task:0/device:GPU:0 -> device: 0, name: GeForce GTX 1060 6GB, pci bus id: 0000:05:00.0, compute capability: 6.1

MatMul: (MatMul): /job:localhost/replica:0/task:0/device:GPU:0
2019-05-01 17:35:15.992063: I tensorflow/core/common_runtime/placer.cc:1059] MatMul: (MatMul)/job:localhost/replica:0/task:0/device:GPU:0
a: (Const): /job:localhost/replica:0/task:0/device:GPU:0
2019-05-01 17:35:16.014781: I tensorflow/core/common_runtime/placer.cc:1059] a: (Const)/job:localhost/replica:0/task:0/device:GPU:0
b: (Const): /job:localhost/replica:0/task:0/device:GPU:0
2019-05-01 17:35:16.033393: I tensorflow/core/common_runtime/placer.cc:1059] b: (Const)/job:localhost/replica:0/task:0/device:GPU:0
[[22. 28.]
 [49. 64.]]
```
And another one (that allocates video memory)

```cmd

In [4]: tf.__version__sess = tf.Session(config=tf.ConfigProto(log_device_placement=True))
2019-05-01 17:38:30.125190: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1512] Adding visible gpu devices: 0
2019-05-01 17:38:30.140031: I tensorflow/core/common_runtime/gpu/gpu_device.cc:984] Device interconnect StreamExecutor with strength 1 edge matrix: 2019-05-01 17:38:30.160869: I tensorflow/core/common_runtime/gpu/gpu_device.cc:990]      0
2019-05-01 17:38:30.172891: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1003] 0:   N
2019-05-01 17:38:30.185328: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1115] Created TensorFlow device (/job:localhost/replica:0/task:0/device:GPU:0 with 4716 MB memory) -> physical GPU (device: 0, name: GeForce GTX 1060 6GB, pci bus id: 0000:05:00.0, compute capability: 6.1)
Device mapping:
/job:localhost/replica:0/task:0/device:GPU:0 -> device: 0, name: GeForce GTX 1060 6GB, pci bus id: 0000:05:00.0, compute capability: 6.1
2019-05-01 17:38:30.225604: I tensorflow/core/common_runtime/direct_session.cc:317] Device mapping:
/job:localhost/replica:0/task:0/device:GPU:0 -> device: 0, name: GeForce GTX 1060 6GB, pci bus id: 0000:05:00.0, compute capability: 6.1
```
