#include <dirent.h>//linux下
/*
struct dirent
{
unsigned char d_type;
char d_name[256];
};
*/

DIR *dir;
struct dirent *dir_data;
if ((dir = opendir(path.data())) != NULL)//path不能以~/开头，绝对相对路径都可以，结尾可以是'/'
{
  std::map<std::string, int> info;
  while ((dir_data = readdir(dir)) != NULL) //循环获取一个个文件，'.'和'..'也会被获取到
  {
     //判断dir_data->d_type，DT_DIR/4/目录，DT_LNK/10/链接，DT_REG/8/普通文件
     //dir_data->d_name，文件名