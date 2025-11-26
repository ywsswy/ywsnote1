https://github.com/facebookresearch/faiss/blob/v1.8.0/INSTALL.md

mkdir build
cd build/
cmake -DCMAKE_BUILD_TYPE=Release -DFAISS_ENABLE_GPU=OFF -DFAISS_ENABLE_PYTHON=ON -DFAISS_OPT_LEVEL=avx2 -DBLA_VENDOR=Intel10_64_dyn -DBUILD_TESTING=OFF ..
# 可选-DBUILD_SHARED_LIBS=ON

make -j faiss  # 会生成libfaiss.a/so，需要放到系统lib目录

make  -j swigfaiss  # 会build出：libfaiss_python_callbacks.a/so、swigfaiss.py、_swigfaiss.so

cd build/faiss/python && python setup.py install  # 会本地源码安装到：
/usr/local/lib/python3.8/site-packages/faiss-1.13.0-py3.8.egg/faiss

export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/opt/intel/oneapi/compiler/latest/lib/

然后就可以测试bench目录里的脚本




如果是从官方源直接python3 -m pip install faiss-cpu，安装的是：/usr/local/lib64/python3.8/site-packages/faiss
array_conversions.py
contrib
gpu_wrappers.py
loader.py
python_callbacks.h
_swigfaiss_avx2.cpython-38-x86_64-linux-gnu.so
_swigfaiss_avx512.cpython-38-x86_64-linux-gnu.so
_swigfaiss.cpython-38-x86_64-linux-gnu.so
swigfaiss.py
class_wrappers.py
extra_wrappers.py
__init__.py
__pycache__
setup.py
swigfaiss_avx2.py
swigfaiss_avx512.py
swigfaiss.i