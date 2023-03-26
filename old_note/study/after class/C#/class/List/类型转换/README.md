//array-》list
static void Main(string[] args)
        {
            string s = @"D:\af";
            var dirInfo = new DirectoryInfo(s);
            FileInfo[] filInfo = dirInfo.GetFiles();
            List<FileInfo> sL = new List<FileInfo>(filInfo);
            foreach (FileInfo fi in sL)
            {
                Console.WriteLine(fi);
            }
        }
