# LAB 11

> 本次LAB目标：
>
> 1. 回顾传值和传值针函数
> 2. struct基础应用练习

## 获取及提交Lab

**获取**：通过 `https://github.com/fdu-20ss-programming-A/lab11或者`超星平台`获取。

**提交**：超星平台上已经发布了LAB11作业，同学需要将每题的**代码**、**运行结果**、个人对题目的思考体会（可选）以**图片**或者**文档**（ word 或者 pdf ）的形式在超星LAB11对应的作业区域提交即可。

**提交物**：代码、运行结果，每题可整理为一份 word 或者 pdf 文档，也可以将两者截在一张图片上提交。

**截止时间**：2020年12月22日 23:59:59



## LAB 10 作业

### Question1

> 分数可以表示为“分子/分母”的形式。编写一个程序，要求用户输入一个分数，然后将其约分为最简分式。
>
> 最简分式是指分子和分母不具有可以约分的成分了。如6/12可以被约分为1/2。当分子大于分母时，不需要表达为整数又分数的形式，即11/8还是11/8；而当分子分母相等时，仍然表达为1/1的分数形式。
>
> 该程序中包含两个函数void reduce(int *a, int *b)，int * reduce(int a,int b)
>
> 一个传入的参数是两个指针，直接修改指针的值作为返回值，另一个传入的参数是两个值，返回的是包含两个值的一维数组
>
> 第二个函数直接声明int result[2]，然后返回这个result不会得到正确结果，可以
>
> 1. 在函数内声明static int result[2]
> 2. 声明result[2]为全局变量
> 3. int * result; result = (int *)malloc(2);
>
> 原因可见于 https://blog.csdn.net/gavechan/article/details/45542913

限制

> 输入都是正整数

示例

> 120/30=4/1
> 2/3=2/3
> 3/0=NaN

**参考答案**

```C
#include <stdio.h>

void reduce1(int * a, int * b){
    int x = *a;
    int y = *b;
    int t;
    while (y != 0){
        t = x % y;
        x = y;
        y = t;
    }
    *a = *a / x;
    *b = *b / x;
}

int * reduce2(int a, int b){
    static int result[2];
    int origina = a;
    int originb = b;
    int t;
    while (b != 0){
        t = a % b;
        a = b;
        b = t;
    }
    result[0] = origina / a;
    result[1] = originb / a;
    return result;
}

int main(int argc, char *argv[]) {
    int a, b;
    scanf("%d/%d", &a, &b);

    if (a==b) {
        printf("%d/%d=1/1\n", a, b);
    } else if (b == 0){
        printf("%d/%d=NaN\n", a, b);
    } else{
        int * result;
        //先测reduce2，不会改变a、b的值
        result = reduce2(a, b);
        printf("%d/%d=%d/%d\n", a, b, result[0], result[1]);
        printf("%d/%d=", a, b);
        reduce1(&a, &b);
        printf("%d/%d", a, b);
    }

    return 0;
}
```

**补充：**

1. 局部变量使用static关键字，变量会存放在内存中的静态存储区中，所占用的存储单元不释放直到整个整个程序运行结束。
2. malloc分配所需的内存空间，并返回一个指向它的指针。给指针分配的空间在堆上，堆上的空间是由程序员自动给予分配和释放的。若程序员不释放,程序结束时可能由OS释放。需要free指针。
3. 在所有函数外部定义的变量称为**全局变量（Global Variable）**，它的作用域默认是整个程序

## Struct相关知识

> 本节我们将介绍 C 结构体相关知识。

### 定义结构

struct 语句的格式如下：

```c
struct Books
{
   char  title[50];
   char  author[50];
   char  subject[100];
   int   book_id;
} book;


 
//也可以用typedef创建新类型
typedef struct
{
    int a;
    char b;
    double c; 
} Simple2;
```

### 结构体变量的初始化

结构体变量可以在定义时指定初始值。

```c
#include <stdio.h>
 
struct Books
{
   char  title[50];
   char  author[50];
   char  subject[100];
   int   book_id;
} book = {"C 语言", "RUNOOB", "编程语言", 123456};
```

### 访问结构成员

我们使用**成员访问运算符（.）**访问结构的成员.

```C
struct Books Book1;        /* 声明 Book1，类型为 Books */
 
 /* Book1 详述 */
strcpy( Book1.title, "C Programming");
strcpy( Book1.author, "Nuha Ali"); 
strcpy( Book1.subject, "C Programming Tutorial");
Book1.book_id = 6495407;
```

### 指向结构的指针

```C
struct Books *struct_pointer;
struct_pointer = &Book1;
struct_pointer->title;
```



## 上机作业1

### Question1

> 某校评选优秀班级，共有五项得分，打分标准如下：
>
> 1) 班级成绩：期末平均成绩高于90分（>90）得20分，小于等于90大于80得15分，小于等于80得10分；
>
> 2) 班级卫生：日常班级卫生评议成绩高于80分（>80）的学生得20分，小于等于80大于60得15分，小于等于60得10分；
>
> 3) 班级风气：每个班级默认得满分20分，班级中每有一名纪律处分学生扣2分；
>
> 4) 班级荣誉：班级期末平均成绩大于80分，体育比赛中获得前3名的班级，可额外获得5分；
>
> 5) 额外奖励：日常班级卫生评议高于90分（>90）、班级中不存在纪律处分学生，可额外获得5分；
>
> 例如A班的期末平均成绩是87分，班级卫生评议成绩82分，班里有一名受处分同学，体育比赛获得第5名，那么A班的成绩是53分。
>
> 请设计合适的结构体class并编写程序能够计算哪些班级获得的分数最高。
>
> 注意：请编写函数名为FindMax的函数，输入为结构体数组classes，并有返回，返回值可格式自行规定。函数需要用以下提供的数据测试。
>
> 提供数据：
>
> A班：期末平均成绩是87分，班级卫生评议成绩82分，班里有一名受处分同学，体育比赛获得第5名
>
> B班：期末平均成绩是98分，班级卫生评议成绩82分，班里有两名受处分同学，体育比赛获得第3名
>
> C班：期末平均成绩是67分，班级卫生评议成绩95分，班里没有受处分同学，体育比赛获得第4名
>
> D班：期末平均成绩是86分，班级卫生评议成绩78分，班里没有受处分同学，体育比赛获得第2名
>
> E班：期末平均成绩是86分，班级卫生评议成绩54分，班里没有受处分同学，体育比赛获得第1名
>
> F班：期末平均成绩是78分，班级卫生评议成绩92分，班里有三处分同学，体育比赛获得第6名

注意：

> 输入都是正整数



### Question2

> 使用合适结构体clock表示时钟
>
> 请编写程序实现从键盘任意输入两个时间（例如4时55分和1时25分），可以计算并输出这两个时间之间的间隔。要求不输出时间差的负号。
>
> 函数原型： void CalculateTime(CLOCK *t1, CLOCK *t2);
>
> 注：时间输入格式为HH:MM，范围是0:00 ~ 23:59

示例：

> 请输入时间：4:55    1:55
>
> 时间间隔为：3h0min

