# Deploing XGBoost Models to Triton Inference Server

![](logos.png)

### Overview

These files describe how to build and deploy predictive models using [XGBoost](https://www.nvidia.com/en-us/glossary/data-science/xgboost/) and the [Triton Inference Server](https://developer.nvidia.com/nvidia-triton-inference-server) on GPU acclerated servers. The first two files describe how to select and configure the server. The last three steps describe how to pull the containers, run the analyses, and deploy the models.

1. [Choose a GPU server](1-choose-a-gpu-server.md)
2. [Configure the server](2-configure-the-server.md)
3. [Pull the NGC containers](3-pull-the-ngc-containers.md)
4. [Build an XGBoost model](4-build-an-xgbost-model.ipynb)
5. [Deploy to Triton](5-deploy-to-triton.ipynb)

### About

This demo is adapted from the blog post [Real-time Serving for XGBoost, Scikit-Learn RandomForest, LightGBM, and More](https://developer.nvidia.com/blog/real-time-serving-for-xgboost-scikit-learn-randomforest-lightgbm-and-more/), and depends on the following [NVIDIA](https://www.nvidia.com/en-us/) hardware and software.

* [NVIDIA CUDA Toolkit](https://developer.nvidia.com/cuda-downloads) provides a development environment for creating high performance GPU-accelerated applications.
* [NVIDIA Container Toolkit](https://github.com/NVIDIA/nvidia-docker) allows users to build and run GPU accelerated Docker containers.
* [NVIDIA NGC](https://catalog.ngc.nvidia.com/) hosts a catalog of GPU-optimized software for AI practitioners to develop their AI solutions.
* [NVIDIA Triton Inference Server](https://developer.nvidia.com/nvidia-triton-inference-server) is an open-source inference serving software that helps standardize model deployment and execution and delivers fast and scalable AI in production.
* [XGBoost](https://www.nvidia.com/en-us/glossary/data-science/xgboost/) is an open-source software library that implements optimized distributed gradient boosting machine learning algorithms under the Gradient Boosting framework. The NVIDIA RAPIDS team works closely with the Distributed Machine Learning Common (DMLC) XGBoost organization to provide seamless, drop-in GPU acceleration.

### Troubleshooting

For help with common issues see [Troubleshooting](resources/troubleshooting.md).
