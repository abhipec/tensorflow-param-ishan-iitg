# tensorflow-param-ishan-iitg
Steps required to set up Tensorflow on a computing cluster (Param-Ishan) available at IITG.

As of now only Tensorflow 0.10 version will work, because later version requires cuda-8 support, which is not yet working on Param-Ishan.

GPU login node IP is 172.17.0.8, login directly to this node by its IP. 

## Requirements: 
1. Cuda 7.5 with cuDNN
2. Python3 virtualenv (Not necessary, but this will make installation and management easier).

## Steps:
1. create a copy of Cuda toolkit to the home directory.
```bash
  mkdir cuda75
  cp -r /cm/shared/apps/cuda80/toolkit/7.5.18/* cuda75/
```
2. Download and install [cuDNN v5 Library for Linux for Cuda 7.5](https://developer.nvidia.com/rdp/cudnn-download)
```bash
  cd
  tar xvf cudnn-7.5-linux-x64-v5.0-ga.tgz
  mv cuda/include/cudnn.h cuda75/include/
  mv cuda/lib64/* cuda75/lib64/
  rm -rf cuda
```

3. Prepare .bashrc, here is the complete content of the .bashrc file that will work.
```bash
# .bashrc

# Source global definitions
if [ -f /etc/bashrc ]; then
        . /etc/bashrc
fi

# User specific aliases and functions
module load gcc/4.8.2
module load python/cdac/3.5.2
module load slurm

export http_proxy=http://username:password@202.10.01.10:3128/
export https_proxy=$http_proxy
export LC_COLLATE=C
export LC_ALL=C

# CUDA settings
export CUDA_ROOT=~/cuda75
export CUDA_HOME=~/cuda75
export PATH=${CUDA_HOME}/bin:$PATH
export LD_LIBRARY_PATH=${CUDA_HOME}/lib64:$LD_LIBRARY_PATH
export LD_LIBRARY_PATH=/cm/shared/apps/glibc_2.14:$LD_LIBRARY_PATH
```
4. Logout and login again to make sure correct modules are loaded.

5. Install virtualenv and Tensorflow 
```bash
pip3 install --upgrade pip --user
.local/bin/pip3 install virtualenv --user
.local/bin/virtualenv p3_env
source p3_env/bin/activate
pip install cython
export
TF_BINARY_URL=https://storage.googleapis.com/tensorflow/linux/gpu/tensorflow-0.10.0-cp35-cp35m-linux_x86_64.whl

pip install $TF_BINARY_URL
pip install ipython
```

6. That's it, it will work!!. (Quick test)
```bash
source p3_env/bin/activate
ipython
import tensorflow as tf
sess = tf.Session()
sess.run(tf.constant(100))
```
