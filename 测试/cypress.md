# cypress

### 1、环境要求：

```cmd
node -v
npm -v
```



### 2、创建目录：

```cmd
# 使用cnpm替换npm，速度更快
npm install -g cnpm --registry=https://registry.npmjs.org

# 下载cypress
cnpm install cypress --save

# 打开界面
.\node_modules\.bin\cypress open

# 运行代码命令
npx cypress open
```



### 3、代码编写：

```json
// 创建jsconfig.json配置文件
{
    "include": [
        "/node_modules/cypress",
        "cypress/**/*.js"
    ]
}

// e2e文件夹中新建integration测试用例存放位

// 简单示例代码“

 describe("导航",() =>{
    it("百度",() =>{
      // 访问百度首页
      cy.visit("https://www.baidu.com/")
      // 访问页面
      cy.visit("https://www.baidu.com/s?ie=utf")
      // 后退
      cy.go("back")
      // 前进
      cy.go(1)
      // 刷新
      cy.reload(true)
      //url
      console.log(cy.url())
      //title
      console.log(cy.title())
    })
 })
```



### 4、命令行启动

```js
// 指定要运行的测试文件
cypress run --spec "cypress/e2e/my-test.spec.js"

// 运行所有的测试文件
cypress run

// 指定浏览器
cypress run --browser chrome

// 以有头模式运行测试
cypress run --headed

```





### 5、常用参数

```javascript
# DOM 操作命令
// 根据元素查找
cy.get('#submit-button', { timeout: 10000 });

// 根据文本查找
cy.contains('Login', { matchCase: false });

// 输入文本
cy.get('input').type('Hello{enter}', { delay: 100 });

// 获取 div 元素中的所有 p 元素
cy.get('div').find('p')；

// 获取页面中第一个 li 元素
cy.get('li').first()

// 获取页面中最后一个 li 元素
cy.get('li').last()

// 获取页面中索引为 2 的 li 元素
cy.get('li').eq(2)

// 滚动页面到元素
cy.get('#inputEmail').scrollIntoView().type("hello")

// 滚动页面到顶端、底部、指定位置
cy.scrollTo('top')
cy.scrollTo('bottom')
cy.scrollTo(1，250)


# 浏览器操作命令
// 访问指定的 URL
cy.visit('https://example.com')

// 重新加载页面(强制)
cy.reload(true)

// 在浏览器历史中前进或后退
cy.go(1)
cy.go(-1)

// 获取页面标题
cy.title()

// 获取页面 URL
cy.url()

# 断言命令
// 对当前元素进行断言
cy.get('h1').should('have.text', 'Welcome')

// 用于组合多个断言
cy.get('input').should('be.visible').and('have.value', 'test')

// 用于否定断言
cy.get('button').should('not.be.disabled')

# 表单操作
// 在输入框中输入文本
cy.get('input').type('Hello, World!')

// 勾选复选框或单选按钮
cy.get('input[type="checkbox"]').check()

// 取消勾选复选框
cy.get('input[type="checkbox"]').uncheck()

// 在下拉菜单中选择选项
cy.get('select').select('option1')

// 提交表单
cy.get('form').submit()

# 网络请求命令
// 发送 HTTP 请求
cy.request('POST', '/api/login', { username: 'test', password: '123456' })

// 拦截网络请求并提供模拟响应
cy.intercept('GET', '/api/data', { data: [1, 2, 3] })

# 其他命令
// 等待指定的时间
cy.wait(2000)

// 在 Cypress 命令日志中打印消息
cy.log('Test message')

// 加载 fixture 文件中的数据
cy.fixture('data.json')

// 滚动到指定的位置
cy.get('div').scrollTo('bottom')
```















































