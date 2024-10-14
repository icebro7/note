#  一、C语言编写编译

​		先编译再执行

​				编译：将(.c)程序	翻译成目标文件(.obj)	

​				链接：将(.obj)	生产可执行文件(.exe)

​				运行：执行(.exe)文件	得到运行结果

<img src="C:\Users\98680\AppData\Roaming\Typora\typora-user-images\image-20220301101553743.png" alt="image-20220301101553743" style="zoom:150%;" />



​			对修改之后的源文件需要重新编译链接，生成新的.exe文件后再执行，才能生效

# 二、语法



### 		1、标准库

​				在程序头部进行引用

```c
#include <stdio.h>
```

​				

### 		2、输出	

​				如果输出的是整数：	%d		(long:	%ld		long long:	%lld)

 				如果输出的是小数：	%f	小数点几位就几f	%2f

​				如果输出的是字符：	%c

​				如果输出的是字符串：	%s

```c
#include<stdio.h>

 int main() {
	int num = 2;
	double s = 2.3;
	char x = 'a';
	char c[] = "abc";
	printf("num=%d s=%f x=%c c=%s", num, s, x, c);

}
```



### 		3、数据类型

![image-20220303102705939](C:\Users\98680\AppData\Roaming\Typora\typora-user-images\image-20220303102705939.png)

​			<1>在c中，没有字符串类型, 使 用字符数组表示字符串 

​			<2>在不同系统上，部分数据类型字节长度不一样  int 为2 或4

<img src="C:\Users\98680\AppData\Roaming\Typora\typora-user-images\image-20220303104215516.png" alt="image-20220303104215516" style="zoom:150%;" />



#### 								(​​1)整型：

![image-20220303121731043](C:\Users\98680\AppData\Roaming\Typora\typora-user-images\image-20220303121731043.png)



#### 				(​2)浮点型

![image-20220303122214718](C:\Users\98680\AppData\Roaming\Typora\typora-user-images\image-20220303122214718.png)



​				<1>浮点型常量默认为double型 ，声明float型常量时，须后加‘f’或‘F:

```c
#include<stdio.h>

int main() {
	float a1 = 1.3f;
	double a2 = 1.3;
	printf("a1=%f a2=%f", a1, a​;

}
```



​				<2>浮点型常量有两种表示形式 

​					十进制数形式：如：5.12 512.0f .512 (必须有小数点, 等价0.512）

​					科学计数法形式:如：5.12e2(5.12  *  10^​ 、 5.12E-2(5.12  /  10^​



#### 			(​3)字符类型

​				<1>在C中，char的本质是一个整数，在输出时，是ASCII码对应 的字符。

​				<2>可以直接给char赋一个整数，然后输出时，会按照对应的 ASCII 字符输出 

```c
#include<stdio.h>

int main() {
	char c1 = 'a';
	char c2 = 'b';
	char c3 = 97;

	printf("c1=%c c2=%c c3=%c", c1, c2,c​; //c1=a c2=b c3=a

}
```

​				<3>char类型是可以进行运算的，相当于一个整数，因为它都对 应有Unicode码

```c
#include<stdio.h>

int main() {
	char c1 = 'a';
	int num = c1 + 10;
	char c2 = 'a' + 1;

	printf("c2=%c num=%d", c2,num); //c2=b num=107

}
```



#### 			(​4)基本数据类型转换

##### 				<1>自动类型转换（小转大)：

​					C程序在进行赋值或者运算时，精度小的类型自动转换为精度大的数据类型。

![image-20220304163705114](C:\Users\98680\AppData\Roaming\Typora\typora-user-images\image-20220304163705114.png)



​					如果右边变量的数据类型长度比左边长时，将丢失一部分数据，这样会降低精度

```c
#include<stdio.h>

int main() {
	float a1 = 1.2;
	double a2 = 1.2545648454384;
	a1 = a2;			  //double -> float
    
	printf("a1=%8f", a​​; //a1=1.254565 失精
	
}
```



##### 			<2>强制类型转换

​				将精度高的数据类型转换为精度小的数据类型, 使用时加上强制转换符 ( )

```c
#include<stdio.h>

int main() {
	double a1 = 1.99;
	int a2 = (int)a1;
	printf("a2=%d", a​; //a2 = 1 ,不是四舍五入，而是直接截断
	

}
```



​				强转符号只针对于最近的操作数有效

```c
#include<stdio.h>

int main() {
	
	int a2 = (int)3.5 * 10 + 6 * 1.5;	//3*10 = 30;	6*1.5 =9
	int a3 = (int)(3.5 * 10 + 6 * 1.​;		//3.5 * 1. =35;	6*1.5 =9
	printf("a2=%d,a3=%d", a2,a​; //a2 = 39   a3 = 44
}
```

#### (5)​常量

##### 		<1>整数常量

​				举例说明：

```
85 /* 十进制 */
0213 /* 八进制 */
0x4b /* 十六进制 */ 八进制和十六进制后面解释
30 /* 整数 */
30u /* 无符号整数 */
30l /* 长整数 */
30ul /* 无符号长整数 */
```



##### 		<2>浮点常量

```
3.14159; //double 常量
314159E-5; // 科学计数法
3.1f; //float 常量
```



##### 		<3>字符常量

​				括在单引号中	‘ ’

```
'X'
'Y'
'A'
'b'
'1'
'\t'
```



##### 	<4>字符串常量

​			字符串字面值或常量是括在双引号	” “

```
"hello, world"
"北京"
"hello \
world"
```



#### 		(6)​常量的定义

##### 				<1>define预处理器  定义常量

​					定义的常量不可以 修改

```c
	#define 常量名 常量值
```

```c
#include <stdio.h>
#define PI 3.14 //定义常量 PI 常量值 3.14
int main() {
//PI = 3.1415 可以吗?=》 不可以修改，因为 PI 是常量
//可以修改 PI 值?
//PI = 3.1415; //提示 = 左值 必须是可修改的值
double area;
double r = 1.2;//半径
area = PI * r * r;
printf("面积 : %.2f", area);
getchar();
return 0;
}
```



##### 			<2>const关键字定义常量

```
const 数据类型 常量名 = 常量值; //即就是一个语句
```

```c
#include <stdio.h>
//说明
//1. const 是一个关键字，规定好，表示后面定义了一个常量
//2. PI 是常量名，即是一个常量，常量值就是 3.14
//3. PI 因为是常量，因此不可以修改
//4. const 定义常量时，需要加 分号
const double PI = 3.14;
int main() {
//PI = 3.1415 可以吗? => 不可以
double area;
double r = 1.2;
area = PI * r * r;
printf("面积 : %.2f", area);
getchar();
return 0;
}
```



##### 		<3>define 和const的区别

```
 const 定义的常量时，带类型，define 不带类型

​ const 是在 编译、运行的时候起作用，而 define 是在编译的预处理阶段起作用

​ define 只是简单的替换，没有类型检查。简单的字符串替换会导致边界效应. 

​ const 常量可以进行调试的，define 是不能进行调试的，主要是预编译阶段就已经替换掉了，调试的时候就没它了

​ const 不能重定义，不可以定义两个一样的，而 define 通过 undef 取消某个符号的定义，再重新定义 

​ define 可以配合#ifdef、 #ifndef、 #endif 来使用， 可以让代码更加灵活，比如我们可以通过#define 来 启动或者关闭 调试信息
```



#### (7)​算术运算符

​			对数值型变量进行运算

![image-20220323214744587](C:\Users\98680\AppData\Roaming\Typora\typora-user-images\image-20220323214744587.png)



#### (8)关系运算符

![image-20220323214952020](C:\Users\98680\AppData\Roaming\Typora\typora-user-images\image-20220323214952020.png)



#### (9)逻辑运算符

![image-20220323215050474](C:\Users\98680\AppData\Roaming\Typora\typora-user-images\image-20220323215050474.png)



#### (10)赋值运算符

![image-20220323215142248](C:\Users\98680\AppData\Roaming\Typora\typora-user-images\image-20220323215142248.png)



#### (1​​1)位运算符

![image-20220323215242731](C:\Users\98680\AppData\Roaming\Typora\typora-user-images\image-20220323215242731.png)

```
 二进制的最高位是符号位: 0 表示正数,1 表示负数
​ 正数的原码，反码，补码都一样 (三码合一)
​ 负数的反码=它的原码符号位不变，其它位取反(0->1,1->0)
​ 负数的补码=它的反码+1 //
​ 0 的反码，补码都是 0
​ 在计算机运算的时候，都是以补码的方式来运算的.
```

​		<1>& 

​			全真为真



​		<2>|

​			全假为假



​		<3>^

​			相同为假



​		<4>~

​			取反，相反	

​			~1 = ?

​			2的补码：

​			00000000 00000000 00000000 00000010

​			取反 => 补码

​			11111111 11111111 11111111 11111101

​			对应的反码：

​			补码-1

​			11111111 11111111 11111111 11111100

​			将反码 ->原码

​			10000000 00000000 00000000 00000011

​			为 -3

​			                                                 

​		<5><<

​			左移，将二进制的位数左移若干位

​			60 << 2  即：0011 1100  => 1111 0000



​		<6>>>

​			右移，将二进制的位数右移若干位

​			60 >> 2 即：0011 1100 => 0000 1111



#### (1​2)运算符优先级

```
 结合的方向只有三个是从右向左，其余都是从左向右
​ 所有的双目运算符中只有赋值运算符的结合方向是从右向左
​ 另外两个从右向左的结合运算符，一个是单目运算，还有一个是三目运算()
​ 逗号的运算符优先级最低
​算术运算符 > 关系运算符 > 逻辑运算符(逻辑非! 除外) > 赋值运算符 > 逗号
```

​		

![image-20220324143058675](C:\Users\98680\AppData\Roaming\Typora\typora-user-images\image-20220324143058675.png)



#### (13)​枚举

```c
enum 枚举名 {
		枚举元素1，枚举元素2……
}
```

```
 第一个枚举成员的默认值为整型的 0，后续枚举成员的值在前一个成员上加 1。我们在这个实例中把第一个枚举成员的值定义为 1，第二个就为 2，以此类推. 
​ 在定义枚举类型时改变枚举元素的值
​ 枚举变量的定义的形式 1-先定义枚举类型，再定义枚举变量
​在定义时没有枚举名，此枚举类型只能使用一次
```



```c
#include <stdio.h>
void main() {
enum DAY
{
MON=1, TUE=2, WED=3, THU=4, FRI=5, SAT=6, SUN=7
};  // 这里 DAY 就是枚举类型, 包含了 7 个枚举元素
enum DAY day; // enum DAY 是枚举类型， day 就是枚举变量
day = WED; //给枚举变量 day 赋值，值就是某个枚举元素
printf("%d",day);// 3 ， 每个枚举元素对应一个值
getchar();
}
```



#### (14)​static关键字

```
 局部变量被 static 修饰后，我们称为静态局部变量
​ 对应静态局部变量在声明时未赋初值，编译器也会把它初始化为 0。
​ 静态局部变量存储于进程的静态存储区(全局性质)，只会被初始一次，即使函数返回，它的值也会保持不变
```



```
函数使用static修饰：
​​ 函数的使用方式与全局变量类似，在函数的返回类型前加上 static，就是静态函数
​ 非静态函数可以在另一个文件中通过 extern 引用 
​ 静态函数只能在声明它的文件中可见，其他文件不能引用该函数[案例]
​ 不同的文件可以使用相同名字的静态函数，互不影响
```

​	

#### (1​5)数据类型转换

##### 		<1>基本数据类型转字符串数据类型  sprintf

```c
char str1[20]; 
sprintf(str1,"%d %d",a,b); //传入字符串中
```

​		

##### 		<2>字符串类型转基本数据类型  atoi(整)  atof(小)

```c
char str[10] = "123456";
char str2[10] = "12.67423";
int num1 = atoi(str) //整数
double d = atof(str​;  //小数
```



#### (1​6)数组

​		数据类型 数组名 [数组大小]

​		数据类型 数组名[ ] = {数据1，数据2……}



##### 		<1>返回数组长度：

​		sizeof(array) /sizeof(array[0])

​		

##### 		<2>字符数组：           



```
数据类型 数组名[ ] = {‘字符1’，‘字符2’……‘/0’}

​	尽量给数组大点，会自动给‘/0’，否则自己加上‘/0’

数据类型 数组名[ ] = "字符"
​	会自动给/0
```

​		

​	<3>字符指针变量

```
数据类型* 指针名 = "字符串"
​	字符指针变量中存放的是字符的地址
```



![image-20220331222624142](C:\Users\98680\AppData\Roaming\Typora\typora-user-images\image-20220331222624142.png)



##### 		<3> 字符串相关数组

![image-20220331222814020](C:\Users\98680\AppData\Roaming\Typora\typora-user-images\image-20220331222814020.png)



```
 程序中往往依靠检测 '\0' 的位置来判定字符串是否结束，而不是根据数组的长度来决定字符串长度。因此，字符串长度不会统计 '\0', 字符数组长度会统计
​ 在定义字符数组时应估计实际字符串长度，保证数组长度始终大于字符串实际长度， 否则，在输出字符数组时可能出现未知字符. 
​ 系统对字符串常量也自动加一个'\0'作为结束符。例如"C Program”共有 9 个字符，但在内存中占 10 个字节，最后一个字节'\0'是系统自动加上的。（通过 sizeof()函数可验证）
​ 定义字符数组时，如果 给的字符个数 比 数组的长度小，则系统会默认将剩余的元素空间，全部设置为 '\0', 比如 char str[6] = "ab" , str 内存布局就是[a][b][\0][\0][\0][\0]
​ 字符数组定义和初始化的方式比较多
```



```c
#include <stdio.h>
void main() {
char str1[ ]={"I am happy"}; // 默认后面加 '\0' 
char str2[ ]="I am happy"; // 省略{}号 ,默认后面加 '\0'
char str3[ ]={'I',' ','a','m',' ','h','a','p','p','y'}; // 字符数组后面不会加 '\0', 可能有乱码
char str4[5]={'C','h','i','n','a'}; //字符数组后面不会加 '\0', 可能有乱码
char * pStr = "hello";//ok

printf("\n str1=%s", str​​;
printf("\n str2=%s", str​;//ok
printf("\n str3=%s", str​;//乱码
printf("\n str4=%s", str​;//乱码
getchar();
}
```



### 4.语句

#### (1)​​键盘输入语句

​			scanf()

```c
#include <stdio.h>
void main() {
//使用字符数组接收名
char name[10] = "";
int age = 0;
double sal = 0.0;
char gender = ' ';
//提示用户输入信息
printf("请输入名字：");
//scanf("%s", name) 表示接收 一个字符串，存放到 name 字符数组
scanf("%s", name);
printf("请输入年龄：");
scanf("%d", &age); // 因为我们将得到输入存放到 age 变量指向地址,因此需要加 &
printf("请输入薪水：");
scanf("%lf", &sal); //接收一个 double 时，格式参数 %lf
printf("请输入性别(m/f)：");
scanf("%c", &gender); //这里是接收到了上面的回车字符
scanf("%c", &gender); //等待用户输入. //输出得到信息
printf("\nname %s age %d sal %.2f gender %c", name, age,sal,gender);
getchar();//接收到一个回车
getchar();//这个 getchar() 才会让控制台暂停
}
```



#### (​2)进制转换

##### 		<1>二转十：

​			例： 1011 = 1 * 2 ^ 0   +  1 * 2 ^1   +   0 + 1 * 2 ^3 = 11

##### 		<2>八转十：

​			例：  0123 = 3 * 8 ^ 0 + 2 * 8 ^ 1 + 1 * 8 ^ 2 + 0 = 83 

##### 		<3>十六转十：

​			例：  0x34A = 10 * 16 ^ 0 + 4 * 16 ^ 1 + 3 * 16 ^2 = 842

##### 		<4>十转二：

​			不断除二直到商为零，将每步余数倒过来

##### 		<5>十转八：

​			不断除八直到商为零，将每步余数倒过来

##### 		<6>十转十六：

​			不断除十六直到商为零，将每步余数倒过来

##### 		<7>二转八：

​			(从右往左算)

​			从低位开始，每三位一组，转成对应的八进制即可

​			11 010 101 => 0325

​			

##### 		<8>二转十六：

​			从低位开始，每四位一组，转成对应的十六进制即可

​			1101 0101 => 0XD5



##### 		<9>八进制转二进制：

​			(从右往左算)

​			八位数每一位数，转成对应的一个三位的二进制数

​			0237 = 10011111



##### 		<10>十六进制成二进制：

​			十六位数的每一位数，转成对应的一个四位的二进制数

​			0x23B = 10 0011 1011



#### 	(​3)if -else -else if

```c 
#include <stdio.h>
void main() {
/*
岳小鹏参加 C 二级考试，他和父亲岳不群达成承诺：
如果：
成绩为 100 分时，奖励一辆 BMW；
成绩为(80，99]时，奖励一台 iphone7plus；
当成绩为[60,80]时，奖励一个 iPad；
其它时，什么奖励也没有。
请从键盘输入岳小鹏的 C 二级考试，并加以判断, 输出提示
分析
1. 定义一个 double
2. 因为判断条件有多个，因此我们使用多分支处理
*/
double score = 0.0;
printf("请输入成绩");
scanf("%lf", &score);
if( score == 100) {
printf("奖励一辆 BMW");
} else if (score > 80 && score <= 99) {
printf("奖励一台 iphone7plus");
} else if (score >= 60 && score <= 80) {
printf("奖励一台 iPad");
} else {
printf("没有奖励");
}
getchar();//得到的回车
getchar();//控制暂停
}
```



#### (4)​switch

![image-20220326221137484](C:\Users\98680\AppData\Roaming\Typora\typora-user-images\image-20220326221137484.png)

```
 switch 语句中的 expression 是一个常量表达式，必须是一个整型(char、short, int, long 等) 或枚举类型
​ case 子句中的值必须是常量,而不能是变量
​ default 子句是可选的，当没有匹配的 case 时，执行 default
​ break 语句用来在执行完一个 case 分支后使程序跳出 switch 语句块；
​ 如果没有写 break，会执行下一个 case 语句块，直到遇到 break 或者执行到 switch 结尾, 这个现象称为穿透
```



```c
#include <stdio.h>
void main() {
//使用 switch 把小写类型的 char 型转为大写(键盘输入)。只转换 a, b, c, d, e. 其它的输出 “other”。
char c1 = 'u';
switch(c​​ {
case 'a' :
printf("A");
break;
case 'b' :
printf("B");
break;
case 'c' :
printf("C");
break;
case 'd' 
printf("D");
break;
case 'e' :
printf("E");
break;
default :
printf("other");
}
getchar();
```



#### (5)​for

```
 循环条件是 返回一个表示真(非 0)假(0) 的表达式
​ for(;循环判断条件;) 中的初始化和变量迭代可以不写（写到其它地方），但是两边的分号不能省略。
​ 循环初始值可以有多条初始化语句，但要求类型一样，并且中间用逗号隔开，循环变量迭代也可以有多条变量
迭代语句，中间用逗号隔开。
```

```c
void main() {
//打印 1~100 之间所有是 9 的倍数的整数的个数及总和. [使用 for 完成]
//分析
//1. 定义变量 count 记录 个数
//2. 定义变量 sum 记录 总和
int i = 0;
int count = 0;
int sum = 0;
for ( i = 1; i <= 100 ; i++) {
//判断 i 是不是 9 的倍数
if( i % 9 == 0 ) { //是
count++; //统计个数
sum += i; //累计到 sum
}
}
printf("\n count=%d sum=%d", count, sum);
}
```



#### (6)​while

![image-20220326222309933](C:\Users\98680\AppData\Roaming\Typora\typora-user-images\image-20220326222309933.png)



```
 循环条件是返回一个表示真(非 0)假(0) 的表达式
​ while 循环是先判断再执行语句
```

```c
void main() {
// 打印 1—100 之间所有能被 3 整除的数
int i = 1;
int max = 100;
while(i <= max) {
if( i % 3 == 0 ) {
printf("\n i=%d 能被 3 整除", i);
}
i++;
}
getchar();
}
```



#### (7)​do - while

```
1循环变量初始化;
do{
    2循环体(多条语句);
    3循环变量迭代;
}while(4循环条件);
注意：do – while 后面有一个 分号，不能省略.
```

```
 循环条件是返回一个表示真(非 0)假(0) 的表达式
​ do..while 循环是先执行，再判断
```

```c
void main() {
//5 句话
int i = 1; //循环变量初始化
int max = 5; //循环的最大次数
do {
printf("\n 你好，尚硅谷 i=%d", i); //循环体
i++; //循环变量迭代
}while(i <= max); //循环条件
printf("\ni=%d", i); // i= 6
getchar();
}
```



#### (8)goto

![image-20220326223046501](C:\Users\98680\AppData\Roaming\Typora\typora-user-images\image-20220326223046501.png)

```c
void main() {
printf("start\n");
goto lable1; //lable1 称为标签
printf("ok1\n");
printf("ok2\n");
lable1:
printf("ok3\n");
printf("ok4\n");
getchar();
} //输出 ok3 和 ok4
```



#### (9)函数

```
返回类型 函数名（形参列表）{

执行语句...; // 函数体
return 返回值; // 可选
}
```

```
 函数的形参列表可以是多个。
​ C 语言传递参数可以是值传递（pass by value），也可以传递指针（a pointer passed by value）也叫引用传递。
​ 函数的命名遵循标识符命名规范，首字母不能是数字，可以采用 驼峰法 或者 下划线法 ，比如 getMax()
get_max()。
​ 函数中的变量是局部的，函数外不生效
​ 基本数据类型默认是值传递的，即进行值拷贝。在函数内修改，不会影响到原来的值
​在函数内掺入变量的地址&，即可让函数内的变量影响函数外的变量（指针传递）
​c语言 不支持函数重载
```



#### (10)头文件

```
include <>：引用的是编译器的类库路径里面的头文件，用于引用系统头文件。(效率高)

include ""：引用的是你程序目录的相对路径中的头文件，如果在程序目录没有找到引用的头文件则到编译器的
类库路径的目录下找该头文件，用于引用用户头文件。
```

##### 		<1>预处理

​			win和linux系统不同的操作：

```c
#if _WIN32 //如果是 windows 平台, 就执行 #include <windows.h>
#include <windows.h>
#elif __linux__ //否则判断是不是 linux ,如果是 linux 就引入<unistd.h>
#include <unistd.h>
#endif
int main() {
//不同的平台下调用不同的函数
#if _WIN32 //识别 windows 平台
Sleep(5000); //毫
#elif __linux__ //识别 linux 平台
sleep(​; //秒
#endif
puts("hello, 尚硅谷~"); //输出
getchar();
return 0;
}
```



##### 		<2>宏定义

​	#define  宏名  字符串  //宏定义

​	#undef  宏名                //取消宏定义

```c
#define N 100
int main(){
int sum = 20 + N;//int sum = 20 +100
printf("%d\n", sum);
getchar();
return 0;
}
```



```
 宏定义是用宏名来表示一个字符串，在宏展开时又以该字符串取代宏名，这只是一种简单的替换。字符串中可以含任何字符，它可以是常数、表达式、if 语句、函数等，预处理程序对它不作任何检查，如有错误，只能在编译已被宏展开后的源程序时发现。
​ 宏定义不是说明或语句，在行末不必加分号，如加上分号则连分号也一起替换
​ 宏定义必须写在函数之外，其作用域为宏定义命令起到源程序结束。如要终止其作用域可使用#undef 命令
​ 代码中的宏名如果被引号包围，那么预处理程序不对其作宏代替
​ 宏定义允许嵌套，在宏定义的字符串中可以使用已经定义的宏名，在宏展开时由预处理程序层层代换
​ 习惯上宏名用大写字母表示，以便于与变量区别。但也允许用小写字母
​ 可用宏定义表示数据类型，使书写方便
​ 宏定义表示数据类型和用 typedef 定义数据说明符的区别：宏定义只是简单的字符串替换，由预处理器来处理；而 typedef 是在编译阶段由编译器处理的，它并不是简单的字符串替换，而给原有的数据类型起一个新的名字，将它作为一种新的数据类型
```



##### 		<2>带参数宏定义

​			\#define  宏名(形参列表)  字符串

```
 带参宏定义中，形参之间可以出现空格，但是宏名和形参列表之间不能有空格出现
​ 在带参宏定义中，不会为形式参数分配内存，因此不必指明数据类型。而在宏调用中，实参包含了具体的数据，要用它们去替换形参，因此实参必须要指明数据类型
​ 在宏定义中，字符串内的形参通常要用括号括起来以避免出错。
```

```c
#include <stdlib.h>
#define SQ(y) (y)*(y) // 带参宏定义,字符串内的形参通常要用括号括起来以避免出错
int main(){
int a, sq;
printf("input a number: ");
scanf("%d", &a);
sq = SQ(a+​​; // 宏替换 (a+​​ * (a+​​
printf("sq=%d\n", sq);
system("pause");
return 0;}
```



![image-20220331144634012](C:\Users\98680\AppData\Roaming\Typora\typora-user-images\image-20220331144634012.png)



#### (11)​​变量作用域

```
 函数内部声明/定义的局部变量，作用域仅限于函数内部。
​ 函数的参数，形式参数，被当作该函数内的局部变量，如果与全局变量同名它们会优先使用局部变量(编译器
使用就近原则
​ 在一个代码块，比如 for / if 中 的局部变量，那么这个变量的的作用域就在该代码块
​ 在所有函数外部定义的变量叫全局变量，作用域在整个程序有效
```



##### 		<1>初始化局部全局变量

```
 局部变量，系统不会对其默认初始化，必须对局部变量初始化后才能使用，否则，程序运行后可能会异常退出. ​ 全局变量，系统会自动对其初始化
```



​			全局变量的自动初始化默认值：

![image-20220329194505450](C:\Users\98680\AppData\Roaming\Typora\typora-user-images\image-20220329194505450.png)



```
 全局变量(Global Variable)保存在内存的全局存储区中，占用静态的存储单元，它的作用域默认是整个程序，也就是所有的代码文件，包括源文件（.c 文件）和头文件（.h 文件）。
​ 局部变量(Local Variable)保存在栈中，函数被调用时才动态地为变量分配存储单元，它的作用域仅限于函数内
部。
​ C 语言规定，只能从小的作用域向大的作用域中去寻找变量，而不能反过来，使用更小的作用域中的变量
​ 在同一个作用域，变量名不能重复，在不同的作用域，变量名可以重复，使用时编译器采用就近原则. 
​ 由{ }包围的代码块也拥有独立的作用
```



​			c程序的内存布局图：

![image-20220329194827709](C:\Users\98680\AppData\Roaming\Typora\typora-user-images\image-20220329194827709.png)





#### (1​2)系统函数



##### 		<1>字符串系统函数 string.h

###### 			1.字符串长度 strlen

###### 			2.字符串赋值 strcpy

###### 			3.字符串增加 strcat

###### 4.字符串比较strcmp

​			

##### 		<2>时间系统函数 time.h



​				

##### 		<3>数学系统函数 math.h

​	

#### (13)​断点调试

```
f5： 开始调试 、执行到下一个断点
f11: 逐句执行代码, 会进入到函数体中
f10: 逐过程执行(遇到函数，不会进入到函数体)
shift+f11: 跳出(跳出某个函数, 跳出前，会将该函数执行完)
shift+f5: 终止调试
```



#### (1​4)动态内存分布

```
 全局变量——内存中的静态存储区
​ 非静态的局部变量——内存中的动态存储区——stack 栈
​ 临时使用的数据---建立动态内存分配区域，需要时随时开辟，不需要时及时释放——heap 堆
​ 根据需要向系统申请所需大小的空间，由于未在声明部分定义其为变量或者数组，不能通过变量名或者数组名来引用这些数据，只能通过指针来引用
```



##### 		<1>相关函数

​			头文件：\#include <stdlib.h>

​			关于内存动态分配

```
 避免分配大量的小内存块。分配堆上的内存有一些系统开销，所以分配许多小的内存块比分配几个大内存块的
系统开销大
​ 仅在需要时分配内存。只要使用完堆上的内存块，就需要及时释放它(如果使用动态分配内存，需要遵守原则：
(谁分配，谁释放)， 否则可能出现内存泄漏
​ 总是确保释放以分配的内存。在编写分配内存的代码时，就要确定在代码的什么地方释放内存
​ 在释放内存之前，确保不会无意中覆盖堆上已分配的内存地址，否则程序就会出现内存泄漏。在循环中分配内
存时，要特别小心
​ 指针使用一览

```



![image-20220401214037681](C:\Users\98680\AppData\Roaming\Typora\typora-user-images\image-20220401214037681.png)





###### 		1、maloc

```
函数原型 void * malloc（usigned int size） //memory allocation
​	作用——在内存的动态存储区(堆区)中分配一个长度为 size 的连续空间。
​	形参 size 的类型为无符号整型，函数返回值是所分配区域的第一个字节的地址，即此函数是一个指针型函数，返回的指针指向该分配域的开头位置。
​	malloc(100); 开辟 100 字节的临时空间，返回值为其第一个字节的地址
```



###### 	2、calloc

```
 函数原型 void *calloc（unsigned n,unsigned size）
​  作用——在内存的动态存储区中分配 n 个长度为 size 的连续空间，这个空间一般比较大，足以保存一个数组
​  用 calloc 函数可以为一维数组开辟动态存储空间，n 为数组元素个数，每个元素长度为 size.  函数返回指向所分配域的起始位置的指针；分配不成功，返回 NULL。
​  p = calloc(50, 4); //开辟 50*4 个字节临时空间，把起始地址分配给指针变量 p
```



###### 	3、free

```
函数原型：void free（void *p）
​ 作用——释放变量 p 所指向的动态空间，使这部分空间能重新被其他变量使用。
​ p 是最近一次调用 calloc 或 malloc 函数时的函数返回值
​ free 函数无返回值
​ free(p); // 释放 p 所指向的已分配的动态空
```



###### 	4、realloc

```
作用——重新分配 malloc 或 calloc 函数获得的动态空间大小，将 p 指向的动态空间大小改变为 size，p 的值不
变，分配失败返回 NULL
​ realloc(p, 50); // 将 p 所指向的已分配的动态空间 改为 50 字节
```



###### #5、案例

```c
#include <stdlib.h>
#include <stdio.h>
int main() {
void check(int *);
int * p,i;
// 在堆区开辟一个 5 * 4 的空间，并将地址 (void *） ， 转成 (int *) , 赋给 p
p = (int *)malloc(5*sizeof(int));
for( i = 0; i < 5; i++){
scanf("%d", p + i);
}
check(p);
//free(p); //销毁 堆区 p 指向的空间
getchar();
getchar();
return 0;
}
void check(int *p) {
int i;
printf("\n 不及格的成绩 有: ");
for(i =0; i < 5; i++){
if(p[i] < 60) {
printf(" %d ", p[i]);
}
}
}
```



#### (15)结构体

​	struct	结构体名称 { 

​			// 结构体名首字母大写，比如 Cat, Person 成员列表;

​	 };

​	

​	举例: 

​	struct Student{

​		char *name; //姓名 

​		int num; //学号 

​		int age; //年龄

​	};



##### 	<1>结构体变量

###### 		1、先定义结构体，再创建结构体变量

​		strict  结构体 变量1，变量2……

```c
struct Stu{
char *name; //姓
int num; //学号
int age; //年龄
char group; //所在学习小组
float score; //成绩
};
struct Stu stu1, stu2;
//定义了两个变量 stu1 和 stu2，它们都是 Stu 类型，都由 5 个成员组成
//注意关键字 struct 不能少
```



###### 			2、定义结构体的同时定义结构体变量

​		struct	结构体名称 { 

​			// 结构体

​	 } 变量1，变量2；

​	

​				//只需要这两个变量，后续不再定义其他变量，可以不定义结构体名

```c
struct Stu{
char *name; //姓名
int num; //学号
int age; //年龄
char group; //所在学习小组
float score; //成绩
} stu1, stu2;
//在定义结构体 Stu 的同时，创建了两个结构体变量 stu1 和 stu
```



#### (16)共用体

union 共用体名{ 

​	成员列表 		//公用数据空间，以占用最大的空间成员为准

}

![image-20220401221726765](C:\Users\98680\AppData\Roaming\Typora\typora-user-images\image-20220401221726765.png)



用法：

struct	结构体名称 { 

​			// 结构体名

​		union{ 

​			成员列表

​		 } sc;//sc 是一个共用体变

​	 };



```c
#include <stdio.h>
#define TOTAL 2 //人员总数
struct Person{
char name[20]; //name
int num; //编号
char sex;//性别 f => 女 m=>男
char profession;//职员 s=>学生 t=>老师
union{
float score;
char course[20];
} sc;//sc 是一个共用体变量
};
void main(){
int i;
struct Person persons[TOTAL]; // 定义了一个结构体数组
//输入人员信息
for(i=0; i<TOTAL; i++){
printf("Input info: ");
scanf("%s %d %c %c", persons[i].name, &(persons[i].num), &(persons[i].sex), &(persons[i].profession));
if(persons[i].profession == 's'){ //如果是学生
printf("请输入该学生成绩:");
scanf("%f", &persons[i].sc.score);
}else{ //如果是老师
printf("请输入该老师课程:");
scanf("%s", persons[i].sc.course);
}
fflush(stdin);//刷新
}
//输出人员信息
printf("\nName\t\tNum\tSex\tProfession\tScore / Course\n");
for(i=0; i<TOTAL; i++){
if(persons[i].profession == 's'){ //如果是学生
printf("%s\t\t%d\t%c\t%c\t\t%f\n", persons[i].name, persons[i].num, persons[i].sex, persons[i].profession, persons[i].sc.score);
}else{ //如果是老师
printf("%s\t\t%d\t%c\t%c\t\t%s\n", persons[i].name, persons[i].num, persons[i].sex, persons[i].profession, persons[i].sc.course);
}
}
getchar();
getchar()；
}
```





























# 三、指针

​		

​		1、概念：

​				存储数据的一个地址，类似于门牌号，通过这个门牌号能找到存储在里面的数据

```c
#include<stdio.h>

int main() {
	
	int num = 10;
	int* ptr = &num;
	printf("\n num的值：%d \n num的地址：%p", num, &num); 
	printf("\n ptr的地址：%p \n ptr存放的值的一个地址：%p \n ptr存放的值：%d", &ptr,ptr,
           *ptr);
			/*num的值：10
			num的地址：0000005C8CAFF8C4
			ptr的地址：0000005C8CAFF8E8
			ptr存放的值的一个地址：0000005C8CAFF8C4
			ptr存放的值：10*/
} 
```



### 		2、值传递和地址传递



​				默认**传递值**的类型：	基本数据类型 (整型类型、小数类型，字符类型), 结构体,  共用体。

​				默认**传递地址**的类似：	指针、数组

 

### 3、指针的自增和自减

​			自增和自减都是4字节，等于一个存储区域

```c
#include <stdio.h>
const int MAX = 3;//常量
int main () {
int var[] = {10, 100, 200}; // int 数组
int i, *ptr; // ptr 是一个 int* 指针
ptr = var; // ptr 指向了 var 数组的首地址
for ( i = 0; i < MAX; i++) {
	printf("var[%d] 地址= %p \n", i, ptr );
	printf("存储值：var[%d] = %d\n", i, *ptr );
	ptr++;// ptr = ptr + 1(1 个 int 字节数); ptr 存放值+4 字节(int)
    	  // 指向下一个元素的地址
}
getchar();
return 0;
}
```



### 4、指针数组

​		比如： int *ptr[3]; 

​		<1>ptr 声明为一个指针数组 

​		<2>由 3 个整数指针组成。因此，ptr 中的每个元素，都是一个指向 int 

```c
#include <stdio.h>
void main() {
//定义一个指针数组，该数组的每个元素，指向的是一个字符串
char *books[] = {
"三国演义", "西游记", "红楼梦", "水浒传" };
char * pStr = "abc";
//遍历
int i, len = 4;
for(i = 0; i < len; i++) {
printf("\nbooks[%d] 指向字符串是=%s pStr 指向的内容=%s", i , books[i], pStr);
}
getchar();
}
```



### 4、多重指针

​		''指向指针的指针''

​		当一个目标值被一个指针间接指向到另一个指针时，访问这个值需要使用两个星号运算符, 比如 **ptr

![image-20220322180418310](C:\Users\98680\AppData\Roaming\Typora\typora-user-images\image-20220322180418310.png)



### 5、传递指针

 		当函数的形参类型是指针类型时，是使用该函数时，需要传递指针，或者地址，或者数组给该形参

![image-20220322192202083](C:\Users\98680\AppData\Roaming\Typora\typora-user-images\image-20220322192202083.png)



### 6、指针函数

​		returnType(*pointerName)( param list)

```
 returnType 为函数指针指向的函数返回值类型
​ pointerName 为函数指针名称
​ param list 为函数指针指向的函数的参数列表
​ 参数列表中可以同时给出参数的类型和名称，也可以只给出参数的类型，省略参数的名称
​ 注意( )的优先级高于*，第一个括号不能省略，如果写作
returnType*pointerName(param list);就成了函数原型，它表明函数的返回值类型为 returnType 
```





### 7、指针比较

```c
#include <stdio.h>
int main () {
int var[] = {10, 100, 200};
int *ptr
ptr = var;//ptr 指向 var 首地址(第一个元素)
if(ptr == var[0]) {//错误,类型不一样 (int *) 和 (int )
	printf("ok1");
}
if(ptr == &var[0]) { // 可以
	printf("\nok2"); //输出
}
if(ptr == var) { //可以
	printf("\nok3"); //输出
}
if(ptr >= &var[1]) { //可以比较,但是返回 false
	printf("\nok4");//不会输出
}
getchar();
return 0;
```



### #8、回调函数



### 9、空指针

```
 指针变量存放的是地址，从这个角度看指针的本质就是地址。
​ 变量声明的时候，如果没有确切的地址赋值，为指针变量赋一个 NULL 值是好的编程习惯。
​ 赋为 NULL 值的指针被称为空指针，NULL 指针是一个定义在标准库 <stdio.h>中的值为零的常量 #define NULL 0
```































