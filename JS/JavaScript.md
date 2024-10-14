# JavaScript



# 一、js的三部分



 	1.ECMAScript:

​		规定基本的语法

2. DOM:

   操作页面各种元素

   

3. BOM：

   操作浏览器窗口

   

   

# 二、JS的三种书写格式



### 1、行内式：

​	在元素内编写代码



### 2、内嵌式：

​	在 <script>中编写JS代码

​	比较清晰可见



### 3、外部式：

​	把JS代码独立在HTML文件之外

​	不在在 <script>的中间编写JS代码



# 三、基本语法



### 1、输入输出

```html
	prompt('输入框')
	
	alert('弹出的警示框，输出给用户看的')
	
	console('控制台输出，给程序猿看的')
```

### 2、变量

​	存放数据的容器

```html
声明变量:
var age;

赋值：
age = 1；

声明初始化：
var name = 2；
  
输出结果：
console.log(age)；
console.log(name);

只声明不赋值：
undefined

不声明，直接赋值：
可以直接使用，变成全局变量
```



​	交换两个变量的值：

```html
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script>
    var temp 
    var xx1 = 'xx1';
    var xx2 = 'xx2';
    temp = xx1;
    xx1 = xx2;
    console.log(temp);
    console.log(xx1);
    </script>

</head>
```



### 3、数据类型

#### 		简单数据类型：Number,String,Boolean,Undefined,Null

#### 		复杂数据类型：object



#### 		(1)Number

##### 			八进制：

​				0~7

​				在程序数字前面加0，表示八进制



##### 			十六进制：

​				0~9，a~f

​				在程序数字前面加0x，表示十六进制



##### 			数值最大：

​				Number.MAX_VALUE



##### 			数值最小：

​				Number.MIN_VALUE

​				

##### 			无穷大：

​				Infinity



##### 			无穷小：

​				-Infinity



##### 			非数值：

​				NaN

​	

##### 			判断非数字isNaN：

```html
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script>
    console.log(isNaN('XXX')); //true
    console.log(isNaN(123)); //false
    </script>

</head>
```



#### 		(2)字符串类型String

​				字符串转义符

​					\n ：换行

​					\\ \  ： 斜杠



#### 		(3)布尔型Boolean

​				参与数值运算

​				true ： 1

​				false ： 0



### 		4、声明变量未赋值 undefined

​				undefined 和数字相加 最后结果为NaN



### 		5、获取变量数据类型 typeof

```html
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script>
    var xx1 = 'xxx'
    var xx2 = 123;
    var xx3 = false
    console.log(typeof(xx1)); //string
    console.log(typeof(xx2)); //number
    console.log(typeof(xx3)); //boolean
    </script>

</head>
```

 

### 		6、转换为字符串：

​				toString()   var  num = 1 ; alert(num.toString());

​				String()      var  num = 1 ; alert(String(num));

​				**加号拼接**    var  num = 1 ; alert(num + '"字符串"')；

 

### 	7、转换为数字型：

​				**parselnt(string)**   		  转换为整数     	  parseInt('78')				

​				**parseFloat(string)**    	 转换为浮点数	   parseFloat('78.12')

​				NumBer()       			   强制转换数值型   Number('12')

​				隐式转换						算术运算转换	  '12' - 0



### 		8、转换为布尔值：

​			Boolean()						  强制转换			   Boolean('true')

​			代表为空、否定的值会被转换为false ，如 '' ,0 , NaN , null , undefined

​			其余值都会被转换为true



### 	9、算术运算符

​				递增：

​						前置递增运算符：			（先自加，后运算）

```html
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script>
    var xx1 = 1
    ++ xx1 //xx1 = xx1 + 1
    console.log(xx1); //2
    </script>
</head>
```



​						后置递增运算符：			（先原值运算，后自加）

```html
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script>
    var xx1 = 1
    xx1 ++ //xx1 = xx1 + 1
    console.log(xx1); //2
    </script>
</head>
```



### 		10、比较运算符：

​					返回结果为true 和 flase

​					== ：	 默认转换数据类型

```html
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script>
    
    console.log(10 == 10);      //true
    console.log(10 == '10');    //true
    
    </script>

</head>
```



​				===：		要求值和数据类型都一模一样

```html
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script>
    
    console.log(10 === 10);      //true
    console.log(10 === '10');    //false
    
    </script>

</head>
```



### 		11、逻辑运算符

​					&& ： 全真为真

​					||    ： 全假为假



​					逻辑与短路运算：			

​							前面若为0，短路，则不运算后面的表达式

​							值参与逻辑运算，如果表达式1为真，则返回表达式2，否则表达式1

```html
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script>
    
    console.log(123 && 456);      //456
    console.log(0 && 1 + 2 && 345 *6789 )；  //0
    
    </script>

</head>
```



​					逻辑或短路运算：

​							值参与逻辑运算，如果表达式1为真，则返回表达式1，否则表达式2

```html
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script>
    
    console.log(123 || 456);      //123
    console.log(0 || 456);      //456
       
    var num = 0;
    console.log(123 || num++);   //123
    console.log(0 || num);       //0
    </script>

</head>
```



### 		12、三元表达式

​				条件表达式  ？ 表达式1  ：  表达式2

​				//  如果条件表达式为真，返回表达式1的值，  否则返回表达式2的值

```html
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script>
    var xx1 = 0
    var xx2 = xx1 <9 ? 123 :456
    
    console.log(xx2);      //123
    
    </script>

</head>
```



### 		13、switch语句

​				变量里的值和case的值相匹配的时候是	全等		===

```html
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script>
        var xx1 = 0
        switch (xx1) {
            case 2:
                console.log(2);
                break;
            case 1:
                console.log(1);
            case 0:
                console.log(0);
                break;
            default:
                console.log('没有此数字');
            //输出为 0

        }

    </script>
```



### 	14、循环

#### 			(1)for循环：

​					for(初始化变量；条件表达式；操作表达式){

​						循环体

​					}

```html
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script>
        for(var i=0;i<10;i++){
            console.log('嗨咯');
        }
        //10句嗨喽
    </script>

</head>
```



####  		(2)while循环：

​					while (判断式){

​						循环体

​					}

```html
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script>
       var j = 1;
       var sum = 0;
       while(j <=100){
           sum +=j;
           j ++;
       }
       console.log(sum); //5050
    </script>

</head>
```



### 	15、数组

#### 			(1)遍历数组：

```html
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script>
       var j = [1,2,3,4]
       for(var i = 0; i<4 ;i++) {
        console.log(j[i]);  // 1    2   3   4
       }
       
    </script>

</head>
```



####   			(2)数组求和：

```html
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script>
      var varray = [3,5,7,9];
      var sum = 0;
      for (var i = 0; i < varray.length; i ++){
          sum+= varray[i];
      }
      console.log(sum);

    </script>

</head>
```



#### 			(3)筛选数组：

```html
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script>
      var varray = [3,5,7,9,11,13,15];
      var newvarray = [];
      var j = 0;
      for(var i = 0;i < varray.length; i++){
          if (varray[i] > 10){
              newvarray[j] = varray[i];
          j ++;
          }
      }
      console.log(newvarray);  //11  13  15
    </script>

</head>

```



#### 			(4)数组翻转：

```html
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script>
      var varray = [3,5,7,9];
      var newvarray = [];
     
      for(var i = varray.length-1;i >=0; i--){
          newvarray[newvarray.length] = varray[i]
      }
      console.log(newvarray);  //9  7   5   3
    </script>

</head>
```



#### 			(5)数组冒泡：

```html
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script>
        var varray = [6,8,5,2,4];
        var newvarray = [];
        var temp;
        for (var i = 0; i<= varray.length - 1; i++){
            for (var j = 0; j<= varray.length - i - 1; j++){
                if (varray[j] < varray[j+1]){
                    temp = varray[j];
                    varray[j] = varray [j + 1];
                    varray[j+1] = temp;
                }
            }
        }
        console.log(varray); //8, 6, 5, 4, 2
    </script>

</head>
```



#### (6)利用new Array创建数组：

​		var	arr	=	new	Array()	//创建空数组

​		var	arr	=	new	Array(2)	//创建一个数组长度为2，有两个空的数组元素

​		var	arr	=	new	Array(2，3)	//等价于[2，3]



#### (7)监测是否为数组：

​		<1>instanceof

​				变量	instanceof	Array	//返回为布尔值

```html
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script>
    var arr = [];
    var object = {};
    console.log(arr instanceof Array);      //true
    console.log(object instanceof Array);   //false
    </script>
</head>
```



​		<2>Array.isArray

​			Array.isArray(变量名)		//返回为布尔值

```html
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script>
    var arr = [];
    var object = {};
    console.log(Array.isArray(arr));      //true
    console.log(Array.isArray(object));   //false
    </script>
</head>
```







### 		16、函数

#### 			(1)形参实参匹配:

​				实参小于形参： NaN

​				实参大于形参：取形参个数

```html
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script>
        function getsum(n1,n2){
            console.log(n1 + n2);
        }
        getsum(1)       //NaN
    </script>

</head>
```



#### 		(2)return的返回值:

​				return返回两个值时，取最后一个值

```html
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script>
        function getsum(){
            return [1,2],[1,2,3,4]
        }
        console.log(getsum());       //[1,2,3,4]
    </script>

</head>
```



#### 		(3)arguments:

​				伪数组，按照索引的方式进行存储，具有 length的属性

```html
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script>
        function getsum(){
            console.log(arguments[1]); //1
            console.log(arguments.length);  //3
        }
         getsum(1,2,3)
    </script>

</head>
```



#### 		(4)匿名函数：

​				var	变量	=	function(){ }；

​				函数表达式也可以进行传递参数

```html
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script>
        var fun = function(arg){
            console.log('函数表达式');     //函数表达式
            console.log(arg);             //777
        }
        fun('777')
    </script>

</head>
```



### 		17、作用域

#### 			(1)全局变量特殊情况：

​						在函数内没有声明， 直接赋值， 为全局变量



#### 			(2)js中没有块级作用域



#### 	    	(3)作用域链：

​					内部函数访问外部函数的变量，采取链式查找，一级一级查		(就近原则)

```html
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script>
       var num = 10
       
       function fn(){
           var num =20;

           function fun(){
               console.log(num);
           }
           fun()
       }
       fn()		//20
    </script>

</head>
```



​		(4)集体声明：

​					只声明前面的变量

```html
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script>
        function f1() {
             var a,b,c = 9   
       		 //var a = 9; b = 9; c = 9;b 和c没有var声明，直接赋值，为全局变量
        }

    </script>

</head>
```



### 		18、预解析

​				js引擎运行js	分为两步：	预解析	代码执行

​				将js中的所有	var 和	function 提升到当前作用域的最前面

#### 						(1)变量提升：

​						把所有变量声明提升到当前作用域最前面，不提升赋值

<img src="C:\Users\98680\AppData\Roaming\Typora\typora-user-images\image-20220227204252699.png" alt="image-20220227204252699" style="zoom:150%;" />



#### 					(2)函数提升：

​					把所有函数声明提升到当前作用域最前面，不调用函数

```html
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script>
        fn()           //22
        function fn() {
            console.log('22');
        }
    </script>

</head>

```



# 四、对象

​		

### 		1、创建对象

#### 			(1)对象字面量创建对象：

​			//var	obj	=	{  }；

```html
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script>
       var obj = {
           uname:'xxx',
           age: 18,
           sex:'男',
           sayhi:function(){
               console.log('hi~');
           }
       }     
    </script>
</head>
```



#### 			(2)利用	new object创建方法：

​			//var	obj	=	new	object( );

```html
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script>
       var obj = new Object();
       obj.uname = 'xxx';
       obj.age = 18;
       obj.sex = '男';
       obj.sayhi = function(){
           console.log('hi~');
       }
    </script>
</head>
```



#### 			(3)利用构造函数创建对象：

​			function	构造函数名() {						//首个函数名字字母大写

​				this.属性	=	值；

​				this.方法	=	function( ){ 

​				}

​			}

​			new	构造函数名( );

```html
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script>
      function Star(uname,age,sex){
          this.name = uname;
          this.age = age;
          this.sex = sex;
      }
      var xxx = new Star('xxx',18,'男');
      
    </script>
</head>

```



### 		2、使用对象

#### 					(1)调用对象的属性：

​					对象名. 属性名

```html
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script>
       var obj = {
           uname:'xxx',
           age: 18,
           sex:'男',
           sayhi:function(){
               console.log('hi~');
           }
       }
       console.log(obj.age);     //18
    </script>
</head>
```

​	

​					对象名['属性名']

```html
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script>
       var obj = {
           uname:'xxx',
           age: 18,
           sex:'男',
           sayhi:function(){
               console.log('hi~');
           }
       }
       console.log(obj['age']);     //18
    </script>
</head>
```



#### 				(2)调用对象的方法：

​					对象名.方法名( )

```html
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script>
       var obj = {
           uname:'xxx',
           age: 18,
           sex:'男',
           sayhi:function(){
               console.log('hi~');
           }
       }
       obj.sayhi()      //hi~
    </script>
</head>
```



### 		3、遍历对象

​			for(变量	in	对象){

​			console.log(变量)；		//得到属性名

​			console.log(对象[变量])；		//得到属性值，也能遍历出方法

​			}

```html
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script>
      function Star(uname,age,sex){
          this.name = uname;
          this.age = age;
          this.sex = sex;
      }
      var xxx = new Star('xxx',18,'男');
      
      for(var k in xxx){
        console.log(xxx[k]);    //xxx 18  男
      }
    </script>
</head>
```





### 			4、内置对象











































































































































































































































































































