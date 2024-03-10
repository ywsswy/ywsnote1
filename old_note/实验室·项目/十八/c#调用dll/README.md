//.dll放在bin/Debug下，
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;
using System.IO;
using System.Threading;
using System.Diagnostics;
namespace calltb1
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
            watch.Path = @"F:\tmp";
            watch.NotifyFilter = NotifyFilters.LastAccess | NotifyFilters.LastWrite | NotifyFilters.FileName | NotifyFilters.DirectoryName;
            //watch.Filter = "*.txt";
            watch.Created += new FileSystemEventHandler(OnChanged);
            textBox1.Text += "ready.";
            watch.EnableRaisingEvents = true;
        }
        private void OnChanged(object source, FileSystemEventArgs e)
        {
            Thread.Sleep(1111);
            try
            {
                textBox1.Text += "\r\nFile: " + e.FullPath + " " + e.ChangeType;
                textBox1.Text += "\r\nstart";
                //Thread.Sleep(5333);//延时
                var file = new FileInfo(e.FullPath);
                var destFileName = "F:/" + file.Name;
                File.Copy(e.FullPath, destFileName, true);
                textBox1.Text += "\r\nFile: " + e.FullPath + "Have moved to D:.";
                File.Delete(e.FullPath);
                textBox1.Text += "\r\nFile: " + e.FullPath + "Have deleted.";
            }
            catch(Exception exc){
                textBox1.Text = exc.ToString();
            }
        }
        public void Method2()
        {
            //yws.ysleep(9999);
        }
        public void Method1()
        {
            textBox1.Text += "\r\nLINE1 start"; try
            {
                //ProcessStartInfo info = new ProcessStartInfo("D:\\af\\formulline.exe");
                //ProcessStartInfo info = new ProcessStartInfo("F:\\tmp\\Test.exe");
                ProcessStartInfo info = new ProcessStartInfo("F:\\projects\\cpp\\VCforMull\\Debug\\VCforMull.exe");
                // ProcessStartInfo info = new ProcessStartInfo(strExePath,传给EXE 的参数);
                info.UseShellExecute = false;
                //隐藏exe窗口状态
                info.WindowStyle = ProcessWindowStyle.Hidden;
                //运行exe
                Process proBach = Process.Start(info);
                // 取得EXE运行后的返回值，返回值只能是整型
                proBach.WaitForExit();
                int returnValue = proBach.ExitCode;
                textBox1.Text += "!!!!!!!!!!!!!!!!!!!!:" + returnValue.ToString();
            }
            catch (Exception ex)
            {
                textBox1.Text = ex.ToString();
            }
            textBox1.Text += "\r\nLINE1 end";
        }
        public void Method3()
        {
            textBox1.Text += "\r\nLINE1 start"; try
            {
                //ProcessStartInfo info = new ProcessStartInfo("D:\\af\\formulline.exe");
                //ProcessStartInfo info = new ProcessStartInfo("F:\\tmp\\Test.exe");
                //ProcessStartInfo info = new ProcessStartInfo("F:\\projects\\cpp\\VCforMull\\Debug\\VCforMull.exe");
                // ProcessStartInfo info = new ProcessStartInfo(strExePath,传给EXE 的参数);
                //info.UseShellExecute = false;
                //隐藏exe窗口状态
                //info.WindowStyle = ProcessWindowStyle.Hidden;
                //运行exe
                //Process proBach = Process.Start(info);
                // 取得EXE运行后的返回值，返回值只能是整型
                //proBach.WaitForExit();
                //int returnValue = proBach.ExitCode;
                int returnValue = yws.ywsf(3, 5);
                textBox1.Text += "!!!!!!!!!!!!!!!!!!!!:" + returnValue.ToString();
            }
            catch (Exception ex)
            {
                textBox1.Text = ex.ToString();
            }
            textBox1.Text += "\r\nLINE1 end";
        }
        private void button1_Click(object sender, EventArgs e)
        {
            t1 = new Thread(new ThreadStart(Method1));
            t1.Start();
        }
        private void button2_Click(object sender, EventArgs e)
        {
            t2 = new Thread(new ThreadStart(Method3));
            t2.Start();
        }
        static FileSystemWatcher watch = new FileSystemWatcher();
        private static Thread t1,t2;
    }
}

