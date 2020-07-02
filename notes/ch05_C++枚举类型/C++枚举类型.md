# 枚举类型

枚举类型(enumeration)是C++中的一种派生数据类型，它是由用户定义的若干枚举常量的集合。

## 格式说明
创建枚举，需要使用关键字 **enum**。枚举类型的一般形式为：

```C++
enum 枚举名{ 
     标识符[=整型常数], 
     标识符[=整型常数], 
... 
    标识符[=整型常数]
} 枚举变量;
```
- 关键词**enum**：指明其后的标识符是一个**枚举类型的名字**
- 大括号内为枚举常量表，其本质是**以标识符形式表示的整型量**。

## 重要提示

- 枚举常量中整型常量可缺省，默认从0开始，依次加1。
- 可部分取值，在指定值之后依次加1原则
-  各枚举常量的值可以重复
- 枚举常量只能以标识符形式表示，而不能是整型、字符型等文字常量

举例：

```C++
enum fruit_set {apple, orange, banana=1, peach, grape}
//枚举常量apple=0,orange=1, banana=1,peach=2,grape=3。
```

```c++
enum week {Sun=7, Mon=1, Tue, Wed, Thu, Fri, Sat};
//枚举常量Sun,Mon,Tue,Wed,Thu,Fri,Sat的值分别为7、1、2、3、4、5、6。
```

```C++
enum letter_set {'a','d','F','s','T'}; //枚举常量不能是字符常量
enum year_set{2000,2001,2002,2003,2004,2005}; //枚举常量不能是整型常量
```

## 声明方式

- 枚举类型定义与变量声明分开：（即先声明枚举类，再利用枚举名声明变量）

```c++
enum Suit { Diamonds, Hearts, Clubs, Spades };
enum Suit a;
enum Suit b,c;
```

- 枚举类型定义与变量声明同时进行

```C++
enum Suit { Diamonds, Hearts, Clubs, Spades }a,b,c;
//此处类型名可以省略，如以下的声明也是可以的。
enum { Diamonds, Hearts, Clubs, Spades }a,b,c;
```

- 用typedef先将枚举类型定义为别名，再利用别名进行变量的声明

```C++
typedef enum Suit { Diamonds, Hearts, Clubs, Spades } newSuit;
enum Suit a;
Suit b;
newSuit c;
//enum newSuit会出现错误

typedef enum{ Diamonds, Hearts, Clubs, Spades }Suit;
Suit a;
//newSuit会出现错误

typedef enum Suit { Diamonds, Hearts, Clubs, Spades };
enum Suit a;
Suit b,c;
```

> 进行枚举变量声明时，枚举名可加可不加enum关键词，但别名不可加

## 相关操作

枚举变量的值只能取枚举常量表中所列的值，就是整型数的一个子集。

枚举变量占用内存的大小与整型数相同。

枚举变量只能参与**赋值**和**关系运算**以及**输出操作**，参与运算时用其本身的整数值。

例如：

设定枚举类：

```C++
enum color_set1 {RED, BLUE, WHITE, BLACK} color1, color2;
enum color_set2 { GREEN, RED, YELLOW, WHITE} color3, color4;
```

赋值操作：

```C++
color3=RED;           //将枚举常量值赋给枚举变量
color4=color3;        //相同类型的枚举变量赋值，color4的值为RED
int  i=color3;        //将枚举变量赋给整型变量，i的值为1
int  j=GREEN;         //将枚举常量赋给整型变量，j的值为0
```

关系运算：(==、<、>、<=、>=、!=等)

```C++
//比较同类型枚举变量color3，color4是否相等
if (color3==color4) cout<<"相等"；
//输出的是变量color3与WHITE的比较结果，结果为1
cout<< color3<WHITE;
```

输出：

```C++
cout<< color3;         //输出的是color3的整数值，即RED的整数值1
```

## 实例

口袋中有红、黄、蓝、白、黑五种颜色的球若干个，每次从口袋中取三个不同颜色的球，统计并输出所有的取法。

**思路：**穷举法三层循环，第一层循环取任意一个，第二层循环取一个（不包含第一层的），第三层循环取一个（不包含第一、第二层取的）

```C++
#include<iostream>
#include<iomanip>
using namespace std;
int main(){
    enum color_set {red,yellow,blue,white,black}; //声明枚举类型color
    color_set color; 
    int i,j,k,counter=0,loop; //counter是累计不同颜色的组合数
    for(i=red;i<=black;i++) {
        for(j=red;j<=black;j++) {
            if(i!=j){                        //前两个球颜色不同
                for(k=red;k<=black;k++)
                if(k!=i&&k!=j){        //第三个球不同于前两个，满足要求
                    counter++;
                    if((counter)%22==0){ //每屏显示22行
                        cout<<"请按回车键继续";
                        cin.get();
                    }
                    cout<<setw(15)<<counter;
                    /*下面输出每种取法，一行为一种取法的三个颜色*/
                    for(loop=1;loop<=3;loop++){
                        switch(loop){
                            case 1: color=(color_set) i; break;    //第一个是i
                            case 2: color=(color_set) j; break;    //第二个是j
                            case 3: color=(color_set) k; break;    //第三个是k
                        }
                        switch(color){
                            case red:   cout<<setw(15)<<"red";   break;
                            case yellow:cout<<setw(15)<<"yellow";break;
                            case blue:  cout<<setw(15)<<"blue";  break;
                            case white:    cout<<setw(15)<<"white"; break;
                            case black: cout<<setw(15)<<"black"; break;
                        }
                    }
                    cout<<endl;            //输出一种取法后换行
                }
            }
        }
    }
    cout<<"共有："<<counter<<"种取法"<<endl;
    return 0;
}
```

