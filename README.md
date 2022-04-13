# Deploing XGBoost Models to Triton Inference Server

![](logos.png)

This demo is adapted from the blog post [Real-time Serving for XGBoost, Scikit-Learn RandomForest, LightGBM, and More](https://developer.nvidia.com/blog/real-time-serving-for-xgboost-scikit-learn-randomforest-lightgbm-and-more/) and describes how to build and deploy predictive models using [XGBoost](https://www.nvidia.com/en-us/glossary/data-science/xgboost/) and the [Triton Inference Server](https://developer.nvidia.com/nvidia-triton-inference-server) on GPU acclerated servers. 

**Important: Make sure you configured a GPU compatible server first. For more information see:**

* [Choose a GPU Server](docs/choose-a-gpu-server.md) 
* [Configure the Server](docs/configure-the-server.md) 

## Set up PyTorch and Triton Containers

This step will download and run two docker containers from NGC. We will use the PyTorch container to build a predictive model using XGBoost. Then we will deploy that model to the Triton container. The two containers are networked so that requests from the PyTorch container can be submitted to the Triton server.

### Set up notes

* Make sure you have properly set up your server.
* You should have plenty of disk space (at least 64 GB)
* Open parts 8000, 8001, 8002 for Triton
* Open port 8888 for Jupyter Lab

## Set up docker network

Create a docker network for the containers to communicate with each other.

```
sudo docker network create tritonnet
sudo docker volume create volume1
```

## Pull and run the Triton container

```
# Triton
sudo docker pull nvcr.io/nvidia/tritonserver:22.03-py3
sudo docker run --gpus=all -d -p 8000:8000 -p 8001:8001 -p 8002:8002 --network=tritonnet \
    -v /var/lib/docker/volumes/volume1/_data/model_repository:/models \
    --name tritonserver nvcr.io/nvidia/tritonserver:22.03-py3 tritonserver --model-repository=/models
```

Copy saved model into the model repository.

```
cp <saved_model>
cp <config>
```

Make a note of the IP of the Triton server for the later on.

```
sudo docker network inspect tritonnet
```

Check the Triton logs.

```
sudo docker logs tritonserver
```


## Pull and run the PyTorch container

```
# PyTorch
sudo docker pull nvcr.io/nvidia/pytorch:22.03-py3
sudo docker run --gpus=all -t -d -p 8888:8888 --ipc=host --ulimit memlock=-1 --ulimit stack=67108864 --network=tritonnet \
    --mount source=volume1,destination=/workspace/volume1 --name pytorch nvcr.io/nvidia/pytorch:22.03-py3
```

Open Jupyter Lab. Jupyter Lab is preinstalled on the PyTorch container. You can run it in headless mode on port 8888. Note that the code below removes the token. If you want to use a security token when you log into the server, you can remove the comment `NotebeookApp.token=''`.

```
sudo docker exec -it pytorch /bin/bash
nohup jupyter-lab --NotebookApp.token='' --no-browser --port=8888 &
exit

```

Clone this repository

```
sudo docker exec -it pytorch /bin/bash
git clone https://github.com/nwstephens/triton-xgboost.git
exit

```

Access Jupyter Lab at `http://<server-ip>:8888/lab?`. Open the scripts for XGBoost and Triton.
