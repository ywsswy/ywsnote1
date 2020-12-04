【c#使用c++导出函数】
新建C#项目pro1
solu1.dll放入bin/debug下
新建yws.cs
//////////////////////////////////////////////////////////////////////
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Runtime.InteropServices;//必需
namespace pro1
{
    class yws
    {
        [DllImport("solu1.dll")]
        public static extern int ywsf(int a,int b);
//只能导入方法
//每个方法前都要写DllImport
    }
}
//////////////////////////////////////////////////////////////////////
Program.cs
//////////////////////////////////////////////////////////////////////
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
namespace pro1
{
    class Program
    {
        static void Main(string[] args)
        {
            int a = yws.ywsf(3, 5);
            Console.WriteLine(a.ToString());
        }
    }
}
//////////////////////////////////////////////////////////////////////
