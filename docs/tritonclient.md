# Tritonclient Python API

The Python API make it easy to communicate with Triton from your C++ or Python application. Using these libraries you can send either HTTP/REST or GRPC requests to Triton to access all its capabilities: inferencing, status and health, statistics and metrics, model repository management, etc. These libraries also support using system and CUDA shared memory for passing inputs to and receiving outputs from Triton. For more information, see the [Tritonclient docs](https://github.com/triton-inference-server/client).

### Example

```
import tritonclient.grpc as triton_grpc
from tritonclient import utils as triton_utils

client = triton_grpc.InferenceServerClient(url='172.18.0.3:8001')

import numpy as np
np_data = np.load('data/np_data.npy')

triton_input = triton_grpc.InferInput('input__0', np_data.shape, 'FP32')
triton_input.set_data_from_numpy(np_data)
triton_output = triton_grpc.InferRequestedOutput('output__0')
response = client.infer('pre-built', model_version='1', inputs=[triton_input], outputs=[triton_output])
print(response.as_numpy('output__0'))
```

