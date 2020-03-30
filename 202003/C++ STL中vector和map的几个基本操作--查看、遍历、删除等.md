# C++ STL中vector和map的几个基本操作--查看、遍历、删除等

## vector    

* 头文件
```
#include<iostream>
#include<vector>
```

* 初始化 
 
```
int a[] ={1,2,3,4,5,4,6,4};
vector<int> vec(a,a+8);//利用数组初始化
vector<int> v1 = {1,2,3,4,5,6,7};  //要c++11，之前的版本可能不支持
vector<int> v2(3, 0); //创建一个容量为3，全部初始化0
vector<int> v3(5);  //创建容量为5，数据类型为int的vector
vector<int> v4(v3);  //创建一个从v3拷贝过来的vector

```  

* 遍历  

```
//直接遍历
for(int i=0;i<vec.size();i++){
    cout<<vec[i];
}
cout<<endl;
// 迭代器遍历
for(vector<int>::iterator it=vec.begin();it!=vec.end();it++){
    cout<<*it;
}
``` 

* 增加元素  

```
vec.push_back(7);//末尾插入
vec.insert(vec.begin()+2,100); //在第二个位置插入(vec.begin()代表首位的指针)
```

* 按位置查找（查看指定位置元素）  

```
cout<<*(vec.begin()+2)<<endl;
cout<<vec[2]<<endl;
```  

* 按值查找   

```
#include<algorithm> //需要引入头文件，find市algorithm里面的
vector<int>::iterator it = find(vec.begin(),vec.end(),4);
if(it!=vec.end()){ //!=vec.end() 说明还没走到末尾就找到这个元素，说明找到了，如果==end，则说明到了末尾还无，即没找到，这里找到的意思市找到了第一个，it的位置代表找到的第一个4
    cout<<"find it";
}
```  

* 修改指定位置（索引）元素

```
vec[2] =102;
*(vec.begin() +2) =101;
```  

* 按位置删除  
```
//删除指定位置元素
vec.erase(vec.begin()+1);
```    

* 按值删除  

```
//这个代码是删除所有是4的，如果删除第一个，则可以用find找到对应的it，然后vec.erase(it)即可
for(vector<int>::iterator it=vec.begin();it!=vec.end();){ //注意这个，因为it如果被删除则不能用++找下一个，所以it的更新在循环里
   if(*it==4){
       it = vec.erase(it); //如果被删除，则erase返回的就是下个元素的指针
   }else{
       it++; //没被删除，则直接++
   }
}
```  

## map

* 头文件  

```
#include<map>
```  

* 初始化  

```
map<string,int> mm; //构造一个空的
map<int, string> int_to_string = {
{1, "a"},
{2, "b"},
{3, "c"},
{4, "d"}}; //用初始化列表，适合数据多的
```  

* 插入  

```
mm.insert(pair<string,int>("a",1));
mm.insert(pair<string,int>("b",2));
```  

* 遍历  

```
// 迭代器遍历
for(map<string,int>::iterator it=mm.begin();it!=mm.end();it++){
    cout<<it->first<<":"<<it->second<<"  ";
}
// for each遍历
for(auto &it:mm){ //auto可以在声明变量时根据初始化表达式自动推断该变量的类型（你可以把其看作万能的）
    cout<<it.first<<":"<<it.second<<"  ";
}
```  

* 按key查找  
```
//查看元素 不存在则返回0
cout<<"a:"<<mm["a"];
```  

* 修改元素  

```
mm["a"] = 101;
```  

* 利用find查找-这种可以返回对应位置指针，方便删除  

```
map<string,int>::iterator it = mm.find("b");
if(it!=mm.end()){
    cout<<"find it  "<<it->first<<":"<<it->second<<endl;
}
```  

* 找到后删除（因为map中key都是唯一，所以不存在多个删除）  

```
if(it!=mm.end()){
    mm.erase(it);
}
```










