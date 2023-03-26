var dirInfo = new DirectoryInfo(dir);
var files = dirInfo.GetFiles().GroupBy(f => f.Extension);
            foreach (var group in files)
            {
                //var size = group.Sum(f => f.Length);
                //var count = group.Count();
                //Console.WriteLine(string.Format("扩展名：{0}\t大小：{1}bytes\t文件数量：{2}", group.Key, size, count));
                Console.WriteLine(string.Format("{0}", group.Key));
            }
//getfiles return FileInfo[]
//GroupBy return 遍历器（即可foreach）
//IEnumerable<IGrouping<string, Student>> studentGroup = studentList.GroupBy(s => s.ClassName);
//

