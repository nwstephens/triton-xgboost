# Deploing XGBoost Models to Triton Inference Server

![](logos.png)

This demo is adapted from the blog post [Real-time Serving for XGBoost, Scikit-Learn RandomForest, LightGBM, and More](https://developer.nvidia.com/blog/real-time-serving-for-xgboost-scikit-learn-randomforest-lightgbm-and-more/) and describes how to build and deploy predictive models using [XGBoost](https://www.nvidia.com/en-us/glossary/data-science/xgboost/) and the [Triton Inference Server](https://developer.nvidia.com/nvidia-triton-inference-server) on GPU acclerated servers. 

## Set up PyTorch and Triton Containers

This step will download and run two docker containers from NGC. We will use the PyTorch container to build a predictive model using XGBoost and teh Triton container for model deployment. The two containers are networked so that requests from the PyTorch container can be submitted to the Triton server.

* Make sure you have properly set up your server. See [docs](docs/README.md) for details.
* You should have plenty of disk space (at least 64 GB).
* Open parts 8888, and 8000-8002.

## Docker network

Create a docker network for the containers to communicate with each other, and a shared volume that will be used by the [model repository](https://github.com/triton-inference-server/server/blob/main/docs/model_repository.md).

```
sudo docker network create tritonnet
sudo docker volume create volume1
```

## Triton container

Pull the Triton container (v22.03) from NGC and run it as a service.

```
sudo docker pull nvcr.io/nvidia/tritonserver:22.03-py3
sudo docker run --gpus=all -d -p 8000:8000 -p 8001:8001 -p 8002:8002 --network=tritonnet \
    -v /var/lib/docker/volumes/volume1/_data/model_repository:/models \
    --name tritonserver nvcr.io/nvidia/tritonserver:22.03-py3 tritonserver --model-repository=/models
```

Copy the pre-built model into the model repository.

```
cp <saved_model>
cp <config>
```

Make a note of the IP of the Triton server. You will need to add the IP to the Triton client later configuration later on. 

```
sudo docker network inspect tritonnet
```

Verify Triton is running correctly. The HTTP request returns status 200 if Triton is ready and non-200 if it is not ready. 

```
curl -v localhost:8000/v2/health/ready
```

Check the Triton Logs. You should see the pre-saved model listed in the model repository. If your model is not displayed in the table check the path to the model repository and your CUDA drivers.

```
sudo docker logs tritonserver
```

## PyTorch container

The PyTorch container (v22.03) on NGC has many prebuilt libraries that makes doing data science easy. Mount the shared volume so you can save models to the model repository.

```
sudo docker pull nvcr.io/nvidia/pytorch:22.03-py3
sudo docker run --gpus=all -t -d -p 8888:8888 --ipc=host --ulimit memlock=-1 --ulimit stack=67108864 --network=tritonnet \
    --mount source=volume1,destination=/workspace/volume1 --name pytorch nvcr.io/nvidia/pytorch:22.03-py3
```

Clone this repository onto the server so you will have easy access to the Jupyter Notebooks.

```
sudo docker exec -it pytorch /bin/bash
git clone https://github.com/nwstephens/triton-xgboost.git
exit

```

## Open Jupyter Lab

Jupyter Lab is pre-installed on the PyTorch container. Note that the code below removes the security token. If you want to use a security token when you log into the server, you can remove the comment `--NotebeookApp.token=''`.

```
sudo docker exec -it pytorch /bin/bash
nohup jupyter-lab --NotebookApp.token='' --no-browser --port=8888 &
exit

```

Access Jupyter Lab at `http://<server-ip>:8888/lab?`. Open the [XGBoost](xgboost-model.ipynb) notebook and follow the instructions for building and deploying a model to Triton. Then open the [Triton](triton-deploy.ipynb) notebook and follow the instructions for submitting requests to Triton.
