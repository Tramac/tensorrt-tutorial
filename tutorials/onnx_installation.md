# ONNX-TensorRT安装教程

因为我们的模型是按照PyTorch—>onnx—>TensorRT的路线进行转化，所以在安装TensorRT之后，需要进一步安装onnx环境。

## 1 环境准备

**1.1 TensorRT安装**

TensorRT的安装可以参考—[TensorRT快速开始](./quick_start.md)

**1.2 protobuf>= 3.8.x**

`onnx-tensorrt`编译时依赖`protobuf`，在编译之前需要保证已经成功安装。

- Ubuntu安装：

```bash
sudo apt-get install libprotobuf-dev protobuf-compiler
```

- Centos安装：

```shell
sudo yum install libprotobuf-dev protobuf-compiler
```

- 源码安装：

```shell
git clone https://github.com/protocolbuffers/protobuf.git
cd protobuf
git submodule update --init --recursive
./autogen.sh

./configure
make
make check
sudo make install
sudo ldconfig
```

**1.3 gcc=8.2**

针对`onnx-tensorrt`与`TensorRT 7.0`，分别尝试过用`gcc 4.8.2`、`gcc 5.3`以及`gcc 8.2`进行编译，只有`gcc 8.2`可编译成功。

**1.4 pycuda**

可选，如果有python安装需求的必须安装。

```shell
pip install pycuda
```

## 2 编译安装

**2.1 onnx-tensorrt下载**

```shell
git clone --recursive https://github.com/onnx/onnx-tensorrt.git
```

注意：参数`--recursive`不可省略，保证`onnx-tensorrt`中的第三方依赖`third_party/onnx`可成功下载。

**2.2 编译&安装**

```shell
cd onnx-tensorrt
mkdir build && cd build
# cmake时可能会出现诸多问题，解决方法可参考QA部分
cmake .. -DProtobuf_INCLUDE_DIR=/path/to/protobuf/src -DTENSORRT_ROOT=/path/to/TensorRT-7.0.0.11

make -j8
sudo make install
```

若安装成功，则显示如下：

```shell
/usr/bin/make64 MAC=64 install
[  2%] Built target gen_onnx_proto
[ 12%] Built target onnx_proto
[ 22%] Built target nvonnxparser_static
[ 33%] Built target nvonnxparser
[ 93%] Built target onnx
[ 95%] Built target onnx2trt
[100%] Built target getSupportedAPITest
Install the project...
-- Install configuration: "Release"
-- Installing: /usr/local/bin/onnx2trt
-- Set runtime path of "/usr/local/bin/onnx2trt" to ""
-- Installing: /usr/local/lib/libnvonnxparser.so.7.0.0
-- Installing: /usr/local/lib/libnvonnxparser.so.7
-- Set runtime path of "/usr/local/lib/libnvonnxparser.so.7.0.0" to ""
-- Installing: /usr/local/lib/libnvonnxparser.so
-- Installing: /usr/local/lib/libnvonnxparser_static.a
-- Installing: /usr/local/include/NvOnnxParser.h
```

**2.3 安装python库**

1）修改setup.py文件

找到第52行，添加`-I{/path/to/TensorRT-7.0.0.11/include}`。

```python
52 SWIG_OPTS = [
53     '-I/path/to/TensorRT-7.0.0.11/include',
54     '-c++',
55     '-modern',
56     '-builtin',
57 ]
```

2）安装

```shell
cd onnx-tensorrt
python setup.py install
```

若安装成功，则显示如下：

```shell
Using /xxx/3.6.5/lib/python3.6/site-packages
Finished processing dependencies for onnx-tensorrt==0.1.0
```

## 3 验证

```python
>>> import onnx
>>> import onnx_tensorrt.backend as backend
```

## 参考

- [onnx-tensorrt](https://github.com/onnx/onnx-tensorrt)
- [protobuf](https://github.com/protocolbuffers/protobuf)
- [swig](http://www.swig.org/Doc4.0/SWIGDocumentation.pdf)
- [speeding-up-deep-learning-inference-using-tensorrt](https://developer.nvidia.com/blog/speeding-up-deep-learning-inference-using-tensorrt/)
- [onnx-tensorrt-issue](https://github.com/onnx/onnx-tensorrt/issues/350)

## QA

- [安装问题及解决方案](../docs/QA_1-2.md)
