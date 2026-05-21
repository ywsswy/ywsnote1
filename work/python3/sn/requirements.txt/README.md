## 作用：让所有开发者保持一致，所以推荐版本固定，形如：

```
requests==2.31.0
numpy>=1.26
```

然后每个开发者开发项目前先：
python3 -m pip install -r requirements.txt [--no-deps]  # 可选无视依赖冲突


## 还有一个setup.py文件
作用：需要打包发布出去，理解成会生成“说明书”给后续使用者install时参考，形如：

```
from setuptools import setup, find_packages

setup(
    name="myproject",
    version="0.1.0",
    packages=find_packages(),
    install_requires=[
        "requests>=2.31.0",
        "numpy",
    ],
)
```