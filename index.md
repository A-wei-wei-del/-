<img src="C:\Users\AAA\AppData\Roaming\Typora\typora-user-images\image-20210106191927727.png" alt="image-20210106191927727" style="zoom:200%;" />

<div STYLE="page-break-after: always;"></div>

[TOC]

<div STYLE="page-break-after: always;"></div>

# 学生信息管理软件设计思路 #

## 1.文字描述:

1. 用c语言设计一款具有简单到通讯录功能的软件,利用结构体变量完成对数据的储存和录入.

2. 功能包括通讯录个人信息(学号,姓名,性别,电话号码,地址)的录入,增加,查找,修改,删除的基本功能.


## 2.图示:

<img src="C:\Users\AAA\AppData\Roaming\Typora\typora-user-images\image-20210106120632059.png" alt="image-20210106120632059" style="zoom:67%;" />

# 部分程序流程图:

## 1.基于冒泡排序法对学号排序的N-S图

<img src="C:\Users\AAA\AppData\Roaming\Typora\typora-user-images\image-20210106044508538.png" alt="image-20210106044508538" style="zoom:50%;" />

- **文字描述**:

1. 通过外层循环控制循环次数
2. 内层循环让相邻学号之间比较大小,如果前面的学号大于后面的学号则互换位置

## 2.利用fread从TXT文档中读取文件到结构体数组的N-S图

<img src="C:\Users\AAA\AppData\Roaming\Typora\typora-user-images\image-20210106111533922.png" alt="image-20210106111533922" style="zoom:50%;" />

- **文字描述**:

1. 以读的方式打开文件,若文件不存在则以写的方式创建一个文件.
2. fread函数循环读取问价内容到结构体数组





# 学生信息管理软件关键技术点

## 1.全局变量的定义

- 将n(结构体数组的个数)以及fp定义为全局变量,减少指针在各个函数中传递的书写.


## 2.排序方法


- 利用**冒泡排序法**对学号的排序:

1. 第一次比较：首先比较第一和第二个数，将小数放在前面，将大数放在后面。
2. 比较第2和第3个数，将小数 放在前面，大数放在后面。
3. ......
4. 如此继续，知道比较到最后的两个数，将小数放在前面，大数放在后面，重复步骤，直至全部排序完成
5. 在上面一趟比较完成后，最后一个数一定是数组中最大的一个数，所以在比较第二趟的时候，最后一个数是不参加比较的。
6. )在第二趟比较完成后，倒数第二个数也一定是数组中倒数第二大数，所以在第三趟的比较中，最后两个数是不参与比较的。
7. 依次类推，每一趟比较次数减少依次

- 利用**选择排序法**对学号进行排序:

1. 首先在未排序序列中找到最小（大）元素，存放到排序序列的起始位置
2. 再从剩余未排序元素中继续寻找最小（大）元素，然后放到已排序序列的末尾。
3. 重复第二步，直到所有元素均排序完毕。

## 3.查找信息.读取信息.保存信息所需要函数

1. 查找信息用到了**strcmp**以及**strstr**两个函数,关键点在于一个strcmp可以实现对内容的精确查找,而**strstr**可以对内容进行模糊查找.
2. 读取信息用到了**fread**以及**sizeof**:
3. 通过sizeof判断一个结构体所占的字节个数,将他以所占字节从TXT文档中读取内容或者写入内容.
4. fread则是在TXT文档中读取内容到结构体当中.
5. 保存信息用到了**fwrited**实现写入TXT文件.


## 4.witch-case结合getch的使用

- 实现了对用户键入的数字用getch读取再利用swith-case进行选择,实现功能模块.






# 程序源代码以及注释

```c
#include <stdio.h>
#include <Windows.h>
#include <string.h>
//定义结构体
typedef struct
{
    char id[50];
    char name[50];
    char sex[50];
    char number[50];
    char address[50];
} V;
V stu[100];
FILE *fp;  //定义全局指针
int i = 0; //定义全局变量,记录人数

//输入函数
void input()
{
    system("cls");
    printf("学号:\n");
    scanf("%s", stu[i].id);
    printf("姓名:\n");
    scanf("%s", stu[i].name);
    printf("性别:\n");
    scanf("%s", stu[i].sex);
    printf("电话:\n");
    scanf("%s", stu[i].number);
    printf("地址:\n");
    scanf("%s", stu[i].address);
    i++; //人数加一
    printf("输入y继续");
    if (getch() == 'y')
    {
        input(); //输入y继续调用input函数
    }
}

//读取信息函数
void readflies()
{
    if (fp = fopen("xs.txt", "r") == NULL) //判断文件是否存在
    {
        printf("不存在文件,已经为您创建");
        fp = fopen("xs.txt", "w");
    }
    else
    {
        fclose(fp);
        fp = fopen("xs.txt", "r");
        for (i = 0; !feof(fp) && fread(&stu[i], sizeof(V), 1, fp); i++)
            ; //feof判断末尾  sizeof确定每次读取输入的大小读取到结构体数组
    }
    fclose(fp);
}

//打印内容 函数
void myprintf()
{
    system("cls");
    for (int t = 0; t < i; t++)
    {
        printf("%s %s %s ", stu[t].id, stu[t].name, stu[t].sex); 
        printf("%s %s\n", stu[t].number, stu[t].address)//循环打印每一位成员
    }
    getch();
}

//保存函数
void mysave()
{
    system("cls");
    fp = fopen("xs.txt", "w"); //以写的方式打开磁盘
    for (int t = 0; t < i; t++)
    {
        fwrite(&stu[t], sizeof(V), 1, fp); //以sizeof字节写入磁盘当中
    }
    fclose(fp);
    printf("已保存");
}

//全部清除信息函数
void alldelete()
{
    fp = fopen("xs.txt", "w"); //以写的方式打开磁盘,覆盖全部内容
    fclose(fp);
    i = 0;
}

//查找学生信息函数
int mysearch()
{
    char c;
    system("cls");
    printf("\n1-按姓名查询");
    printf("\n2-按学号查询");
    c = getch();
    switch (c)
    {
    case '1':
        chazhao_xingming();
        break; //调用按姓名查询函数
    case '2':
        chazhao_xuehao();
        break; //调用按学号查询函数
    }
}
void chazhao_xingming()
{
    int mark = 0;
    int t;
    char name[20];
    printf("\n请输入您要查找的姓名:");
    scanf("%s", name);
    for (t = 0; t < i; t++)
    {
        if (strcmp(stu[t].name, name) == 0)
        {
            printf("%s %s %s ", stu[t].id, stu[t].name, stu[t].sex); 
            printf("%s %s\n", stu[t].number, stu[t].address)
            mark++;
            getch();
            return;
        }
    }
    if (mark == 0)
    {
        printf("\n没有找到学生信息");
        printf("\n按任意键返回主菜单");
        getch();
        return;
    }
}
void chazhao_xuehao()
{
    int mark = 0;
    int t;
    char xuehao[20];
    printf("\n请输入您要查找的学生的学号:");
    scanf("%s", xuehao); //用户从键盘输入要查询的内容
    for (t = 0; t < i; t++)
    {
        if (strcmp(stu[t].id, xuehao) == 0) //strcmp进行字符串比对
        {
            printf("%s %s %s ", stu[t].id, stu[t].name, stu[t].sex); 
            printf("%s %s\n", stu[t].number, stu[t].address)//打印查找到的内容
            mark++;                                                                                        //记数
            getch();
            return;
        }
    }
    if (mark == 0)
    {
        printf("\n没有找到学生信息");
        printf("\n按任意键返回主菜单");
        getch();
        return;
    }
}

//部分删除函数
void deletepart()
{
    char c;
    if (i == 0) //判断信息是否存在
    {
        printf("\n对不起,文中无任何记录");
        printf("\n按任意键返回主菜单");
        getch();
        return;
    }
    else
    {
        system("cls");
        printf("\n1-按姓名删除\n2-按电话删除");
        printf("\n3-按指定末尾学号删除信息");
        c = getch();
        switch (c)
        {
        case '1':
            shanchu_xingming(); //分别调用各自函数
            break;
        case '2':
            shanchu_dianhua();
            break;
        case '3':
            shanchu_xuehao();
            break;
        }
    }
}
void shanchu_dianhua()
{
    int t, m, mark = 0;
    char phone[20];
    printf("\n请输入要删除学生电话号码:");
    scanf("%s", phone);
    if (i == 0)
    {
        printf("\n无信息");
        getch();
        return;
    }
    for (t = 0; t < i; t++)
    {
        if (strcmp(stu[t].number, phone) == 0) //利用strcmp对比信息
        {
            printf("\n以下是您要删除的学生记录:");
            printf("%s %s %s ", stu[t].id, stu[t].name, stu[t].sex); 
            printf("%s %s\n", stu[t].number, stu[t].address) //打印查找到的信息
            printf("\n是否删除？（y/n）");
            if (getch() == 'y')
            {
                for (m = i; m < i - 1; m++) //前移进行替换
                {
                    stu[m] = stu[m + 1];
                }
                i--;    //减少人数
                mark++; //记录删除次数
                printf("\n删除成功");
                printf("\n是否继续删除？（y/n）");
                if (getch() == 'y')
                {
                    shanchu_dianhua();
                }
                return;
            }
            else
            {
                return;
            }
        }
        continue;
    }
    if (mark == 0)
    {
        printf("\n没有该学生的记录");
        printf("\n是否继续删除？（y/n）");
        if (getch() == 'y')
        {
            return;
        }
    }
}
void shanchu_xingming()
{
    int t, m, mark = 0, a = 0;
    char name[20];
    printf("\n请输入要删除学生姓名:");
    scanf("%s", name); //键盘输入姓名
    for (t = a; t < i; t++)
    {
        if (strcmp(stu[t].name, name) == 0) //对比字符串查找到要删除的联系人
        {
            printf("\n以下是您要删除的学生记录:");
            printf("%s %s %s ", stu[t].id, stu[t].name, stu[t].sex); 
            printf("%s %s\n", stu[t].number, stu[t].address)
            printf("\n是否删除？（y/n）");
            if (getch() == 'y') //删除
            {
                for (m = i; m < i - 1; m++)
                {
                    stu[m] = stu[m + 1];
                }
                i--;
                mark++;
                printf("\n删除成功");
                printf("\n是否继续删除？（y/n）");
                if (getch() == 'y')
                {
                    shanchu_xingming();
                }
                return;
            }
            else
            {
                return;
            }
        }
        continue;
    }
    if (mark == 0)
    {
        printf("\n没有该学生的记录");
        printf("\n是否继续删除（y/n）");
        if (getch() == 'y')
        {
            shanchu_xingming();
        }
        return;
    }
}
void shanchu_xuehao()
{
    int mark = 0;
    int t;
    char tedingxuehao[20];
    char *p;
    printf("\n请输入你要查询的特定学号的最后一位:");
    scanf("%s", tedingxuehao); //从键盘读取一个字符
    for (t = 0; t < i; t++)
    {
        p = strrchr(stu[i].id, '0');      //利用strrchr函数进行信息匹配
        if (strcmp(p, tedingxuehao) == 0) //利用strcmp函数找到学生信息
        {
            printf("\n以下是该生的信息:");
            printf("%s %s %s ", stu[t].id, stu[t].name, stu[t].sex); 
            printf("%s %s\n", stu[t].number, stu[t].address)//打印
            printf("\n是否删除该学生信息？（y/n）");
            if (getch() == 'y') //实现删除
            {
                for (int m = i; m < i - 1; m++)
                {
                    stu[m] = stu[m + 1];
                }
                i--;
                mark++;
                printf("\n删除成功");
                printf("\n是否继续删除？（y/n）");
                if (getch() == 'y')
                {
                    shanchu_xuehao(); //继续删除
                }
                return;
            }
            else
            {
                return;
            }
        }
        continue;
    }
    if (mark == 0)
    {
        printf("\n没有找到学生信息");
        printf("\n按任意键返回主菜单");
        getch();
        return;
    }
}

//查找修改    函数
void correct()
{
    char c;
    if (i == 0)
    {
        printf("\n对不起，文件中无任何记录");
        printf("\n按任意键返回主菜单");
        getch();
        return;
    }
    system("cls");
    printf("1-按姓名修改\n2-按电话修改");
    c = getch();
    switch (c)
    {
    case '1':
        xiugai_xingming(); //调用各自函数
        break;
    case '2':
        xiugai_dianhua();
        break;
    }
}
void xiugai_xingming()
{
    char c;
    int t, mark = 0;
    char name[20];
    printf("\n请输入要修改的学生姓名：");
    scanf("%s", name); //键入
    if (i == 0)
    {
        printf("\n文件中无任何学生");
        getch();
        return;
    }
    for (t = 0; t < i; t++)
    {
        if (strcmp(stu[t].name, name) == 0) //strcmp函数匹配
        {
            printf("\n以下是您要修改的学生信息：\n");
            printf("%s %s %s ", stu[t].id, stu[t].name, stu[t].sex); 
            printf("%s %s\n", stu[t].number, stu[t].address)//打印
            mark++;
            printf("\n输入y继续");
            if (getch() == 'y')
            {
                printf("\n1-修改姓名\n2-修改电话");
                printf("\n3-修改性别\n4-修改地址");
                printf("\n5-修改学号");
                scanf("%s", &c);
                getch();
                switch (c) //按不同方式修改信息
                {
                case '1':
                    printf("\n\t请输入新姓名：");
                    scanf("%s", stu[t].name);
                    break;
                case '2':
                    printf("\n\t请输入新电话：");
                    scanf("%s", stu[t].number);
                    break;
                case '3':
                    printf("\n\t请输入新性别：");
                    scanf("%s", stu[t].sex);
                    break;
                case '4':
                    printf("\n\t请输入新地址：");
                    scanf("%s", stu[t].address);
                    break;
                case '5':
                    printf("\n\t请输入新学号：");
                    scanf("%s", stu[t].id);
                    break;
                }
            }
        }
    }
    if (mark == 0)
    {
        printf("\n没有找到学生信息");
        printf("\n是否继续修改？（y/n）");
        if (getch() == 'y')
        {
            xiugai_xingming();
            return;
        }
    }
}
void xiugai_dianhua()
{
    char c;
    int t, mark = 0;
    char phone[20];
    printf("\n请输入要修改的学生电话:\n");
    scanf("%s", phone); //键入信息
    if (i == 0)
    {
        printf("\n文件中无任何学生");
        getch();
        return;
    }
    for (t = 0; t < i; t++)
    {
        if (strcmp(stu[i].number, phone) == 0) //strcmp做判断
        {
            printf("%s %s %s ", stu[t].id, stu[t].name, stu[t].sex); 
            printf("%s %s\n", stu[t].number, stu[t].address)//打印
            mark++;
            printf("\n输入y继续");
            if (getch() == 'y')
            {
                printf("\n1-修改姓名\n2-修改电话");
                printf("\n3-修改性别\n4-修改地址");
                printf("\n5-修改学号 ");
                scanf("%s", &c);
                getch();
                switch (c) //按不同方式修改
                {
                case '1':
                    printf("\n\t请输入新姓名：");
                    scanf("%s", stu[t].name);
                    break;
                case '2':
                    printf("\n\t请输入新电话：");
                    scanf("%s", stu[t].number);
                    break;
                case '3':
                    printf("\n\t请输入新性别：");
                    scanf("%s", stu[t].sex);
                    break;
                case '4':
                    printf("\n\t请输入新地址：");
                    scanf("%s", stu[t].address);
                    break;
                case '5':
                    printf("\n\t请输入新学号：");
                    scanf("%s", stu[t].id);
                    break;
                }
            }
        }
    }
    if (mark == 0)
    {
        printf("\n没有找到学生信息");
        printf("\n是否继续修改？（y/n）");
        if (getch() == 'y')
        {
            xiugai_dianhua();
            return;
        }
    }
}

//排序  函数
int pa()
{
    int w = i + 5, m;

    for (int c = 0; c < i - 1; c++)
    {
        for (int j = 0; j < i - c - 1; j++)
        {
            if (m = strcmp(stu[j].id, stu[j + 1].id) > 0)
            {

                stu[w] = stu[j];
                stu[j] = stu[j + 1];
                stu[j + 1] = stu[w];
            }
        }
    }
    myprintf(); //排完序后利用函数打印全部信息
}

//主函数
int main()
{
    int z;
    readflies();
    while (z != 1)
    {
        system("cls");
        char a;
        printf("\t请输入你要执行的操作:\n");
        printf("1.录入信息\t\t2.查看信息\n3.保存信息\t\t4.全部清空\n");
        printf("5.查询信息\t\t6.退出系统\n7.删除信息\t\t8.修改信息\n9.排序\n"); //菜单
        a = getch();
        switch (a) //进入不同函数模块
        {
        case '1':
            input();
            break;
        case '2':
            myprintf();
            break;
        case '3':
            mysave();
            break;
        case '4':
            alldelete();
            break;
        case '5':
            mysearch();
            break;
        case '6':
            z = 1; //实现退出系统
            break;
        case '7':
            deletepart();
            break;
        case '8':
            correct();
            break;
        case '9':
            pa();
            break;
        }
    }
}

```





# 通讯录管理软件运行结果截图与分析:

## 1.录入信息函数运行结果

- **输入过程**

<img src="C:\Users\AAA\AppData\Roaming\Typora\typora-user-images\image-20210106223005936.png" alt="image-20210106223005936" style="zoom:50%;" />

- **最终录入信息后结果如图**

<img src="C:\Users\AAA\AppData\Roaming\Typora\typora-user-images\image-20210106223200680.png" alt="image-20210106223200680" style="zoom:50%;" />



## 2.打印函数测试以及结果

- **打印正常没有出现乱码等情况**

<img src="C:\Users\AAA\AppData\Roaming\Typora\typora-user-images\image-20210106223444068.png" alt="image-20210106223444068" style="zoom:50%;" />

- **查询到学生信息后的打印信息也同样正常,如图:**

<img src="C:\Users\AAA\AppData\Roaming\Typora\typora-user-images\image-20210106224031273.png" alt="image-20210106224031273" style="zoom:50%;" />

## 3.清空函数测试

- **原有信息**

<img src="C:\Users\AAA\AppData\Roaming\Typora\typora-user-images\image-20210106224205109.png" alt="image-20210106224205109" style="zoom:50%;" />

- **运行清空函数后,原有内容成功清除**

<img src="C:\Users\AAA\AppData\Roaming\Typora\typora-user-images\image-20210106224512158.png" alt="image-20210106224512158" style="zoom:50%;" />

## 4.排序函数测试以及debug调试

- **debug测试(此时m的值等于1,说明前面的数大于后面的数,说明两个结构体成功交换)**

<img src="C:\Users\AAA\AppData\Roaming\Typora\typora-user-images\image-20210106225431542.png" alt="image-20210106225431542" style="zoom:50%;" />

- **排序完成后最终效果图(成功对学号进行了升序排序)**

<img src="C:\Users\AAA\AppData\Roaming\Typora\typora-user-images\image-20210106225753522.png" alt="image-20210106225753522" style="zoom:50%;" />

## 5.对修改学生信息函数的测试

- **初始图(目标完成对王五的姓名改成六五)**

<img src="C:\Users\AAA\AppData\Roaming\Typora\typora-user-images\image-20210106230009831.png" alt="image-20210106230009831" style="zoom:50%;" />

- **结果图**

<img src="C:\Users\AAA\AppData\Roaming\Typora\typora-user-images\image-20210106230147281.png" alt="image-20210106230147281" style="zoom:50%;"/>

![image-20210106231036306](C:\Users\AAA\AppData\Roaming\Typora\typora-user-images\image-20210106231036306.png)

## 6.删除信息函数测试

- **删掉六五**

<img src="C:\Users\AAA\AppData\Roaming\Typora\typora-user-images\image-20210106231211265.png" alt="image-20210106231211265" style="zoom:50%;" />

- **结果**

<img src="C:\Users\AAA\AppData\Roaming\Typora\typora-user-images\image-20210106231346136.png" alt="image-20210106231346136" style="zoom:50%;" />





#  在开发和设计过程中所遇到的问题和解决过程

## 1.开发过程中的困难 

1. 在以特定尾号查询学生信息时常常会出现查询到不是尾号的数字,导致难以达到需求.
2. 在利用结构体函数写进TXT文档中乱发,并且多读少读,无输出的情况.
3. 排序过程中不知道如何将整个结构体交换.

## 2.解决过程

1. 对于特定尾号查找,通过查询网上资源明白了可以通过strcmp以及strstr相互结合达到效果.
2. 写入文档时先用sizeof对一个结构体的字节数进行读取再以特定的字节书写入文档.
3. fwrite(&stu[t], sizeof(V), 1, fp); 
4. fread(&stu[t], sizeof(V), 1, fp); 

![image-20210107004400753](C:\Users\AAA\AppData\Roaming\Typora\typora-user-images\image-20210107004400753.png)





# 新学函数知识,以及小程序运行分析

## 1.fopen函数

1. 描述:

   C 库函数 **FILE \*fopen(const char \*filename, const char \*mode)** 使用给定的模式 **mode** 打开 **filename** 所指向的文件。

2. 模式:

3. | 模式 | 作用                                                         |
   | :--- | :----------------------------------------------------------- |
   | r    | 打开一个用于读取的文件。该文件必须存在。                     |
   | w    | 创建一个用于写入的空文件。如果文件名称与已存在的文件相同，则会删除已有文件的内容，文件被视为一个新的空文件。代码 |

   代码:

   ```c
   #include <stdio.h>
   #include <stdlib.h>
   int main()
   {
      FILE * fp;
      fp = fopen ("file.txt", "w");
      fprintf(fp, "%s %s %s %d", "We", "are", "in", 2014);
      fclose(fp);
      return(0);
   }
   ```

   

## 2.sizeof

1. 描述:

   **sizeof** 是一个关键字，它是一个编译时运算符，用于判断变量或数据类型的字节大小。sizeof 运算符可用于获取类、结构、共用体和其他用户自定义数据类型的大小。

2. 可以使用 sizeof 操作符计算int, float, double 和 char四种变量字节大小。sizeof 是 C 语言的一种单目操作符，如C语言的其他操作符++、--等，它并不是函数。

3. sizeof实际上是获取了数据在内存中所占用的存储空间，以字节为单位来计数。

4. 代码:

   ```c
   #include <stdio.h>
   
   int main()
   {
       int integerType;
       float floatType;
       double doubleType;
       char charType;
   // sizeof 操作符用于计算变量的字节大小
   printf("Size of int: %ld bytes\n",sizeof(integerType));
   printf("Size of float: %ld bytes\n",sizeof(floatType));
   printf("Size of double: %ld bytes\n",sizeof(doubleType));
   printf("Size of char: %ld byte\n",sizeof(charType));
   return 0;
   }
   ```

   



- **运行结果示例**

<img src="C:\Users\AAA\AppData\Roaming\Typora\typora-user-images\image-20210107010558920.png" alt="image-20210107010558920" style="zoom:50%;" />

## 3.strstr函数

1. 描述:

   C 库函数 **char \*strstr(const char \*haystack, const char \*needle)** 在字符串 **haystack** 中查找第一次出现字符串 **needle** 的位置，包含终止符 '\0'。

2. 返回值:

   该函数返回在 haystack 中第一次出现 needle 字符串的位置，如果未找到则返回 null。

3. 声明:

   ```c
   char *strstr(const char *haystack, const char *needle)
   ```

   代码:

   ```c
   #include <stdio.h>
   #include <string.h>
   int main()
   {
      const char haystack[20] = "神奇的东西";
      const char needle[10] = "东西";
      char *ret;
      ret = strstr(haystack, needle);
      printf("子字符串是： %s\n", ret);
      return(0);
   }
   ```

   **运行结果**

   <img src="C:\Users\AAA\AppData\Roaming\Typora\typora-user-images\image-20210107011724051.png" alt="image-20210107011724051" style="zoom:50%;" />

   
