# YOLOX 使用 TensorRT 进行加速

> 所有操作建议在 conda 环境下进行操作

## 安装TensorRT

### 下载

下载列表地址：https://developer.nvidia.com/nvidia-tensorrt-8x-download

找到适合自己 CUDA 版本的TensorRT，建议下载 TAR 包

TensortRT8.0 适用于CUDA 113. https://developer.nvidia.com/compute/machine-learning/tensorrt/secure/8.0.3/tars/tensorrt-8.0.3.4.linux.x86_64-gnu.cuda-11.3.cudnn8.2.tar.gz

> 注意将下述所有操作的 TensorRT-8.0.3.4 替换为自己下载安装的版本

### 解压

```
tar -zxvf TensorRT-8.0.3.4.Linux.x86_64-gnu.cuda-11.3.cudnn8.2.tar.gz
```

### 安装

#### 生成环境变量

```
# 将下列语句加入到 ~/.bashrc 文件

export TRT_PATH=/home/szyd/Downloads/TensorRT-8.0.3.4
export PATH=$PATH:$TRT_PATH/bin
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$TRT_PATH/lib
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$TRT_PATH/targets/x86_64-linux-gnu/lib
```

#### 安装 Python TensorRT wheel file

```
cd TensorRT-8.0.3.4/python
python3 -m pip install tensorrt-*-cp3x-none-linux_x86_64.whl
```

> 注意 tensorrt-*-cp3x-none-linux_x86_64.whl 的版本选择要参考自己当前的 python 环境  python3 --version

#### 安装 Python graphsurgeon wheel file

```
cd TensorRT-8.0.3.4/graphsurgeon
python3 -m pip install graphsurgeon-0.4.6-py2.py3-none-any.whl
```

#### 安装 Python onnx-graphsurgeon wheel file

```
cd TensorRT-8.0.3.4/onnx_graphsurgeon
python3 -m pip install onnx_graphsurgeon-0.3.12-py2.py3-none-any.whl
```

## 安装 CUDNN

### 下载

下载列表地址：https://developer.nvidia.com/rdp/cudnn-download

找到适合自己 CUDA 版本的 CUDNN，建议下载 TAR 包

### 安装

#### 解压

```
tar -xvf cudnn-linux-x86_64-8.x.x.x_cudaX.Y-archive.tar.xz
```

#### 将对应的文件复制到 CUDA 目录下，并设置权限

```
sudo cp cudnn-*-archive/include/cudnn*.h /usr/local/cuda/include 
sudo cp -P cudnn-*-archive/lib/libcudnn* /usr/local/cuda/lib64 
sudo chmod a+r /usr/local/cuda/include/cudnn*.h /usr/local/cuda/lib64/libcudnn*
```

## 安装 torch2trt

```
git clone https://github.com/NVIDIA-AI-IOT/torch2trt
cd torch2trt
python setup.py install
```

## 模型转换

修改模型文件尺寸

```
# yolox_base.py 文件 103 行
self.test_size = (1280, 1280)
```

执行模型转换

```
cd /opt/YOLOX
python tools/trt.py -f exps/example/custom/yolox_s.py -c YOLOX_outputs/yolox_s/best_ckpt.pth
```

测试

```
python tools/demo.py image -f exps/default/yolox_s.py --trt --save_result --tsize 1280
```

