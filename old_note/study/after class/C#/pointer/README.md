using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
namespace CSHARPTEST1
{
    class Program
    {
        static unsafe void swap(int* a, int* b)
        {
            int t = *a;
            *a = *b;
            *b = t;
            return;
        }
        static unsafe void Main(string[] args)
        {
            int x = 3;
            int y = 5;
            int* px = &x;
            int* py = &y;
            swap(px, py);
            Console.WriteLine(x);
        }
    }
}

