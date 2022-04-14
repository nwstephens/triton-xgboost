# Deploing XGBoost Models to Triton Inference Server

![](logos.png)

This demo is adapted from the blog post [Real-time Serving for XGBoost, Scikit-Learn RandomForest, LightGBM, and More](https://developer.nvidia.com/blog/real-time-serving-for-xgboost-scikit-learn-randomforest-lightgbm-and-more/) and describes how to build and deploy predictive models using [XGBoost](https://www.nvidia.com/en-us/glossary/data-science/xgboost/) and the [Triton Inference Server](https://developer.nvidia.com/nvidia-triton-inference-server) on GPU accelerated servers. 

## Set up PyTorch and Triton Containers

This step will download and run two docker containers from NGC. We will use the PyTorch container to build a predictive model using XGBoost and the Triton container for model deployment. The two containers are networked so that requests from the PyTorch container can be submitted to the Triton server.

These notebooks were tested on the following configuration:

* NVIDIA Tesla V100 (running on Azure)
* Ubuntu 20.04 running on Linux x86
* 64 GB of disk space
* Open ports: 22; 8888; 8000-8002
* CUDA Toolkit 11.6
* NVIDIA Container Toolkit 1.9.0
* NGC PyTorch 22.03
* NGC Triton Server 22.03

## Requirements

* Make sure you have properly set up your server. See the [docs](docs/README.md) for details.
* You should have plenty of disk space.
* Open ports: 8888; 8000-8002.

## Docker network

Create a docker network for the containers to communicate with each other, and a shared volume that will be used by the [model repository](https://github.com/triton-inference-server/server/blob/main/docs/model_repository.md).

```
sudo docker network create tritonnet
sudo docker volume create volume1
```

## PyTorch container

The [PyTorch container](https://catalog.ngc.nvidia.com/orgs/nvidia/containers/pytorch) (v22.03) on NGC has many pre-built libraries that makes doing data science easy. Mount the shared volume so you can save models to the model repository.

```
sudo docker pull nvcr.io/nvidia/pytorch:22.03-py3
sudo docker run --gpus=all -t -d -p 8888:8888 --ipc=host --ulimit memlock=-1 --ulimit stack=67108864 --network=tritonnet \
    --mount source=volume1,destination=/workspace/volume1 --name pytorch nvcr.io/nvidia/pytorch:22.03-py3
```

Clone this repository onto the server so you will have easy access to the Jupyter Notebooks. Copy the pre-built model into the model repository.

```
sudo docker exec -it pytorch /bin/bash
git clone https://github.com/nwstephens/triton-xgboost.git
cp -R triton-xgboost/data/pre-built/ volume1/model_repository/.
exit

```

## Triton container

The [Triton container](https://catalog.ngc.nvidia.com/orgs/nvidia/containers/tritonserver) (v22.03) on NGC will serve models in the model repository.

```
sudo docker pull nvcr.io/nvidia/tritonserver:22.03-py3
sudo docker run --gpus=all -d -p 8000:8000 -p 8001:8001 -p 8002:8002 --network=tritonnet \
    -v /var/lib/docker/volumes/volume1/_data/model_repository:/models \
    --name tritonserver nvcr.io/nvidia/tritonserver:22.03-py3 tritonserver --model-repository=/models
```

Verify Triton is running correctly. The HTTP request returns status 200 if Triton is ready and non-200 if it is not ready. 

```
curl -v localhost:8000/v2/health/ready
```

Check the Triton logs. You should see the pre-built model listed in the model repository. If your model is not displayed in the table check the path to the model repository and your CUDA drivers.

```
sudo docker logs tritonserver
```

Make a note of the IP of the Triton server. You will need to add the IP to the Triton client later configuration later on. 

```
sudo docker network inspect tritonnet
```

## Starting containers after a server restart

If you shut down your server instance to save costs, you can start containers when you bring your server instance back online.

```
sudo docker start pytorch
sudo docker start tritonserver
```

## Open Jupyter Lab

Jupyter Lab is pre-installed on the PyTorch container. Note that the code below removes the security token. If you want to use a security token when you log into the server, you can remove the comment `--NotebeookApp.token=''`.

```
sudo docker exec -it pytorch /bin/bash
nohup jupyter-lab --NotebookApp.token='' --no-browser --port=8888 &
curl -s api.ipify.org | awk '{print $1":8888"}'
exit

```

Access Jupyter Lab at `http://<server-ip>:8888`. Make sure port 8888 is open. Open the [XGBoost](1-xgboost-model.ipynb) notebook and follow the instructions for building and deploying a model to Triton. Then open the [Triton](2-triton-deploy.ipynb) notebook and follow the instructions for submitting requests to Triton.
