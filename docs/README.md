# Deploing XGBoost Models to Triton Inference Server

This demo is adapted from the blog post [Real-time Serving for XGBoost, Scikit-Learn RandomForest, LightGBM, and More](https://developer.nvidia.com/blog/real-time-serving-for-xgboost-scikit-learn-randomforest-lightgbm-and-more/).

* [Choose a GPU server](choose-a-gpu-server.md)
* [Configure the server](configure-the-server.md)

### About

This demo depends on the following [NVIDIA](https://www.nvidia.com/en-us/) hardware and software.

* [NVIDIA CUDA Toolkit](https://developer.nvidia.com/cuda-downloads) provides a development environment for creating high performance GPU-accelerated applications.
* [NVIDIA Container Toolkit](https://github.com/NVIDIA/nvidia-docker) allows users to build and run GPU accelerated Docker containers.
* [NVIDIA NGC](https://catalog.ngc.nvidia.com/) hosts a catalog of GPU-optimized software for AI practitioners to develop their AI solutions.
* [NVIDIA Triton Inference Server](https://developer.nvidia.com/nvidia-triton-inference-server) is an open-source inference serving software that helps standardize model deployment and execution and delivers fast and scalable AI in production.
* [XGBoost](https://www.nvidia.com/en-us/glossary/data-science/xgboost/) is an open-source software library that implements optimized distributed gradient boosting machine learning algorithms under the Gradient Boosting framework. The NVIDIA RAPIDS team works closely with the Distributed Machine Learning Common (DMLC) XGBoost organization to provide seamless, drop-in GPU acceleration.

### Details

This demo is based on the following configuration:

* NVIDIA Tesla V100 (running on Azure)
* Ubuntu 20.04 running on Linux x86
* 64 GB of disk space
* Open ports: 22; 8888; 8000-8002
* CUDA Toolkit 11.6
* NVIDIA Container Toolkit 1.9.0
* NGC PyTorch 22.03
* NGC Triton Server 22.03

### Troubleshooting

For help with common issues see [Troubleshooting](resources/troubleshooting.md).
