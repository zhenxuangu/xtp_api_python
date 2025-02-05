# 使用源码编译

## Python

代码基于 Python 3.6.5, 你可以通过你喜欢的方式安装 Python 3.6的版本


如 Ubuntu：

```bash
apt install python3.6 python3.6-dev
```

注意，需要python3.6-dev, 后续编译 boost 用到

对于有多个 Python 版本，可以考虑 virtualenv 创建一个干净的环境

```bash
virtualenv --python=/usr/bin/python3.6 env/python3.6
```

> /usr/bin/python3.6 换成你当前安装的路径

```bash
which python3.6
```

应用Python 3.6

```bash
source ./env/python3.6/bin/activate
 ```

看到如下提示：

 > (python3.6.5)

 说明已经成功切换了

## Boost

如果系统没有安装 boost，会看到如下错误

> ImportError: libboost_python.so.1.66.0: cannot open shared object file: No such file or directory

安装如下：

```bash
wget https://dl.bintray.com/boostorg/release/1.66.0/source/boost_1_66_0.tar.gz
tar zxf boost_1_66_0.tar.gz
cd boost_1_66_0
./bootstrap.sh --with-python=/usr/bin/python3.6
sudo ./b2 install --with-python include="/usr/include/python3.6m"  --with-atomic  --with-log  --with-test --with-thread --with-date_time  --with-chrono
```

安装后的文件在 /usr/local/lib/ 目录下

## 编译源码

这里以 Python 3 为例

```bash
cd source/Linux/xtp_python3_2.2.25.5/
```

主要修改两个地方

### Python

* PYTHON_INCLUDE_PATH
* PYTHON_LIBRARY

```shell
	set(PYTHON_LIBRARY /usr/lib/python3.6/)
	set(PYTHON_INCLUDE_PATH /usr/include/python3.6m)
```

### Boost

* BOOST_ROOT

```shell
    set(BOOST_ROOT   /usr/local/lib/)
```


执行 build.sh 编译

```bash
./build.sh
```

编译的结果在 build/lib 目录下，拷贝文件至 /usr/local/lib

```bash
cp build/lib/*.so /usr/local/lib
```

## 测试代码

好了，到此我们的环境已经配置好，可以尝试运行测试代码。在运行之前，请确认你已经拥有了测试账号，如果没有，请先至官网注册： https://xtp.zts.com.cn/registration-form.html

如果 你的 /usr/local/lib 不在当前的环境中，可手工加载进来，在测试代码最上方，加上

```python
# encoding: UTF-8

import sys
sys.path.append(r"/usr/local/lib")
```


