# Troubleshooting FAQ

#### 1. Why do I get CUDA memory allocation errors?
The data set might be too large for your instance. Try using a larger GPU or restricting the data set size using `cudf.read_csv(train_csv, nrows=100000)`.

#### 2. Why do I get CUDA driver errors when starting my containers?
The easiest way to handle this is to restart the server.

#### 3. Why won't Triton respond to the client?
Check that [Triton is running](https://github.com/triton-inference-server/server/blob/main/docs/quickstart.md#verify-triton-is-running-correctly) and that you have the correct IP address in the client config. You can check the IP address with `sudo docker network inspect tritonnet`. Also make sure you ports are open. Triton uses ports `8000-8002`.

#### 4. Why can't I open Jupyter Lab?
Make sure Jupyter Lab is running and that port `8888` is open on the server.
