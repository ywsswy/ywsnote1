var sourceFilePath = "D:/tmp/p.txt";
                var file = new FileInfo(sourceFilePath);
                var destFileName = "D:/" + file.Name;
//move
                File.Copy(sourceFilePath, destFileName);
