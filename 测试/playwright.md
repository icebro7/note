# playwright

### 一、安装与启动

#### 1、下载安装

```cmd
# 1、下载库
pip install playwright

# 2、下载配套浏览器
playwright install

to (C:\Users\98680\AppData\Local\ms-playwright\)
```

  

#### 2、简单示例

```python
from playwright.sync_api import sync_playwright

# 启动playwright driver进程
p = sync_playwright().start()

# 启动google浏览器
browser = p.chromium.launch(headless=False)

# 创建页面进行操作
page = browser.new_page()
page.goto("https://www.baidu.com")
print(page.title())

# 输入框输入数据，进行点击操作
page.locator("#kw").fill('游戏')
page.locator('#su').click()

# 关闭浏览器
browser.close()
# 关闭驱动进程
p.stop()
```



#### 3、自动化代码助手

```cmd
# 启动命令,主要是记录人对页面的输入
playwright codegen 
```



#### 4、跟踪

```cmd
// 示例
from playwright.sync_api import sync_playwright

# 启动playwright driver进程
p = sync_playwright().start()

# 启动google浏览器，有头模式
browser = p.chromium.launch(headless=False)

# 创建BrowserContext对象
context = browser.new_context()
# 启动跟踪功能
context.tracing.start(snapshots=True, sources=True, screenshots=True)

# 创建页面进行操作
page = browser.new_page()
page.goto("https://www.baidu.com")
print(page.title())

# 输入框输入数据，进行点击操作
page.locator("#kw").fill('游戏')
page.locator('#su').click()

# 自带等待函数
page.wait_for_timeout(1000)

# 获取内容
lcs = page.locator("._3rqxpq2 fc-8567ba6c00143fc6 _3rqxpq2 EC_result new-pmd c-container").all()
for lc in lcs:
    print(lc.inner_text())

# 暂停跟踪，获取跟踪文件
context.tracing.stop(path="trace.zip")


# 关闭浏览器
browser.close()
# 关闭驱动进程
p.stop()



// 查看跟踪文件
# 在线
https://trace.playwright.dev/

# 本地 
playwright show-trace trace.zip


```



#### 5、playwright定位

```python
# 根据文本内容定位
lcs = page.get_by_text('4399').all()

# 根据元素角色定位
lcs = page.get_by_role('alert').all()


```















