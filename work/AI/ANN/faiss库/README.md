https://github.com/facebookresearch/faiss/blob/v1.8.0/INSTALL.md
需要安装python3.9(python39 python39-devel python39-pip)、pip numpy swig、libgomp、https://www.intel.com/content/www/us/en/developer/tools/oneapi/onemkl-download.html?operatingsystem=linux&linux-install=yum


mkdir build
cd build/
cmake -DCMAKE_BUILD_TYPE=Release -DFAISS_ENABLE_GPU=OFF -DFAISS_ENABLE_PYTHON=ON -DFAISS_OPT_LEVEL=avx2 -DBLA_VENDOR=Intel10_64_dyn -DBUILD_TESTING=OFF ..
# 最好加上可选的 -DBUILD_SHARED_LIBS=ON

make -j faiss  # 会build出 faiss.so

make -j swigfaiss  # 会build出：libfaiss_python_callbacks.so、swigfaiss.py、_swigfaiss.so，如果只需要python可以不make -j faiss

cd build/faiss/python && python3 setup.py install  # 会本地源码安装到：
/usr/local/lib/python3.9/site-packages/faiss-1.13.0-py3.9.egg/faiss

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