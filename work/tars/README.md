TAF内/TARS外 framework 一个后台逻辑层的高性能RPC框架
https://tarscloud.github.io/TarsDocs/
tarsgo
https://github.com/TarsCloud/TarsGo/blob/master/README.zh.md
Tars RPC所使用的IDL（接口定义）格式是JCE(类似于GRPC中的Protobuf), 扩展名是.tars或.jce(只是扩展名不同，实质无差别)。
提供了Tars2go工具: tars文件转换为go语言，并提供JCE了序列化和反序列化的能力

APP: APP(应用)的概念经常出现在各类Tars有关的文档和配置平台中，其实就是一个划分。一般一个业务或一个部门的服务会被分配到一个APP名，按需选择和填写即可。
Server: 服务就是在Tars运维平台上用来查找服务的名称，同一份代码也可以部署成多个服务。同一个服务的同一个Set拥有相同的配置
Obj: 一个服务可以对外提供多个接口(接口是方法的集合)，一个接口对应一个OBJ。
APP.Server.OBJ 三个组成Tars RPC时的被调名称

Communicator: 通讯器，用于生成远程被调服务的本地代理对象。通讯器本身持有RPC相关的配置参数，有调整时一般是修改通讯器配置. //如果每次都new出来则是创建的短连接
JCE: Tars RPC使用的IDL文件格式。部分语境下也可能表示JCE的序列化实现。(以前的TAF框架使用的序列化/反序列化协议，现在的tRPC框架已经使用protobuf作为序列化/反序列化层的协议了)
Imp: 通过JCE知识定义了接口，具体的实现在大部分编程语言中都会通过一个类对象来实现，这个对象常被称作Imp对象.


先定义一个tars文件，类似gprc中，先定义一个proto
tars.go格式如：
module <module>
{
  interface <class1>
  {
    int <func>(string req, string rsp);
  };
};

然后使用tars2go --outdir=./ <IDL_file>
|定义接口文件名|生成的文件1（用于rpc）|生成的文件2（用于序列化的协议）|
|---|---|---|
|qrw.proto|qrw.grpc.pb.h/cc|grq.pb.h/cc|
|qrw.tars|qrw.tars.go|qrw.go|
|qrw.jce|qrw.tars.go|grw.jce.go|


|tars/jce|go里的类型映射
|---|---|
|enum|int32|
|string|string|
|int|int32|
|vector\<byte>|[]int8|



格式如：
type <class1> struct {
  s m.Servant
}

func (_obj *<class1>) <func 首字母转了大写，但是下划线还是会留着>(req string, rsp *string, _opt ...map[string]string) (ret int32, err error) {
func (_obj *<class1>) <func 首字母转了大写，但是下划线还是会留着>WithContext(tarsCtx context.context, req string, rsp *string, _opt ...map[string]string) (ret int32, err error) {
func (_obj *<class1>) <func 首字母转了大写，但是下划线还是会留着>OneWayWithContext(tarsCtx context.context, req string, rsp *string, _opt ...map[string]string) (ret int32, err error) {
func (_obj *<class1>) Dispatch(tarsCtx context.Context, _val interface{}, tarsReq *requestf.RequestPacket, tarsResp *requestf.ResponsePacket, _withContext bool) (err error) {
type _imp<class1> interface {
	<func>(req *, resp *) (ret int32, err error)
}
然后就可以使用了

主调方：
使用三种<func>发请求（
被调方：
单独创建一个imp类，实现接口<func>即可

