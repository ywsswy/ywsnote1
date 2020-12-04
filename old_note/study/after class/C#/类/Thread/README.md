using namespace System.Threading.Tasks;
private static Thread t1, t2, t3;
t1 = new Thread(new ThreadStart(Method1));
t1.Start();
public static void Method1()
        {
            Console.WriteLine("LINE1 start");
            Thread.Sleep(3333);
            for (int i = 0; i < 100; i++)
            {
                Console.WriteLine("LINE1:" + i.ToString());
            }
            Console.WriteLine("LINE1 end");
        }
