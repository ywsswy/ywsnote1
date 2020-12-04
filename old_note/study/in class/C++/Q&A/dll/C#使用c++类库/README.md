【详见C++创建托管程序集】
创建C#项目，
解决方案下的项目右键-添加-引用-浏览-dll
//////////////////////////////////////////////////////////////////////
//.cs
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using My0414clr1;//////////////////////////////////////////////////////////////////////
namespace DllTest
{
    class Program
    {
        static void Main(string[] args)
        {
            int v2 = MyDll.zhsf(3, 4);
            Class1 c1 = new Class1();//必要//必要
            Console.WriteLine(c1.ymul(3, 4).ToString());
            c1.print();
        }
    }
}
