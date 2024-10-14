# WEB自动化

  

## 一、网页定位

#### 1、html定位

```python
id定位:
driver.find_element(By.ID,"kw").send_keys("码尚学院")

name定位:
driver.find_element(By.NAME,"wd").send_keys("码尚学院")

link_text定位
driver.find_element(By.LINK_TEXT,"新闻").clicK()

partail_link_text定位
driver.find_element(By.PARTIAL_LINK_TEXT, "新"").click()
```



#### 2、xpath定位 

```python
绝对路径∶
/开头是绝对路径
/htmlbodyldiv[1]/div[1]div[5]/dividiv/form/span[1]y/input
相对路径∶
//开头是相对路径
//input

1.相对路径+索引定位∶ 
//form/span[1]/input
                   
2.相对路径+属性定位︰
//input[@autocomplete='off' and @name='wd']

3.相对路径+通配符定位*∶
//*[@autocomplete='off']
//*[@*='off']

4.相对路径+部分属性值定位∶
以开头:
//*[starts-with(@autocomplete,'of')]
以结尾:
//*[substring(@autoco mplete,2)='ff']
包含:
//*[contains(@autocomplete 'of')]
              
5.相对路径+文本定位
//span[text()='按图片搜索']
```



#### 3、cookie登陆

```python
# 获取登陆cookie
    def cookie_test(self):
        global driver
        self.driver = webdriver.Chrome()
        driver = self.driver
        driver.get("https://www.16personalities.com/ch/%E4%BA%BA%E6%A0%BC%E6%B5%8B%E8%AF%95")
        cks = driver.get_cookies()
        for i in cks:
            print(i)
   
# 注入cookie
    def cookie_test(self):
        global driver
        self.driver = webdriver.Chrome()
        driver = self.driver
         driver.get("https://www.16personalities.com/ch/%E4%BA%BA%E6%A0%BC%E6%B5%8B%E8%AF%95")
        driver.add_cookie("{'domain': '.16personalities.com', 'expiry': 173363 9096, 'httpOnly': False, 'name': '_ga', 'path': '/', 'sameSite': 'Lax', 'secure': False, 'value': 'GA1.2.193279096.1699079096'}")

```



## 二、Unittest

```python
常用套件：
1、testcases：测试用例
2、testsuites：测试套件
3、testfixtures：测试固件/夹具
4、testloader：测试加载器
5、testrunner：测试运行器

用例的前置和后置：
1、setUp/tearDown:用例前后运行一次
	打开浏览器、加载网页/关闭网页
	
2、setUpClass/tearDownClass：类运行前后运行一次 
	创建数据库链接，创建日志对象/关闭数据库链接，摧毁日志对象，需要加上装饰器：@classmethod 
	
3、setUpModule/tearDownModule：模块前后运行一次 
	#不在类中


忽略测试用例：
@unittest.Skip(msg)
@unittest.ifSkip(true,msg)
@unittest.UnlessSkip(false,msg)



断言：
assertEqual(a,b):断言a==b
assertNotEqual(a,b):断言a！==b

assertTrue(a):断言a为真 
assertFalse(a):断言a为假

assertIn(a,b):断言a在b里面
assertNotin(a,b):断言a不在b里面
	

报告：
htmltestrunner

数据启动：
ddt

用例分类执行：
默认执行所有，可用testsuite执行部分用例，或者-k参数(通配：-k *_xx)

```



#### 1、基础操作

```python
import unittest
from enium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.select import Select


class TestCase(unittest.TestCase):

    def setUp(self):
        self.driver = webdriver.Chrome()
        # 打开浏览器

    def test_example(self):
        self.driver.get('https://item.jd.com/10055931096628.html#crumb-wrap')
        # 打开浏览器页面

        self.driver.find_element(By.NAME, "loginname").send_keys("admin")
        self.driver.find_element(By.NAME, "password").send_keys("123456")
        self.driver.find_element(By.XPATH, "//*[@id = 'loginsubmit']").click()
        # 登陆部分

        self.driver.implicitly_wait(10)
        # 等待加载，防止反应过快查找不到元素

        self.driver.switch_to.frame("框架名")
        # 如果页面有框架，那就得先进入框架再执行操作
        self.driver.switch_to.default_content()
        # 出框架，需要清楚知道是否有跨框架的操作

        self.driver.find_element(By.LINK_TEXT, "点击的按钮名").click()
        # 点击页面中的链接

        sel = Select(self.driver.find_element(By.NAME, "下拉框名称"))
        sel.select_by_value("3")
        sel.select_by_index("3")
        # 下拉框的选中，value中的值是显示几个下拉框，index中的是下标第几个下拉框
        
        self.driver.find_element(By.NAME, "site").send_keys(r"E://img")
        # 在页面中上传文件

        shop_price = self.driver.find_element(By.NAME, "price")
        shop_price.clear()
        shop_price.send_keys("777")
        # 如果输入框内已经有数据，则先定位，后clear(),最后输入数据

        del_button_list = self.driver.find_elements(By.XPATH, "定位的site")
        if len(del_button_list) > 0:
            del_button_list[0].click()
        else:
            print("无数据")
        # find_elements是查询所有的元素转化为一个列表，通过判断语句来增强代码的健壮性

        ale = self.driver.switch_to.alert
        ale.accept()
        # 处理弹窗。alert(只有确定)、confirm(有确定有取消)、prompt(有确定取消还能输入) 

    def tearDown(self):
        self.driver.quit()
        # 关闭浏览器
```



```python
# 开启测试	


if __name__ == '__main__':
    unittest.main()
    # 启动全部测试
    
    suite = unittest.TestSuite()
    testcases = [TestCase("test01"),TestCase("test02")]
    suite.addTests(testcases)
    # 测试套件运行
    
    unittest.TextTestRunner(verbosity=2).run(suite)
    # 运行器运行，以具体的模式输出
    
   unittest.defaultTestLoader.discover(start_dir=os.getcwd(), pattern='test*.py')
	# 默认的发现测试运行
    
    
    
    
# 忽略测试用例
    @unittest.skip()
    def test_01_login(self):
        pass
    # 直接忽略
    
    @unittest.skipIf(a > 10, "如果成立则忽略")
    def test_01_login(self):
        pass
    # 条件成立则忽略
    
    @unittest.skipUnless(a > 10, "如果不成立则忽略")
    def test_01_login(self):
        pass
    # 条件不成立则忽略
   
```



#### 2、POM项目构造

 

```
把项目进行分化，底层架构将一些基础的操作进行封装，如元素的查找、操作与输入删除等操作。

中间层将具体的一个个页面进行操作，直接调用底层的功能，实现功能的复用。

测试层将需要实现的最上层功能，调用中间层写好的页面设置直接测试。
```

 

##### Base底层架构

```python
from selenium import webdriver
from selenium.webdriver.support.select import Select
from selenium.webdriver.common.by import By


class BaseSet:

    def __init__(self):
        global driver
        self.driver = webdriver.Chrome()
        driver = self.driver
        # 打开浏览器
        self.driver.get('https://item.jd.com/10055931096628.html#crumb-wrap')
        # 打开浏览器页面

    # 定位元素的关键字
    def locator_element(self, loc):
        return self.driver.find_element(*loc)

    # 设置值的关键字
    def send_Mykeys(self, loc, value):
        self.locator_element(loc).send_keys(value)

    # 实现属性的点击效果
    def click(self, loc):
        self.locator_element(loc).click()

    # 清除属性(文本框)中的内容
    def value_clear(self, loc, ):
        self.locator_element(loc).clear()

    # 进入框架
    def goto_frame(self, frame_name):
        self.driver.switch_to.frame(frame_name)

    # 出框架
    def quit_frame(self):
        self.driver.switch_to.default_content()

    # 下拉框
    def pull_down(self, loc, value):
        sel = Select(self.locator_element(loc))
        sel.select_by_value(value)
        # sel.select_by_index("3")
        # 下拉框的选中，value中的值是显示几个下拉框，index中的是下标第几个下拉框

    # 页面等待时间
    def page_wait(self, long):
        self.driver.implicitly_wait(long)

    # 获取文本的值
    def get_value(self, loc):
        return self.locator_element(loc).text

    # 向页面提交文件
    def post_file(self, loc, file):
        self.locator_element(loc).find_element(file)

    # 查找页面元素，判断他是否为空，如果为空，则返回信息，不为空则进行相应的操作，以下代码为点击操作
    def judge_list_isnull(self,loc):
        del_button_list = self.locator_element(loc)
        if len(del_button_list) > 0:
            del_button_list[0].click()
        else:
            print("无数据")
        # find_elements是查询所有的元素转化为一个列表，通过判断语句来增强代码的健壮性

    # 弹窗处理
    def alert_approach(self):
        ale = self.driver.switch_to.alert
        ale.accept()
    # 处理弹窗。alert(只有确定)、confirm(有确定有取消)、prompt(有确定取消还能输入)

```



##### 中间层

​	在此使用的是页面操作代码

```python
from base.base_set import BaseSet
from selenium.webdriver.common.by import By


class ProductPage(BaseSet):
    # 页面元素
    exit_loc = (By.LINK_TEXT, "退出")
    click_loc = (By.LINK_TEXT, "点击的按钮名")
    file_loc = (By.NAME, "site")
    prise_site = (By.NAME, "price")
    list_loc = (By.XPATH, "定位的site")
    location_file_site = r"E://img"

    def page_operation(self):
        # 页面动作

        self.goto_frame("框架名")
        # 进入框架
        self.page_wait(10)
        # 等待加载，防止反应过快查找不到元素
        self.quit_frame()
        # 出框架，需要清楚知道是否有跨框架的操作
        self.click(ProductPage.click_loc)
        # 点击页面中的链接
        self.post_file(ProductPage.file_loc, ProductPage.location_file_site)
        # 在页面中上传文件
        self.value_clear(ProductPage.prise_site)
        # 清除输入框内的内容
        self.send_Mykeys(ProductPage.prise_site, "777")
        # 如果输入框内已经有数据，则clear后输入数据
        self.judge_list_isnull(ProductPage.list_loc)
        # 查询一整个页面list列表，并且判断元素是否存在，再进行操作
        self.alert_approach()
        # 处理弹窗。alert(只有确定)、confirm(有确定有取消)、prompt(有确定取消还能输入)


    # 断言
    def get_except_result(self):
        self.goto_frame("框架名")
        return self.get_value(ProductPage.exit_loc)

```





##### 测试层

```python
import unittest
from pageobject.login_page import LoginPage
from pageobject.product_page import ProductPage


class TestCase(unittest.TestCase):

    def test_01_login(self):

        # 登陆
        user = "admin"
        password = "123456"
        log = LoginPage()
        log.login_input(user, password)

        # 页面操作
        pro = ProductPage()

        # 断言
        self.assertEquals(pro.get_except_result(),"退出")
```





#### 3、DDT+EXCEL数据驱动

```
DDT：数据驱动测试。可以和Unittest结合实现数据驱动

DDT使用方式：通过装饰器使用

装饰器：
@ddt装饰类：作用是用于申明当前类使用ddt数据驱动。
@data：函数装饰器，用于给测试用例传递数据

@unpack装饰函数：
函数装饰器，作用是数据解包
不能解数字或者字符串，
解元组和列表，参数的个数需和解完包后的个数一致
字典解包，参数的名字和个数必须和字典的键保持一致

@file_data装饰函数：函数装饰器，作用是直接读取yaml、json文件。
```



```python
import os
from openpyxl.reader.excel import load_workbook

class ExcelOper:

    def get_excel_path(self):
        return os.path.abspath(os.path.dirname(__file__)).split("common")[0]
        # 获得文件路径前半段

    def read_excel(self):
        excel_path = os.path.join(self.get_excel_path(), "data", "login.xlsx")
        # 打开 Excel 文件
        workbook = load_workbook(excel_path)
        sheet = workbook['Sheet1']
        # 选择要读取的工作表，这里假设工作表名为 'Sheet1'

        all_list = []
        for rows in range(2, sheet.max_row + 1):
            # 取出行数据，过滤表头
            temp_list = []
            for cols in range(1, sheet.max_column):
                temp_list.append(sheet.cell(rows, cols).value)
            all_list.append(temp_list)
        print(all_list)
```



```python
from ddt import ddt, data

@ddt
class TestCase(unittest.TestCase):
 
    @data(*ExcelOper.read_excel())
    def test_01_login(self,args):
```



#### 4、测试报告:BeautifulReport

```python
# BeautifulReport

import os
import unittest
from BeautifulReport import BeautifulReport

class TestCase:

    def test_01_login(self):
        print("success01")


    def test_02_unitest(self):
        print("success02")


if __name__ == '__main__':

    test_suite = unittest.defaultTestLoader.discover(start_dir=os.getcwd(), pattern='test*.py')
    result = BeautifulReport(test_suite)
    nowtime = time.strftime("%Y-%m-%d %H-%M-%S")
    result.report(filename=nowtime + '测试报告', description='测试deafult报告', report_dir='C:\\Users\\98680\\Desktop\\项目代码\\学习文件\\report',
                  theme='theme_default')
```





## 三、Pytest

#### 1、pytest默认规则：

```python

1.模块名必须以test_开头或者_test结尾
2.测试类必须以Test开头，并且不能有init方法
3.测试方法必须以test开头


用例的前置和后置：
1 、方法级：
	setup_mothod/teardown_mothod
	setup/teardown
2、函数级：
	setup_function/teardown_function
3、类级：
	setup_class/teardown_class
4、模块级：
	setup_module/teardown_module
5、特殊用法：
	函数之前加@pytest.fixture()
	

断言：assert

报告：pytest-HTML,allure

失败重跑：pytest-rerunfailures

数据驱动：@pytrst.mark.parametrize装饰器

用例分类执行：@pytest.mark
```

 



#### 2、详细参数 

```python
# 一、详细参数
-s: 输出调式信息，包括print打印的信息
-v：显示更详细的信息
-vs：两个参数一起用
-n：支持多线程或分布式运行测试用例
--reruns =num ：失败重跑 
-x：只要有一个测试报错，停止
-maxfail=2：出现两个测试报错，停止 
-k：测试部分指定字符串的用例
--htmk ./report/report.html :生成测试报告





# 二、运行测试用例的方法
1、指定所有：pytest .main()
2、指定模块：pytest.main(['-vs','test_01.py'])
3、指定目录：pytest.main(['-vs','/test_case'])  
4、nodeid指定用例：pytest.main(['-vs','./testcase/test_case.py::TestCase::test_01_login'])





# 三、使用装饰器运行测试用例
@pytest.mark.parametrize("datainfo", ExcelOper().read_excel())
    def test_01_login(self, datainfo):
        print(datainfo)
        
 



# 四、读取pytest.ini全局配置文件运行
pytest.int是pytest单元测试框架的核心配置文件
1、site：项目根目录
2、编码：ANSI，可以使用notpad++修改编码格式，必须重写
3、作用：改变pytest默认的行为
4、运行规则：主函数和命令行模式运行，都会读取这个配置文件

# pytest.ini文件
[pytest]
addopts = -vs		# 命令行的参数，用空格分隔
testpaths = ./testcase		# 测试用例的路径
python_files = test_*.py		# 模块名的规则
python_classes = Test*		# 类名的规则
python_functions = test		# 方法名的规则 





# 五、分组执行
冒烟、分模块执行、分接口和web执行
 
1、smoke：冒烟用例，可以分布在各个模块里面
2、标记冒烟：@pytest.mark.smoke
3、pytest -m "smoke or manager"



# 六、跳过用例
@pytest.mark.skip(reason=)		# 无条件跳过

@pytest.mark.skipif(age>=18,reason=)		#有条件跳过


# 七、@pytest.fixture()实现部分用例的前后置
@pytest.fixture(scope="",params="",autouse="",ids="",name="")
1、scope：表示被标记的作用域。function(默认)，class，module，package/session
2、params：参数化，return request.param
3、autouse=True：自动执行，默认False
4、ids：当使用params参数化时，给每一个值设置一个变量名，
5、name：表示给被@pytest.fixture标记的方法去一个别名,取别名后原来名称不能使用



# 八、 conftest.py + @pytest.fixture()实现全局配置前置应用
1、conftest.py文件是单独存放的一个夹具配置文件，名称不能修改
2、可以在不同文件中使用同一个fixture函数，无需调用
3、conftest.py需要和运行的函数放在同一个目录里，但是可以引用当前 目录和上一级目录的配置文件



```





#### 3、测试报告:Allure_pytest

```python
1、下载，解压，配置path路径
2、验证是否安装成功。cmd验证成功但是pycharm验证失败，可重启pycharm
3、具体配置：
    pytest.main(['-vs','./testcase/test_case.py','--alluredir','./temp'])
    # 首先启动pytest，指定目录，生成allure临时json报告
    
    os.system('allure generate ./temp -o ./report --clean')
    # 将临时json报告生成html文件，指定目录并且清除以往文件

```



## 四、 YAML文件（留坑）

```python
yaml简介 :
yaml是一种数据格式，支持注释，换行，多行字符串，裸字符串( 整形，字符串 )

1.用于全局的配置文件 ini/yaml
2.用于写测试用例( 接口测试用例)


语法规则:
1.区分大小写
2.使用缩进表示层级(和python一样)，不能使用tab键进，只能用空格

3.缩进没有数量的，只要前面是对其的就行

4.注释是#

数据组成：
1、键值对
2、list：添加横线，对齐的横线就是数组 
```



#### 1、读取和写入

```python
class YamlUtil:

    # 通过init方法把yaml文件传入到这个类
    def __init__(self, yaml_file):
        self.yaml_file = yaml_file

    # 读取yaml文件
    def read_file(self):
        with open(self.yaml_file, encoding='utf-8') as f:
            value = yaml.load(f, Loader=yaml.FullLoader)
            print(value, type(value))
            
    # 写入yaml文件
    def write_file(self):
        with open(self.yaml_file, encoding='utf-8',mode='w') as f:
            data = [{'text': [{'text01': 'aaa'},{'text02': 'bbb'}]}]
            yaml.dump(data,f,allow_unicode=True)
    
    # md5加密
    def md5_encode(self, args):
        args = str(args).encode("utf-8")
        # 将输入的值转换为utf-8格式
        md5_value = hashlib.md5(args).hexdigest()
        # 进行hd5加密
        return md5_value
	# base64加密
    def base64_encode(self,args):
        args = str(args).encode("utf-8")
        # 将输入的值转换为utf-8格式
        base64_value = base64.b64encode(args).decode(encoding="utf-8")
        # 进行base64加密
        return base64_value


if __name__ == '__main__':
    YamlUtil('test_api.yaml').write_file()
```



#### 2、Yaml实现接口自动化

```yaml
-
    feature: 用户管理模块
    story: 获取access_token鉴权码接口
    title: 获取鉴权码接口成功
    request : 
        url: https://api.weixin.qq.com/cgi-bin/token
        method: get
         headers :
            ACCept : "*/*1
            Content-Type: application/json
        params :
            grant type: client credential
            appid: wx6b11b3efd1cdc290
            secret: 106a9c6157c4db5f6029918738f9529d
    assert :
        eq：
            expires in: 7200
```



## 五、邮件发送

#### 1、创建邮件管理

```python
import smtplib
from email.mime.text import MIMEText
from email.mime.multipart import MIMEMultipart 

class EmailManage():
    def send_email(self，report_name):
        # 创建邮件服务器
        emailserver = 'smtp.qq.com'
 
        # 邮箱的账号密码
        username = '986807058@qq.com'
        password = 'gvszkzvtkknybdae'

        # 接收方
        receiver = '1915790282@qq.com'

        # 创建邮件对象
        message = MIMEMultipart('related')
        subject = '邮箱测试'
        fujian =  MIMEText(open(report_name,'wb').read(),'html','utf-8')

        # 把邮件信息封装进邮件对象里
        message['from'] = username
        message['to'] = receiver
        message['subject'] = subject
        message.attach(fujian)

        # 登陆smtp服务器并发送邮件
        smtp = smtplib.SMTP()
        smtp.connect(emailserver)
        smtp.login(username,password)
        smtp.sendmail(username,receiver,message.as_string())
        smtp.quit()


```



 

 





















 



































































































































