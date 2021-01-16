参考 https://tarscloud.github.io/TarsDocs/hello-world/tarsgo.html

git@github.com:TarsCloud/TarsGo.git

TarsGo/tars/tools里面
sh create_tars_server.sh AppY ServerY ServantY

cd $GOPATH/src/AppY/ServerY

make # tars2go -outdir=vendor ObjY.tars ; go build -o ServerY; ./ServerY --config=config.conf


//客户端
cd client; go build; ./client