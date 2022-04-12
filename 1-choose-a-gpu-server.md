# Where to find NVIDIA GPU's

GPU's are avaiable in the cloud, in the data center, and for personal computing. The following is a partial list of where GPU's are available.

### Cloud service providers
1. [AWS](https://aws.amazon.com/) offers 
2. [Azure](https://docs.microsoft.com/en-us/azure/)
3. [GCP](https://cloud.google.com/)

### Machine Learning Platforms
1. [AWS Sagemaker](https://aws.amazon.com/sagemaker/)
2. [AzureML](https://azure.microsoft.com/en-us/services/machine-learning/)
3. [Google Colab](https://colab.research.google.com/)

### GPU providers
1. [Paperspace](https://www.paperspace.com/)

### Data centers
1. [NVIDIA Launchpad](https://www.nvidia.com/en-us/data-center/launchpad/)

### Personal computing
1. Workstations
2. Laptops

## Compute capability

The software you run on an NVIDIA GPU requires a certain compute capability. The compute capability can be found [here](https://developer.nvidia.com/cuda-gpus). NVIDIA chip architectures (e.g. Volta, Turing, Ampere, Hopper) have features that are associated to a specific [compute capability](). The software you are running should match the compute capability of the GPU. For example, RAPIDS requires a compute capability greater than 6, which means you should run RAPIDS on a P100, V100, T4, or an Ampere in the data center.

### NVIDIA Data Center Products

GPU	| Compute Capability
----|-------------------
NVIDIA A100 | 8.0
NVIDIA A40	| 8.6
NVIDIA A30	| 8.0
NVIDIA A10	| 8.6
NVIDIA A16	| 8.6
NVIDIA A2	| 8.6
NVIDIA T4	| 7.5
NVIDIA V100	| 7.0
Tesla P100	| 6.0

### NVIDIA Quadro and NVIDIA RTX Desktop GPUs

GPU | Compute Capability
----|-------------------
RTX A6000	| 8.6
RTX A5000	| 8.6
RTX A4000	| 8.6
T1000	| 7.5
T600	| 7.5
T400	| 7.5
Quadro RTX 8000	| 7.5
Quadro RTX 6000	| 7.5
Quadro RTX 5000	| 7.5
Quadro RTX 4000	| 7.5
Quadro GV100	| 7.0
Quadro GP100	| 6.0

### GeForce Notebook Products

GPU | Compute Capability
----|-------------------
GeForce RTX 3080	| 8.6
GeForce RTX 3070	| 8.6
GeForce RTX 3060	| 8.6
GeForce RTX 3050 Ti	| 8.6
GeForce RTX 3050	| 8.6
Geforce RTX 2080	| 7.5
Geforce RTX 2070	| 7.5
Geforce RTX 2060	| 7.5
GeForce GTX 1080	| 6.1
GeForce GTX 1070	| 6.1
GeForce GTX 1060	| 6.1

### Compute capability prerequisites software

* [RAPIDS](https://rapids.ai/start.html) 22.04 requires compute capability 6.0+.
* [PyTorch](https://docs.nvidia.com/deeplearning/frameworks/pytorch-release-notes/index.html) 22.03 requires compute capability 6.0+.
