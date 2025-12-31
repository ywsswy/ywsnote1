关于map 的insert返回值是个std::pair<iterator, bool> # 如果已经设置了，就是false。

# 
map 和 vector/list
实刻保持有序用map，否则用vector/list
vector 和 list
有头插，头出，用list，大小未定且能屈能伸用list，否则用vector
随机读取用vector
随机插入用map

# constructor
std::vector<int> ve(size,value);
# stl function
std::swap()  
# object.function() //object:map,vector,list(,string)  
clear  
empty  
size	//string also can use length  
swap    // a.swap(b) is same b.swap(a)
begin   //::iterator ; ++ to end()，可以通过迭代器修改值（map的it->second）;除了vector<>::iterator外，其他的没有 +/- +=/-=运算符  
end	//end is the lastloc's next loc  
rbegin	//::reverse_iterator ; ++ to rend()  
rend  
## map
[]/at	//按键返回值引用 //这俩不一样，[]如果没有的话，会创建一个key-0 
find	//按键 返回迭代器  
erase	//按迭代器删除，返回指向下一个元素的迭代器(C++11中才支持,不然返回void)，反迭不行  
## vector
[]/at	//按下标返回值引用  
back	//引用  
front	//引用  
pop_back //void return  
push_back  
emplace_back  // 相比之下可以接收参数进行原地构造，避免前者只能接收本类对象进行复制/移动构造
erase	//按迭代器删除，返回指向下一个元素的迭代器，反迭不行  
insert(it,elem)	//插入元素排在前面，返回插入元素的迭代器，反迭不行  
reserve	//提前分配好capacity（不改变size），因为连续内存不够会重新分配导致移动重新构造元素。  
resize	//resize(size_int,value) 改变size，多加的元素初始值为value（类似memset），未写value会填充默认值，如果是从多减少，前几个值是不变的； //二维数组初始化也用这个
## list
back	//左值引用，不会core  
front	//引用，但是要看怎么传出来，int a = q.front()不同于int &a = q.front()  
pop_front //void return  
pop_back  //会core
push_front  
push_back  
remove	//按值删除（不存在不会报错）  
reverse	//反序排列  
sort	//默认使用less<int>()升序排序，自建类要重载，注意不能用std::sort()，因为std::sort要求容器的迭代器是【随机迭代器】，std::make_heap也是一样的要求，且默认也是less<int>()
unique	//按值移除【相邻的（所以用之前应该先排序）】重复元素  
erase	//按迭代器删除，返回指向下一个元素的迭代器，反迭不行  
insert(it,elem)	//插入元素排在前面，返回插入元素的迭代器，反迭不行  
# others
## 1
```
class Y{
  bool operator<(const Y& y){//类内重载的方法
  bool operator() (const Y& yc1, const Y& yc2）{//类内自定义函数，可实现比较函数
bool operator<(const Y &yc1, const Y &yc2){//类外重载的方法
bool mycompare(const Y &yc1, const Y &yc2){//类外自定义比较函数的方法（使用时sort(.begin(), .end(), mycompare)），即第三个参数是个比较函数的地址

return false时会移动元素
yc1/this/比较时前面的元素
yc2/y/比较时后面的元素
//一次list.sort()中两个元素在最多比较一次，但priority_queue（默认大根堆top/pop操作最大的）和std::sort(vec1.begin(),vec1.end())中可能比较大于1次，所以可能大于1次的比较函数不要写出歧义
```
class InvertedNode {
 public:
  InvertedNode() {}
  bool operator<(const InvertedNode& b) const {
    if (this.score_ > b.score_) {
      return true;
    }
    return false;
  }
  double score_;
};
// 这个例子中，InvertedNode类内的比较函数表明，当this.score值更大的时候，后面的元素值更大的时候，会return true，会交换二者，那最终排序的话就等价于按照score降序
```
```
## 2
```
ostream & operator <<(ostream &os,const ycedge &yc){
os << "my:" << yc.x <<; return os
```
## 3
vector<bool>不是容器，时间效率也差，不要用
## 4 erase的正确用法(list,vector,map(c++11)）
```
for(list<yc>::iterator it = li.begin();it != li.end();){//注意这里
        if(*it == yc(2,3)){
            it = li.erase(it);//注意这里
        } else{
            it++;
        }
    }
for (; i < size; i++) {//old map
            map<int,int>::iterator itback = itm;
            itback++;
            ma.erase(itm);
            itm = itback;
```
## 5 insert的正确用法(list,vector)
```
for(list<int>::iterator it = li.begin();it != li.end();it++){
        if(*it == 3){
            it = li.insert(it,-3);
            it++;
        } 
    }
```

std::lower_bound
https://blog.csdn.net/albertsh/article/details/106976688

erase支持传两个参数，start, end，表示删除一个区间段的元素；
配合std::remove_if可以实现先把符合条件（return true）的所有元素移动到尾部，返回“原”尾部迭代器，进而实现删除所有符合条件的元素
x.erase(std::remove_if(x.begin(), x.end(), []{}), x.end());