# Testing manual for https://github.com/harrywang/self-contained-python-repo.git, the mindiffusion part.

## 1. Purpose
The purpose of this manual is to give a detailed step by step instruction of how to run the code. 

### 1.1 code to test
self-contained-python-repo/train_mnist.py

## 2. Device used in test
+---------------------------------------------------------------------------------------+
| NVIDIA-SMI 535.171.04             Driver Version: 535.171.04   CUDA Version: 12.2     |
|-----------------------------------------+----------------------+----------------------+
| GPU  Name                 Persistence-M | Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp   Perf          Pwr:Usage/Cap |         Memory-Usage | GPU-Util  Compute M. |
|                                         |                      |               MIG M. |
|=========================================+======================+======================|
|   0  NVIDIA A100-SXM4-40GB          Off | 00000000:31:00.0 Off |                  Off |
| N/A   37C    P0              34W / 400W |     13MiB / 40960MiB |      0%      Default |
|                                         |                      |             Disabled |
+-----------------------------------------+----------------------+----------------------+
|   1  NVIDIA A100-SXM4-40GB          Off | 00000000:4B:00.0 Off |                  Off |
| N/A   38C    P0              33W / 400W |     13MiB / 40960MiB |      0%      Default |
|                                         |                      |             Disabled |
+-----------------------------------------+----------------------+----------------------+

## 3. Step by step guide

### setup environment
1. clone the git repo from GitHub (here I cloned the repo under path: ~/Desktop): 
```bash
$ git clone https://github.com/harrywang/self-contained-python-repo.git
```
   
2. To test the repo, first make a virtual environment. 

a. If using conda, which better at working with different python versions, and I choose python 3.9 here:
```bash
$ conda create -n repo_test python=3.9
$ conda activate repo_test
```

b. If use venv, you should be careful if you have conda in the environment.
You should always specify the python version when using python, since the default python comes with conda.
In my machine, python3.10 is the version that is different from conda.
```bash
$ python3.10 -m venv venv
$ source venv/bin/activate
```


3. Make sure you are under the right path `~/self-contained-python-repo`, then you can download required packages using pip (here the server default uses aliyun mirror, or you can try Tingshua Mirror if you are in maindland China:` pip install -i https://pypi.tuna.tsinghua.edu.cn/simple -r requirements.txt`):
```bash
$ pip install -r requirements.txt
```

4. Before really running the code, make sure all the mentioning directories in the script existed. 

In this specific example, two most straightfoward ways are:

a. directly create the folder `./contents` under `/self-contained-python-repo`

b. In script `train_mnist.py`, you should add the following codes:

```
import os
# create folder if it does not exit
os.makedirs("./contents", exist_ok=True)
```

before `save_image(grid, f"./contents/ddpm_sample_{i}.png")`

5. Run with nohup (a way of running script in background, which is especially essential for using sever, since it would not be influenced by connection issue).

a. for conda
```bash
$ nohup python train_mnist.py > log.log&
```

b. for venv
```bash
$ nohup python3.10 train_mnist.py > log.log&
```


The `log.log` is the log file, it can be found via `cat log.log ` if you did not see it (while it appears at the current working dictory by default);

You can also check the running status using 
```bash
$ ps -ax | train_mnist.py
```
The command should return the pid.

If you want to stop the process, you should use
```bash
$ kill pid
``` 

## 4. Result
After running, you can check sample img under folder `./contents`, and the model is saved as `ddpm_mnist.pth`.

With single `A100-SXM4-40GB`, it takes 1 to 2 minutes for each epoch.
