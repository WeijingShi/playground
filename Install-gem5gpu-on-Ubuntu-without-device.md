# A quick start for gem5gpu installation 

First the official [site](https://gem5-gpu.cs.wisc.edu/wiki/Main_Page) offers a real good [Getting Started page](https://gem5-gpu.cs.wisc.edu/wiki/start). If you haven't seen it already, check it out.

Here are some notes from my experience.

**Objective: Running gem5-gpu on Ubuntu without cuda GPU device**

**OS: Ubuntu 14.04** (Ubuntu 16.04 caused some headaches and I am lazy)

**1. Install CUDA.** 

According the [sheet](https://docs.google.com/spreadsheets/d/1dPpw6M7U71SIo94wOY9axlAA6FCt4_I76NW2foZsfi4/edit#gid=3) provided by gem5-gpu team. So far, only CUDA 3.1 or [3.2](https://developer.nvidia.com/cuda-toolkit-32-downloads) is supported. 

Both  CUDA Toolkit and GPU Computing SDK are needed. You can download them [here](https://developer.nvidia.com/cuda-toolkit-32-downloads). Once you download CUDA Toolkit let's install it:

    $sudo sh cudatoolkit_3.2.16_linux_64_ubuntu10.04.run

Set environment (use default CUDA install path /usr/local/) :

    $export CUDAHOME=/usr/local/cuda
    $export PATH=$PATH:$CUDAHOME/bin
    $export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$CUDAHOME/lib64:$CUDAHOME/lib

CUDA 3.2 works well with gcc-4.4 but Ubuntu 14.04 has gcc-4.8 by default. So let's install gcc-4.4

    $sudo apt-get update
    $sudo apt-get install gcc-4.4 g++-4.4

Specify the compilers for nvcc (credits goes to [Nicu Stiurca](http://stackoverflow.com/questions/6622454/cuda-incompatible-with-my-gcc-version) ):

    $sudo ln -s /usr/bin/gcc-4.4 /usr/local/cuda/bin/gcc
    $sudo ln -s /usr/bin/g++-4.4 /usr/local/cuda/bin/g++

**2.Install SDK**

Download Gpu computing sdk example [here](https://developer.nvidia.com/cuda-toolkit-32-downloads). Install it (use default install path ~/):

    $sh gpucomputingsdk_3.2.16_linux.run
    $export NVIDIA_CUDA_SDK_LOCATION=~/NVIDIA_GPU_Computing_SDK/C

**Build SDK (only libcutil.so):**

A complete build requires some library comes from driver such as libcuda.a. Since we don't have CUDA device here, we don't install the driver. It is OK. We just build libcutil.so which is all we need.

    $sudo apt-get install freeglut3-dev libxi-dev
    $cd ~/NVIDIA_GPU_Computing_SDK/C
    $make lib/libcutil.so

 


**Install gem5-gpu**

Before you start to build gem5-gpu, here are some dependencies you may want deal with (credit goes to [DeepSidhu1313](http://askubuntu.com/questions/350475/how-can-i-install-gem5) ):

    $sudo apt-get install swig gcc m4 python python-dev libgoogle-perftools-dev  g++  scons  mercurial  zlib1g-dev protobuf-compiler libprotobuf-dev

And enable mercurial's extension and username, you can do it by edit:

    sudo gedit /etc/mercurial/hgrc

Add lines and save :

    [extensions]
    mq =

    [ui]
    username = XXXX <XXX@XXX.XX>

Now, go ahead and build gem5-gpu following the steps [here](https://gem5-gpu.cs.wisc.edu/wiki/start).

**Try a benchmark**


You may also want to test it out by a benchmark, following the steps [here](https://gem5-gpu.cs.wisc.edu/wiki/benchmarks). 

**Trouble shooting:**
If your benchmark aborts showing something like "fatal: syscall unimplemented". It could be a known [issue](https://groups.google.com/forum/#!searchin/gem5-gpu-dev/syscall$20gettid%7Csort:relevance/gem5-gpu-dev/CkeXDolt9wA/ANEW58lsEAAJ) and use full path to the benchmark binary could solve the problem. 

**Good luck and Have fun :D**
