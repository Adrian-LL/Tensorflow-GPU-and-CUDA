# Tensorflow-GPU-and-CUDA
Short procedure to install tensorflow-gpu on my system

####Updated 2019-05-01

## From here: https://www.tensorflow.org/install/gpu#hardware_requirements
1. Check graphic card compatibility - here: https://developer.nvidia.com/cuda-gpus (checked)
2. Download and install CUDA Toolkit - here: https://developer.nvidia.com/cuda-toolkit-archive
* NOTES
  * installed version 10.0 - at the document date, CUDA 10.1 is already available. Not tested yet.
  * CUPTI in installed witd CUDA
  * Did not have Visual Studio, just check the "I understand" and go without VS
  * Custom installation:
    * uncheck all except CUDA (i.e. I already had the latest drivers etc.)
3. Install cuDNN SDK (libraries) - here: https://developer.nvidia.com/cudnn

By unzippinng in a folder, e.g.```c:\cuda```
* NOTES:
 * Developer account is needed for download - done
 * Install the cuDNN that works with the current version of CUDA (that is cuDNN 7.5.1)
 


