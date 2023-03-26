Export 导出dll
TaskManager::Instance()->ProcessTask 主调用
设置mGuardModule指针数组依次指向执行模块守卫
执行TaskManager::ProcessTask
【模块
mGuardModule[i]->Initialize依次加入此模块中的处理单元
{
this->AddProcessUnit(new ProcessParamerInvalidCheck());
this->mProcessUnit[this->mProcessIndx++] = process;
this->AddProcessUnit(new xxxxx());
this->mProcessUnit[this->mProcessIndx++] = process;
this->AddProcessUnit(new yyyyyyy());
this->mProcessUnit[this->mProcessIndx++] = process;
}
执行 ret = this->mGuardModule[i]->ToDealTask(pCtx);
【处理单元
bRet = this->mProcessUnit[i]->Process(pContent);
】
。。。
】
YS-100 数字为Q类仪表
我的修改：c#程序传入参数
p_data.yaml 数字类_type
跳过二三模块
设置Q类处理单元，包含头文件，新建ProcessNumA.h.cpp
(to_string((long long)yp)
pow(10.0F
testInfo.ImageFiles = "1|2|测试图片\\普通长\\DSC_5127-3.JPG";
测试图片的全路径
testInfo.TempletDir = "测试图片\\普????????????通";
testInfo.EquipNo = "DSC_5126"; //用测试参数编号表示模板图像的编号 
模板图片的文件夹，及路径
Content 联系整体的全局数据
enum GuardType 各模块类型
每大模块（由该模块守卫*Guard负责管理）
EnvValidityGuard环境合法性检查
SearchGuard仪表查找模块、对齐、读取配置文件
PreprocessingGuard
RecongnitionGuard图像识别模块
构造函数定义成了protected类型
纯虚函数
抽象类
ZeroMemory 分配const char*
轨道、lengsi？防爆
多路激光
硅胶与水反应
DSC_5630
透视变换与对齐分开
硅胶、雷击表、
gabor响应设置下限参数
代码性能热点如何计算
官伂 13657712
岳阳
段
WinFormTool2要上下左右控制四个点移动
掩模缺失
//YNUM类中不用读取_path了
