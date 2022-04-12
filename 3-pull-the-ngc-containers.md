# Set up PyTorch and Triton Containers

This step will download and run two docker containers from NGC. We will use the PyTorch container to build a predictive model using XGBoost. Then we will deploy that model to the Triton container. The two containers are networked so that requests from the PyTorch container can be submitted to the Triton server.

### Set up notes

* Make sure you have properly set up your server as described in [Configuring the server]().
* You should have plenty of disk space (at least 64 GB)
* Open parts 8000, 8001, 8002 for Triton
* Open port 8888 for Jupyter Lab

## Set up docker network

Create a docker network for the containers to communicate with each other.

```
sudo docker network create tritonnet
sudo docker volume create volume1
```

## Pull and run docker containers

Pull the contains are run them within the network. Mount a volume on the PyTorch container. This volume will be used to save models that will later be transfered to the Triton model repository.

```
# PyTorch
sudo docker pull nvcr.io/nvidia/pytorch:22.03-py3
sudo docker run --gpus=all -t -d -p 8888:8888 --ipc=host --ulimit memlock=-1 --ulimit stack=67108864 --network=tritonnet \
    --mount source=volume1,destination=/workspace/volume1 --name pytorch nvcr.io/nvidia/pytorch:22.03-py3

# Triton
sudo docker pull nvcr.io/nvidia/tritonserver:22.03-py3
sudo docker run --gpus=all -d -p 8000:8000 -p 8001:8001 -p 8002:8002 --network=tritonnet \
    -v /var/lib/docker/volumes/volume1/_data/model_repository:/models \
    --name tritonserver nvcr.io/nvidia/tritonserver:22.03-py3 tritonserver --model-repository=/models
```

## Check the Triton logs

Use this command to check for saved models in the model repository. Note: When you first set up Triton, there will be no saved models in the model repository.

```
sudo docker logs tritonserver
```

## Identify Triton IP

Make a note of the IP of the Triton server for the later on.

```
sudo docker network inspect tritonnet
```

## Start Jupyter Lab

Jupyter Lab is preinstalled on the PyTorch container. You can run it in headless mode on port 8888. Note that the code below removes the token. If you want to use a security token when you log into the server, you can remove the comment `NotebeookApp.token=''`.

```
sudo docker exec -it pytorch /bin/bash
nohup jupyter-lab --NotebookApp.token='' --no-browser --port=8888 &
exit

```

## Open Jupyter Lab

Access Jupyter Lab at `http://<server-ip>:8888/lab?`. Using a terminal in Jupyter Lab, clone this repository: `git clone https://github.com/nwstephens/triton-xgboost.git`. Then open the scripts for XGBoost and Triton.

* [4-build-an-xgboost-model.ipynb](4-build-an-xgboost-model.ipynb)
* [5-deploy-to-triton.ipynb](5-deploy-to-triton.ipynb)
