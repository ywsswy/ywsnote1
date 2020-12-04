using System;
using System.Collections.Generic;
using System.Linq;
using System.Runtime.InteropServices;
using System.Text;
using System.Threading.Tasks;
namespace RobotMonitor.Support
{
    //识别测试项信息结构体
    public struct TEST_ITEM_INFO
    {
        public string TaskNo;//任务号
        public string StationNo;//站点编号
        public string TestItemNo;//测试项编号
        public string TempletDir;//模板图像文件目录，模板图像命名规则：设备编号+拍照编号+“.jpg”，例如：E0001-1-1.jpg,其中最后一个数字是拍照的编号
        public string ImageFiles;//待识别的所有图像的文件名，即所以的文件名在一个字符串，文件名直接用字符“|”作为分割符。
        //待识别的图像命名规则是“任务号-站点编号-测试项编号-拍照时间-拍照序号”，图像格式为jpg，例：“T000000001-3-2-20170210155519-1.jpg”,表示为T000000001任务，第3个站点，第2个测试项的第一张照片，拍摄时间为2017年2月10日15:55:19(三张照片拍摄时间设为同一时间)
        public string EquipNo; //待识别的设备编号
        public string TestType; //待识别的参数类型,即测试项名称
        public float Focus; ////焦距，此处的焦距是第1张图片的焦距倍数，
        //另外两张的焦距倍数根据该焦距和后面的焦距增量来计算。
        //例：Focus = 20, ZoomIncrement1=-10，ZoomIncrement2=5，则三张照片的焦距倍数分别是 20，（20-10）, （20+5）
        public float ZoomIncrement1;	//焦距增量1
        public float ZoomIncrement2;	//焦距增量2
        [MarshalAsAttribute(UnmanagedType.ByValArray, SizeConst = 24, ArraySubType = UnmanagedType.I1)]
        public byte[] Reversed;//保留字节
    };
    //返回结果
    public struct RECONG_INFO
    {
        [MarshalAsAttribute(UnmanagedType.ByValArray, SizeConst = 12, ArraySubType = UnmanagedType.U1)]
        public char[] TaskNo;//任务号
        [MarshalAsAttribute(UnmanagedType.ByValArray, SizeConst = 12, ArraySubType = UnmanagedType.U1)]
        public char[] StationNo;//站点编号
        [MarshalAsAttribute(UnmanagedType.ByValArray, SizeConst = 12, ArraySubType = UnmanagedType.U1)]
        public char[] TestItemNo;//测试项编号
        [MarshalAsAttribute(UnmanagedType.ByValArray, SizeConst = 80, ArraySubType = UnmanagedType.U1)]
        public char[] TestType;//待识别的参数类型
        public float TestValue;//识别结果
        public float TuningFocus;	//焦距微调参数         //预留
        public float TuningRotation;	//云台旋转微调参数     //目前传x值
        public float TuningPitch;	//云台俯仰微调参数      //目前传y值
        public int RecongState; //识别结果状态 0-识别成功，非0返回识别错误号（错误码后续补充）
        [MarshalAsAttribute(UnmanagedType.ByValArray, SizeConst = 12, ArraySubType = UnmanagedType.I1)]
        public char[] Reserved; //预留
    };
    class ImageRecong
    {
        [DllImport(@"ImageRecongnition.dll")]
        public static extern void ImageRecongnition(TEST_ITEM_INFO TestItemInfo, float[] Parameters,
                                int ParamNum, bool bReturnParam, ref RECONG_INFO RecongInfo);
    }
}

