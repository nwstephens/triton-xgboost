# Configuring the server

These instructions explain how to configure a server in preparation for doing data science on a GPU with NGC containers. NVIDIA NGC contains GPU-optimized AI software, enterprise services and support. You can pull containers from the [NGC Catalog](https://catalog.ngc.nvidia.com/) that will help you get started with data science projects. 

## Getting started

All of the software and hardware on your system needs to be compatible. Please note that the following components should be compatible with each other:

1. The GPU [compute capability](https://developer.nvidia.com/cuda-gpus) should be compatible with the NGC container you run.
2. The [NGC container](https://catalog.ngc.nvidia.com/) you run should be compatible with the CUDA toolkit you run.
3. The [CUDA toolkit](https://developer.nvidia.com/cuda-downloads) you run should be compatible with the operating system you run.

If you haven't already, procure a GPU with the required compute capability as described in [Where to find NVIDIA GPU's for data data science](1-choose-a-gpu-server.md).

## Install the CUDA toolkit

```
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/cuda-ubuntu2004.pin
sudo mv cuda-ubuntu2004.pin /etc/apt/preferences.d/cuda-repository-pin-600
wget https://developer.download.nvidia.com/compute/cuda/11.6.2/local_installers/cuda-repo-ubuntu2004-11-6-local_11.6.2-510.47.03-1_amd64.deb
sudo dpkg -i cuda-repo-ubuntu2004-11-6-local_11.6.2-510.47.03-1_amd64.deb
sudo apt-key add /var/cuda-repo-ubuntu2004-11-6-local/7fa2af80.pub
sudo apt-get update
sudo apt-get -y install cuda
```

```
nvidia-smi
```

## Set CUDA Home

You can add CUDA to your home path by altering your bash file. First open the file with `vim ~/.bashrc`,add the lines below, close the file, and then source it with `source ~/.bashrc`.

```
export CUDA_HOME=/usr/local/cuda
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda/lib64:/usr/local/cuda/extras/CUPTI/lib64
export PATH=$PATH:$CUDA_HOME/bin
```

```
nvcc --version
```

## Install the container toolkit

```
curl https://get.docker.com | sh \
  && sudo systemctl --now enable docker
distribution=$(. /etc/os-release;echo $ID$VERSION_ID) \
      && curl -s -L https://nvidia.github.io/libnvidia-container/gpgkey | sudo apt-key add - \
      && curl -s -L https://nvidia.github.io/libnvidia-container/$distribution/libnvidia-container.list | sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list
sudo apt-get update
sudo apt-get install -y nvidia-docker2
sudo systemctl restart docker
```

```
sudo docker run hello-world
```
