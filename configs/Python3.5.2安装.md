## Python3.5.2安装

### 安装Python

* 下载Python3.5.2 ,下载地址:[https://www.python.org/ftp/python/](https://www.python.org/ftp/python/3.5.2/Python-3.5.2.tgz)
* 编译安装

```shell
安装必要依赖
yum install openssl-devel   -y
yum install zlib-devel  -y

cd Python-3.5.2
./configure --prefix=/opt/Python     #安装目录可以自己定义无所谓。
make
make install
```

* 更改系统默认python版本为Python3.5;建立软链接

```shell
mv /usr/bin/python /usr/bin/python2.7.5
ln -s /usr/local/python/bin/python3.5 /usr/bin/python
```

* 核对Python版本

```shell
python -V
```

* 解决python升级后，YUM不能正常工作的问题

```shell
#vi /usr/bin/yum
将文件头部的 　　#!/usr/bin/python 　　改成 　　#!/usr/bin/python2.7.5

再次运行yum命令，就不回再报错了
如果运行后报以下错误File "/usr/libexec/urlgrabber-ext-down", line 28
    except OSError, e:
就修改/usr/libexec/urlgrabber-ext-down文件，将python同样指向旧版本，就可以了
```

### 安装pip

* 首先安装setuptools

```shell
wget --no-check-certificate  https://pypi.python.org/packages/source/s/setuptools/setuptools-19.6.tar.gz#md5=c607dd118eae682c44ed146367a17e26

tar -zxvf setuptools-19.6.tar.gz

cd setuptools-19.6.tar.gz

python3 setup.py build

python3 setup.py install
```

* 然后安装pip

```shell
wget --no-check-certificate  https://pypi.python.org/packages/source/p/pip/pip-8.0.2.tar.gz#md5=3a73c4188f8dbad6a1e6f6d44d117eeb

tar -zxvf pip-8.0.2.tar.gz

cd pip-8.0.2

python3 setup.py build

python3 setup.py install
```

```shell
pip install --upgrade pip  更新pip

pip3 -V
```

