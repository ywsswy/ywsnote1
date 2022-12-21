# 不使用yum install protobuf-compiler，因为这个版本低

git clone https://github.com/protocolbuffers/protobuf

cd protobuf
git checkout v3.6.1.3

./autogen.sh
./configure
make -j8
make install