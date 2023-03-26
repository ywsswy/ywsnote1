using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
namespace fileWatch
{
    class Program
    {
        static FileSystemWatcher watch = new FileSystemWatcher();
        static void Main(string[] args)
        {
            watch.Path = @"D:\tmp";
            watch.NotifyFilter = NotifyFilters.LastAccess | NotifyFilters.LastWrite | NotifyFilters.FileName | NotifyFilters.DirectoryName;
            // Only watch text files.
            watch.Filter = "*.txt";
            //watch.Changed += new FileSystemEventHandler(OnChanged);
            watch.Created += new FileSystemEventHandler(OnChanged);
            //watch.Deleted += new FileSystemEventHandler(OnChanged);
            //watch.Renamed += new RenamedEventHandler(OnRenamed);
            Console.WriteLine("Press any key to end watch.");
            watch.EnableRaisingEvents = true;
            Console.ReadLine();
        }
        private static void OnChanged(object source, FileSystemEventArgs e)
        {
            // Specify what is done when a file is changed, created, or deleted.
            if (e.FullPath == @"D:\tmp\p.txt")
            {
                Console.WriteLine("File: " + e.FullPath + " " + e.ChangeType);
                watch.EnableRaisingEvents = false;
            }
        }
//         private static void OnRenamed(object source, RenamedEventArgs e)
//         {
//             // Specify what is done when a file is renamed.
//             if (e.FullPath == @"D:\tmp\p.txt")
//                 Console.WriteLine("File: {0} renamed to {1}", e.OldFullPath, e.FullPath);
//         }
    }
}
