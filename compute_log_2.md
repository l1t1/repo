**计算2的非负整数次幂的次数的方法比较**  
在输出N王后的图形问题中，我遇到这个问题。如最靠右的位置是1，然后是2、4、8...，怎么把它变成0、1、2、3...呢？大致有如下几种办法：
1. 利用log(2,N)=log(e,N)/log(e,2)公式求2个自然对数的商。需要用round。 
2. 查表法，因为能确定输入就是那几个数，所以全列举出来查表，用switch case语句实现。
3. 循环比较法，其逻辑和查表法一样，用for语句实现。
4. 循环移位法，依次将输入右移，直到结果为0时结束循环。返回循环次数。
5. 利用C++11新引入的log2函数,不用round能返回正确值。速度比除以log(2)的算法快。
6. 利用C++的map，相当于二叉树。
7. 利用C++11的unordered_map，相当于散列表。  

用2个不同值分布的测试用例来分别测试。代码如下：   


```
#include <iostream>
#include <cmath>
#include <ctime>
#include <map>
#include <unordered_map>
int log2_div(int x)
{
return (int)round(log(x)/log(2));
}
int log2_while(int n){
int res = 0;
while((n>>=1))
    res++;
return res;
}
int log2_sys(int x)
{
return (int)log2(x);
}
int log2_case(int x)
{
switch(x){
case 1     : return 0 ;
case 2     : return 1 ;
case 4     : return 2 ;
case 8     : return 3 ;
case 16    : return 4 ;
case 32    : return 5 ;
case 64    : return 6 ;
case 128   : return 7 ;
case 256   : return 8 ;
case 512   : return 9 ;
case 1024  : return 10;
case 2048  : return 11;
case 4096  : return 12;
case 8192  : return 13;
case 16384 : return 14;
case 32768 : return 15;
case 65536 : return 16;
case 131072: return 17;
case 262144: return 18;
case 524288: return 19;
}
}

int log2_for(int x)
{
for(int i=0;i<20;i++)
 if(x==(1<<i))
 return i;
}

std::map<int,int> bmap;
int log2_map(int x)
{
 return bmap[x];
}

std::unordered_map<int,int> umap;
int log2_umap(int x)
{
 return umap[x];
}

void test(int (*f)(int x),int data[],int d_size,int range,const char msg[])
{
int t=clock(); 
long long sum=0LL;
for(int j=0;j<range;j++)
for(int i=0;i<d_size;i++)
{
 sum+=(*f)(data[i]);    
}
printf("%s\t%lld,%dms\n",msg,sum,clock()-t);
}

int main()
{
int a[20];

for(int i=0;i<20;i++)
{
 a[i]=1<<i;
 bmap[1<<i]=i;
 umap[1<<i]=i;
}

int b[1<<18]={1}; //更大的数组会溢出

for(int j=0;j<18;j++)
for(int i=1<<j;i<1<<(j+1);i++)
b[i]=1<<j;

for(int x=0;x<2;x++)
{
int *d=a;
int size=20;
int range=(1<< 20);
if(x==1)
{
d=b;size=1<<18;range=80;
}
printf("test case %d\n",x);

test(log2_div,d,size,range,"use divide log(2)");

test(log2_case,d,size,range,"use switch");

test(log2_for,d,size,range,"use for");

test(log2_sys,d,size,range,"use std::log2");

test(log2_while,d,size,range,"use while");

test(log2_map,d,size,range,"use map");

test(log2_umap,d,size,range,"use hash map");
}
return 0;
}
```
测试结果如下：
```
D:\>c++ power2a.cpp -O2

D:\>a
test case 0
use divide log(2)       199229440,2090ms
use switch      199229440,124ms
use for 199229440,406ms
use std::log2   199229440,1123ms
use while       199229440,218ms
use map 199229440,219ms
use hash map    199229440,405ms
test case 1
use divide log(2)       335544480,2091ms
use switch      335544480,93ms
use for 335544480,561ms
use std::log2   335544480,1061ms
use while       335544480,297ms
use map 335544480,218ms
use hash map    335544480,343ms

```
结论是：运行速度 查表法 > map或while > hash_map > for > log2> log对数除法。