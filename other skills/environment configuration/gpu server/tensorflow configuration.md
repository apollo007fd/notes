# tensorflow configuration



1. 安装cpu版本的tensorflow:

   cpu版的比较好装, 直接pip install tensorflow --user就可以了. 

   但是gpu版的对cuda和cudnn的版本有依赖, 需要注意cuda和cudnn的版本不能过低, 或者tensorflow版本过高.

2. 安装gpu版本的tensorflow:

​        pip3 install tensorflow-gpu --user

安装过程没报错, 使用时报错. 有2个原因. 安装过程信息如下:

```shell
apollo@micc-PR4768GW-Invalid-entry-length-16-Fixed-up-to-11:~$ pip3 install tensorflow-gpu --user
Looking in indexes: http://pypi.douban.com/simple/
Collecting tensorflow-gpu
  Downloading http://pypi.doubanio.com/packages/f2/fa/01883fee1cdb4682bbd188edc26da5982c459e681543bb7f99299fca8800/tensorflow_g                                                                                pu-1.8.0-cp35-cp35m-manylinux1_x86_64.whl (216.3MB)
    100% |████████████████████████████████| 216.3MB 11.5MB/s
Collecting protobuf>=3.4.0 (from tensorflow-gpu)
  Downloading http://pypi.doubanio.com/packages/11/c4/8a35f5af5f26040ae7f3d521875e43429d2955d598fa3f2d0b6b88133bb1/protobuf-3.6                                                                                .0-cp35-cp35m-manylinux1_x86_64.whl (7.1MB)
    100% |████████████████████████████████| 7.1MB 12.1MB/s
Collecting gast>=0.2.0 (from tensorflow-gpu)
  Downloading http://pypi.doubanio.com/packages/5c/78/ff794fcae2ce8aa6323e789d1f8b3b7765f601e7702726f430e814822b96/gast-0.2.0.t                                                                                ar.gz
Requirement already satisfied: wheel>=0.26 in /usr/lib/python3/dist-packages (from tensorflow-gpu) (0.29.0)
Requirement already satisfied: numpy>=1.13.3 in ./.local/lib/python3.5/site-packages (from tensorflow-gpu) (1.14.5)
Collecting tensorboard<1.9.0,>=1.8.0 (from tensorflow-gpu)
  Downloading http://pypi.doubanio.com/packages/59/a6/0ae6092b7542cfedba6b2a1c9b8dceaf278238c39484f3ba03b03f07803c/tensorboard-                                                                                1.8.0-py3-none-any.whl (3.1MB)
    100% |████████████████████████████████| 3.1MB 11.9MB/s
Collecting absl-py>=0.1.6 (from tensorflow-gpu)
  Downloading http://pypi.doubanio.com/packages/57/8d/6664518f9b6ced0aa41cf50b989740909261d4c212557400c48e5cda0804/absl-py-0.2.                                                                                2.tar.gz (82kB)
    100% |████████████████████████████████| 92kB 14.5MB/s
Collecting grpcio>=1.8.6 (from tensorflow-gpu)
  Downloading http://pypi.doubanio.com/packages/8a/c1/d5af6bd4d0d90804a1c48f8e52e428fba587d23880d66ad47eaf62b1adc7/grpcio-1.13.                                                                                0-cp35-cp35m-manylinux1_x86_64.whl (9.1MB)
    100% |████████████████████████████████| 9.1MB 11.5MB/s
Collecting astor>=0.6.0 (from tensorflow-gpu)
  Downloading http://pypi.doubanio.com/packages/35/6b/11530768cac581a12952a2aad00e1526b89d242d0b9f59534ef6e6a1752f/astor-0.7.1-                                                                                py2.py3-none-any.whl
Collecting termcolor>=1.1.0 (from tensorflow-gpu)
  Downloading http://pypi.doubanio.com/packages/8a/48/a76be51647d0eb9f10e2a4511bf3ffb8cc1e6b14e9e4fab46173aa79f981/termcolor-1.                                                                                1.0.tar.gz
Requirement already satisfied: six>=1.10.0 in /usr/lib/python3/dist-packages (from tensorflow-gpu) (1.10.0)
Requirement already satisfied: setuptools in /usr/lib/python3/dist-packages (from protobuf>=3.4.0->tensorflow-gpu) (20.7.0)
Collecting werkzeug>=0.11.10 (from tensorboard<1.9.0,>=1.8.0->tensorflow-gpu)
  Downloading http://pypi.doubanio.com/packages/20/c4/12e3e56473e52375aa29c4764e70d1b8f3efa6682bef8d0aae04fe335243/Werkzeug-0.1                                                                                4.1-py2.py3-none-any.whl (322kB)
    100% |████████████████████████████████| 327kB 49.2MB/s
Collecting bleach==1.5.0 (from tensorboard<1.9.0,>=1.8.0->tensorflow-gpu)
  Downloading http://pypi.doubanio.com/packages/33/70/86c5fec937ea4964184d4d6c4f0b9551564f821e1c3575907639036d9b90/bleach-1.5.0                                                                                -py2.py3-none-any.whl
Collecting markdown>=2.6.8 (from tensorboard<1.9.0,>=1.8.0->tensorflow-gpu)
  Downloading http://pypi.doubanio.com/packages/6d/7d/488b90f470b96531a3f5788cf12a93332f543dbab13c423a5e7ce96a0493/Markdown-2.6                                                                                .11-py2.py3-none-any.whl (78kB)
    100% |████████████████████████████████| 81kB 13.9MB/s
Collecting html5lib==0.9999999 (from tensorboard<1.9.0,>=1.8.0->tensorflow-gpu)
  Downloading http://pypi.doubanio.com/packages/ae/ae/bcb60402c60932b32dfaf19bb53870b29eda2cd17551ba5639219fb5ebf9/html5lib-0.9                                                                                999999.tar.gz (889kB)
    100% |████████████████████████████████| 890kB 11.8MB/s
Building wheels for collected packages: gast, absl-py, termcolor, html5lib
  Running setup.py bdist_wheel for gast ... done
  Stored in directory: /home/apollo/.cache/pip/wheels/af/f5/6b/969ee0fce5a60134761e2949c105918c9ff8938256be496541
  Running setup.py bdist_wheel for absl-py ... done
  Stored in directory: /home/apollo/.cache/pip/wheels/33/b5/56/cf4be10dffb72d9f467bd283a4fa6ff29350faf9fcc5ed51ab
  Running setup.py bdist_wheel for termcolor ... done
  Stored in directory: /home/apollo/.cache/pip/wheels/12/ef/81/0a093cc726842acb8f2a8c2455b28d4c333db4b7bef81fdf90
  Running setup.py bdist_wheel for html5lib ... done
  Stored in directory: /home/apollo/.cache/pip/wheels/fe/31/27/9dd533e6032ba113bc63d6d1077d057570ac1043bbdcc9bcbe
Successfully built gast absl-py termcolor html5lib
Installing collected packages: protobuf, gast, werkzeug, html5lib, bleach, markdown, tensorboard, absl-py, grpcio, astor, termc                                                                                olor, tensorflow-gpu
  Found existing installation: html5lib 1.0.1
    Uninstalling html5lib-1.0.1:
      Successfully uninstalled html5lib-1.0.1
  Found existing installation: bleach 2.1.3
    Uninstalling bleach-2.1.3:
      Successfully uninstalled bleach-2.1.3
Successfully installed absl-py-0.2.2 astor-0.7.1 bleach-1.5.0 gast-0.2.0 grpcio-1.13.0 html5lib-0.9999999 markdown-2.6.11 proto                                                                                buf-3.6.0 tensorboard-1.8.0 tensorflow-gpu-1.8.0 termcolor-1.1.0 werkzeug-0.14.1

```

安装完毕, import tensorflow as tf报错:

```shell
apollo@micc-PR4768GW-Invalid-entry-length-16-Fixed-up-to-11:~$ python3
Python 3.5.2 (default, Nov 23 2017, 16:37:01)
[GCC 5.4.0 20160609] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import tensorflow as tf
Traceback (most recent call last):
  File "/home/apollo/.local/lib/python3.5/site-packages/tensorflow/python/pywrap_tensorflow.py", line 58, in <module>
    from tensorflow.python.pywrap_tensorflow_internal import *
  File "/home/apollo/.local/lib/python3.5/site-packages/tensorflow/python/pywrap_tensorflow_internal.py", line 28, in <module>
    _pywrap_tensorflow_internal = swig_import_helper()
  File "/home/apollo/.local/lib/python3.5/site-packages/tensorflow/python/pywrap_tensorflow_internal.py", line 24, in swig_impo                                                                                rt_helper
    _mod = imp.load_module('_pywrap_tensorflow_internal', fp, pathname, description)
  File "/usr/lib/python3.5/imp.py", line 242, in load_module
    return load_dynamic(name, filename, file)
  File "/usr/lib/python3.5/imp.py", line 342, in load_dynamic
    return _load(spec)
ImportError: libcublas.so.9.0: cannot open shared object file: No such file or directory

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/home/apollo/.local/lib/python3.5/site-packages/tensorflow/__init__.py", line 24, in <module>
    from tensorflow.python import pywrap_tensorflow  # pylint: disable=unused-import
  File "/home/apollo/.local/lib/python3.5/site-packages/tensorflow/python/__init__.py", line 49, in <module>
    from tensorflow.python import pywrap_tensorflow
  File "/home/apollo/.local/lib/python3.5/site-packages/tensorflow/python/pywrap_tensorflow.py", line 74, in <module>
    raise ImportError(msg)
ImportError: Traceback (most recent call last):
  File "/home/apollo/.local/lib/python3.5/site-packages/tensorflow/python/pywrap_tensorflow.py", line 58, in <module>
    from tensorflow.python.pywrap_tensorflow_internal import *
  File "/home/apollo/.local/lib/python3.5/site-packages/tensorflow/python/pywrap_tensorflow_internal.py", line 28, in <module>
    _pywrap_tensorflow_internal = swig_import_helper()
  File "/home/apollo/.local/lib/python3.5/site-packages/tensorflow/python/pywrap_tensorflow_internal.py", line 24, in swig_impo                                                                                rt_helper
    _mod = imp.load_module('_pywrap_tensorflow_internal', fp, pathname, description)
  File "/usr/lib/python3.5/imp.py", line 242, in load_module
    return load_dynamic(name, filename, file)
  File "/usr/lib/python3.5/imp.py", line 342, in load_dynamic
    return _load(spec)
ImportError: libcublas.so.9.0: cannot open shared object file: No such file or directory
```

错误原因:

1. 第一个原因:

   因为之前装的cuda8.0, 这个libcublas.so.9.0对应的是cuda9.0.

   不指定tensorflow-gpu的版本,默认安装最新版的---1.8.0, 然而服务器装的是cuda8.0和cudnn v5.1, 最高只支持到1.2.

   所以只能卸载刚才装的tensorflow,  安装1.2版.

   

2. 第二个原因:

   没有将cuda的库添加到环境变量中去.参考:https://blog.csdn.net/u010454261/article/details/71268325



- 卸载1.8.0

pip3 uninstall tensorflow  //出现以下错误, 要指定卸载gpu版本的tensorflow:

![1530956242543](assets/1530956242543.png)

pip3 uninstall tensorflow-gpu  //卸载成功:

![1530956936812](assets/1530956936812.png)



- 重新安装1.2:

pip3 install tensorflow-gpu==1.2 --user  //安装成功



安装过程尝试过安装1.4gpu版的, 然而遇到错误:

ImportError: libcudnn.so.6: cannot open shared object file: No such file or directory

原因是1.4版的需要cudnn v6.x,  参考:https://blog.csdn.net/silent56_th/article/details/77587792



tensorflow指定GPU

```
import os
os.environ["CUDA_VISIBLE_DEVICES"] = "0"
```

