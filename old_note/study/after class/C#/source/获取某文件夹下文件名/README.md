//获取后缀名、文件夹名、文件名
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
namespace getfilename
{
    class Program
    {
        //1:only show suffix
        //2:only show foldername
        //4:only show filename
        //fix:1+2+4
        static int flag = 7;
        static void showFileExt(string s,List<FileInfo> sL)
        {
            var dirInfo = new DirectoryInfo(s);
            FileInfo[] filInfo = dirInfo.GetFiles();
            foreach(var i in filInfo){
                sL.Add(i);
            }
            foreach (FileInfo fi in sL)
            {
                //Console.WriteLine(fi);
            }
            DirectoryInfo[] dir1 = dirInfo.GetDirectories();
            for (var i = 0; i < dir1.Length; i++)
            {
                string dir = s+@"\"+dir1[i].ToString();
                if ((flag & 2) != 0)
                {
                    Console.WriteLine(dir);
                }
                showFileExt(dir,sL);
            }
        }
        static void Main(string[] args)
        {
            if (args.Length <= 2)
            {
                try
                {
                    string s;
                    if (args.Length == 2)
                    {
                        flag = int.Parse(args[1]);
                        s = args[0];
                        if (s == @"\")
                        {
                            s = Directory.GetCurrentDirectory();
                        }
                    }
                    if (args.Length == 0)
                    {
                        s = Directory.GetCurrentDirectory();               
                    }
                    else
                    {
                        s = args[0];
                        if (s == @"\")
                        {
                            s = Directory.GetCurrentDirectory();  
                        }
                    }
                    var dirInfo = new DirectoryInfo(s);
                    FileInfo[] filInfo = dirInfo.GetFiles();
                    List<FileInfo> sL = new List<FileInfo>();
                    if ((flag & 2) != 0)
                    {
                        Console.WriteLine("");
                        Console.WriteLine("FOLDER NAME：");
                        Console.WriteLine(s);
                    }
                    showFileExt(s, sL);
                    FileInfo[] filArr = sL.ToArray();
                    if ((flag & 4) != 0)
                    {
                        Console.WriteLine("");
                        Console.WriteLine("FILE NAME：");
                        var filGroup2 = filArr.GroupBy(f => f.FullName);
                        foreach (var i in filGroup2)
                        {
                            Console.WriteLine(i.Key);
                        }
                    }
                    if ((flag & 1) != 0)
                    {
                        Console.WriteLine("");
                        Console.WriteLine("SUFFIX NAME：");
                        var filGroup = filArr.GroupBy(f => f.Extension);
                        foreach (var i in filGroup)
                        {
                            Console.WriteLine(i.Key);
                        }
                    }
                }
                catch (Exception e)
                {
                    Console.WriteLine(e);
                }
            }
            else
            {
                Console.WriteLine("参数个数错误");
            }
        }
    }
}

