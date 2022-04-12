# Deploing XGBoost to Triton Inference Server

![](logos.png)

*This demo is adapted from [Real-time Serving for XGBoost, Scikit-Learn RandomForest, LightGBM, and More](https://developer.nvidia.com/blog/real-time-serving-for-xgboost-scikit-learn-randomforest-lightgbm-and-more/).* 

These files describe how to build and predictive model using XGBoost and deploy it to Triton Inference server. If you already have a the CUDA Toolkit and the Container Toolkit installed on NVIDIA GPU Server, you can skip the first two steps.  

1. Choose a GPU server
2. Configure the server
3. Pull the NGC containers
4. Build an XGBoost model
5. Deploy to Triton


### About

This demo was first developed [NVIDIA](https://www.nvidia.com/en-us/) hardware and software on [Azure](https://azure.microsoft.com/en-us/).

[XGBoost](https://www.nvidia.com/en-us/glossary/data-science/xgboost/) is an open-source software library that implements optimized distributed gradient boosting machine learning algorithms under the Gradient Boosting framework.

[NVIDIA Triton Inference Server](https://developer.nvidia.com/nvidia-triton-inference-server) is an open-source inference serving software that helps standardize model deployment and execution and delivers fast and scalable AI in production.

[NVIDIA NGC](https://catalog.ngc.nvidia.com/) hosts a catalog of GPU-optimized software for AI practitioners to develop their AI solutions.

[NVIDIA Container Toolkit](https://github.com/NVIDIA/nvidia-docker) allows users to build and run GPU accelerated Docker containers.

[NVIDIA CUDA Toolkit](https://developer.nvidia.com/cuda-downloads) provides a development environment for creating high performance GPU-accelerated applications.