# Google C++命名规范

## 一、简述

Google 的C++规范号称世界上最优秀的编码规范，于是拿来研究下，已改善自己以后的工作，今天只先学习下它的命名规范。

## 二、命名规则

### 1.文件命名规则

文件名**全部小写**，可以含下划线或连字符，按项目约定命名,且尽量保证文件名明确。比如：

cmd_save_player_info_class.cc                  my_use_full_class.cc 

定义类的文件名一般是成对出现，如：foo_bar.h   foo_bar.cc  

若是类中含大量内联函数，我们还可使用-ini.h文件，使之文件内容更加清晰，于是又如：

url_table.h     url_table.cc    url-table-ini.h

### 2.类命名规则
类型命名**每个单词首字母大写**，不含下划线，以**名词**形式。比如： MyPalyerManager，这对于所有类型命名一样，类结构体，枚举，类定义都是如此，比如：MyExcitingEnum

### 3.变量命名规则 
* 正常变量名**一律小写**，单词用下划线相连，例如：
int player_id;                      string table_name; 

* 特殊的是**类成员变量**，**后跟下划线**区别普通变量，比如： 
player_name_         player_id_

* 全局变量则以  g_  开头，比如 ：g_system_time

* 结构体成员变量还是和普通变量一样,比如：
string name;   int num_entries;

### 4.常量命名规则
**k后面跟大写字母开头**的单词，比如：
const int kDaysInAWeek=7;
const string kCompanyName="Tecent";

### 5.函数命名规则
常规函数**每个单词首字母大写**，使用命令式语气，比如：OpenFile()   CheckFileName()，

而**存取函数**或短小的内联函数使用**小写加下划线**，且与访问变量相吻合，比如 set_num_errors();

```C++
class Player{
 public:
   void set_player_id(const int player_id){return player_id_=player_id;}
   int get_player_id() const{return player-id_;}
 private:
   int palyer_id_;
};
```

### 6.名字空间命名
命名空间**全小写**，并基于项目名称和目录结构，比如
google_awesome_project

### 7.枚举命名规则
枚举类名属于类型名，按类命名，枚举值**全大写加下划线**，比如：ENUM_NAME

### 8.宏变量命名规则
如果你一定要用到宏，**全大写加下划线**，比如：
`#define PI_ROUND 3.0`

## 三、总结
总的来说，函数名、变量名、文件名都应该具有描述性，不要随意缩写，类型变量名保持名词性描述，函数名称保持命令性语气，宏、枚举值全大写加下划线，变量、文件、命名空间、存取函数全小写加下划线，其中类成员变量还要以下划线结尾，全局变量g_开头

————————————————
版权声明：本文为CSDN博主「云梦泽1989」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/u012333003/article/details/20282277