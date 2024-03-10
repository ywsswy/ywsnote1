进程！=线程
线程：支线，某线程的附属，不影响其他线程阻塞。
进程：主线，可阻塞。
using System.Threading;
Process p = Process.Start("D:\\af\\formulline.exe");
try
            {
                ProcessStartInfo info = new ProcessStartInfo("D:\\af\\formulline.exe");
                // ProcessStartInfo info = new ProcessStartInfo(可执行文件路径,传递参数);
//不新建exe窗口状态《==》将info的输出定向到当前程序
                info.UseShellExecute = false;
                
                //info.WindowStyle = ProcessWindowStyle.Hidden;
                
//运行exe
                Process proBach = Process.Start(info);
                // 取得exe的返回值，返回值只能是整型
                proBach.WaitForExit();
                int returnValue = proBach.ExitCode;
                Console.WriteLine("!!!!!!!!!!!!!!!!!!!!:"+returnValue.ToString());
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex);
            }
