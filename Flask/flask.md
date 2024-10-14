## Flask配置

#### 1、设置中文

```python
#1、文件内配置
app.config['JSON_AS_ASCII'] = False

#2、创建config. py配置文件进行配置
JSON_AS_ASCII = False

#3、加载config.py文件
app.config.from_object(config)

```



#### 2、url与视图函数的映射

```python
from flask import Flask,jsonify

#需要展示的list
books = [
    {"id":1,"name":"三国演义"}
]

@app.route("/book/<int:book_id>")
def book_detail(book_id):
    for book in books：
    	if book_id == book['id']:
            return book 
        
```



#### 3、url_for

```python
from flask import Flask,url_for,jsonify

@app.route("/book/list")
def book_list():
    for book in books：
    	book['url'] = url_for("book_detail",book_id=book['id'])
       return jsonify(books)
```



#### 4、methods

```python
# 设置GET、POST请求
@app.route("/book/list"，methods=['GET','POST'])
```



#### 5、重定向 

```python
from flask import Flask,url_for,jsonify,request

@app.route("/profile")
def profile():
    user_id = request.args.get("id")
    if user_id:
        return "用户个人信息"
    else:
        return redirect(url_for("index"))
```



#### 6、jinja模板、过滤器

```python
from flask import Flask,render_template

# render_template用法
@app.route("/text")
def text():
    context = {
        "username": "x1"
    }
    return render_template("text.html",**context)
# html中：
<div> {{ username}} </div>
<div> {{ username}} </div>



# 过滤器
<div> {{ username | length} </div>
<div> {{ username | join(",")}} </div>
       
 
# 控制语句
<body>
{% if age > 18 %}
       <div>成年</div>
{% endif %}
       
{% for book in books %}
       <li> {{ book }} </li>
{% endfor %}
       
</body>

```



#### 7、模板继承

```html
<!-- 创建主模板和网页模板 -->

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title> {% block title%}{% endblock %} </title>
</head>
<body>
<ul>
    <li>
        <a href="/">首页</a>
    </li>

    <li>
        <a href="text.html">第一页</a>
    </li>

    <li>
        <a href="text2.html">第二页</a>
    </li>
</ul>
{% block body%}{% endblock %}
<footer style="background-color:#cccccc ">底部</footer>
</body>
</html>


<!-- 需求网页继承 -->
{% extends "main.html" %}

{% block title %}
    首页
{% endblock %}

{% block body %}
    <h1>首页</h1>
{% endblock %}
```



#### 8、静态文件加载

```html
<!-- 1、在static文件中创建CSS文件并添加样式 -->

<!-- 2、在模板文件head中添加指引 -->
 {% block head %}{% endblock %}

<!-- 3、在需求文件中引入样式 -->
{% block head %}
        <link rel="stylesheet" href="{{ url_for('static',filename = 'CSS/text.css') }}">
{% endblock %}
```



#### 9、蓝图和子域名

```python
# 1、创建apps包

# 2、创建相对应的python文件
from flask import Blueprint,render_template

# url_prefix:127.0.0.1:5000/book/main
bp = Blueprint("main",__name__,url_prefix="/book")
@bp.route('main')
def app_main():
    return render_template("main.html")

# 3、在主app文件中创建连接
from apps.main import bp as main_bp

app.register_blueprint(main_bp)
```



#### 10、数据库操作

```python
from flask_sqlalchemy import SQLAlchemy

app = Flask(__name__)

# 设置连接数据
HOSTNAME = '127.0.0.1'
PORT     = '3306'
DATABASE = 'text'
USERNAME = 'root'
PASSWORD = '7777777'
DB_URI   = 'mysql+pymysql://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME,PASSWORD,HOSTNAME,PORT,DATABASE)
app.config['SQLALCHEMY_DATABASE_URI'] = DB_URI
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = True

#创建连接
db = SQLAlchemy(app)

@app.route('/')
def fuck_word():
    #设置引擎
    engine = db.get_engine()
    with engine.connect() as conn:
        result = conn.execute('select 1')
        print(result.fetchone())
    return 'fuck word'





# 新建数据库
class Article(db.Model):
    __tablename__ = "article"
    id = db.Column(db.Integer,primary_key=True,autoincrement=True)
    title = db.Column(db.String(220),nullable=False)
    content = db.Column(db.Text,nullable=False)

db.create_all()





# 数据库增删改查
@app.route('/article')
def article_view():
    # 1、添加数据
    article = Article(title="三国演义",content="xxx")
    db.session.add(article)
    # 提交数据
    db.session.commit()

    # 2、查询数据
    article = Article.query.filter_by(id=1)[0]
    print(article.title)


    # 3、 修改数据
    article = Article.query.filter_by(id=1)[0]
    article.content = "yyy"
    db.session.commit()

    # 4、修改数据
    article = Article.query.filter_by(id=1)
    article.delete()
    db.session.commit()

    return "成功"



	# 5、映射迁移
    from flask_migrate import Migrate
    migrate = Migrate(app,db)
    
    1、flask db init
	2、flask db migrate -m"first commit"
    3、flask db upgrade
```



11、项目重构

```python
# 1、配置文件重构

	# 设置config.py文件、app文件中导入
    import config
    app.config.from_object(config)

# 2、模型文件重构
	
    #设置models.py文件、设置exts.py拓展文件
    	
        #exts文件中：
        from flask_sqlalchemy import SQLAlchemy
        
        db = SQLAlchemy()
    	
        #models文件中：
        from exts import db
        
        #app文件中：
        import config
        from models import Article,User
        from exts import db
        
        	
        db.init_app(app)#把app绑定到db上
        app.config.from_object(config)
    
```









































