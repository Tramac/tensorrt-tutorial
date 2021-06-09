# TensorRT快速开始

## TensorRT安装

官方提供了多种安装方式，包括DEB、RPM与TAR安装。这里我们选择TAR安装（类似于绿色免安装，解压即可）方式：

1）从[官网](https://developer.nvidia.com/nvidia-tensorrt-download)下载与环境相匹配的tar安装包，如[TensorRT 7.0.0.11](https://developer.nvidia.com/compute/machine-learning/tensorrt/secure/7.0/7.0.0.11/tars/TensorRT-7.0.0.11.CentOS-7.6.x86_64-gnu.cuda-10.0.cudnn7.6.tar.gz)

<p align="center"><img width="80%" src="docs/tensorrt_download.jpg" /></p>

2）解压安装包

```shell
tar -xzvf TensorRT-7.0.0.11.CentOS-7.6.x86_64-gnu.cuda-10.0.cudnn7.6.tar.gz
```

3）添加环境变量，使程序能够找到TensorRT的libs

```shell
vim ~/.bashrc
# 添加以下内容
export LD_LIBRARY_PATH=/path/to/TensorRT-7.0.0.11/lib:$LD_LIBRARY_PATH
export LIBRARY_PATH=/path/to/TensorRT-7.0.0.11/lib::$LIBRARY_PATH
```

以上，安装完毕～

## 安装TensorRT组件

TensorRT 同时支持 C++ 和 Python。本质上，C++ API 和 Python API 在需求支持方面接近相同， Python API 的主要优点是数据预处理和后处理更加方便，因为可以使用各种库，如 NumPy 和 SciPy，由于日常工作中用Python居多，所以这里只介绍如何使用Python API的部分。

1）python调用tensorrt

在上一部分中，虽然我们已经安装了TensorRT，但是我们的Python环境还不能通过`import tensorrt`导入，所以需要通过安装对应的`.whl`来实现。

```shell
pip3 install /path/to/TensorRT-7.0.0.11/python/tensorrt-7.0.0.11-cp36-none-linux_x86_64.whl
```

注意：选择与python版本对应的`.whl`文件

安装完成之后，可以执行以下命令检查是否安装成功：

```python
>>> import tensorrt as trt
>>> trt.__version__
'7.0.0.11'
```

2）安装uff组件

uff 包用于将经过训练的模型从各种框架转换为通用格式。

```shell
pip3 install /path/to/TensorRT-7.0.0.11/uff/uff-0.6.5-py2.py3-none-any.whl
```

同样可以用以下方式验证安装是否成功：

```python
>>> import uff
>>> uff.__version__
'0.6.5'
```

3）安装graphsurgeon组件

graphsurgeon用于转换 TensorFlow 图。 其功能主要包含在 TensorFlow 图中查找节点以及修改、添加或删除节点。

```shell
pip3 install /path/to/TensorRT-7.0.0.11/graphsurgeon/graphsurgeon-0.4.1-py2.py3-none-any.whl
```

## 测试

TensorRT提供了多个samples可用于测试安装是否成功。

1）编译samples

```shell
# cd /path/to/TensorRT-7.0.0.11/samples
make -j8
```

Note: 如果编译出现报错，可以参考QA部分

2）验证

```
# cd /path/to/TensorRT-7.0.0.11
./bin/sample_int8 mnist
```

Note: 验证之前确认已经将tensorrt libs添加到环境变量中

## 参考

- [官方介绍](https://developer.nvidia.com/zh-cn/tensorrt)
- [python_api](https://docs.nvidia.com/deeplearning/tensorrt/api/python_api/)
- [segmentation model](https://developer.nvidia.com/blog/speeding-up-deep-learning-inference-using-tensorrt/)
- [classification model](https://developer.nvidia.com/blog/speed-up-inference-tensorrt/)
- [cudnn](https://developer.nvidia.com/rdp/cudnn-archive)

## QA

1.`cannot find -lcudnn`及[解决方案](./docs/QA_1-1.md)
