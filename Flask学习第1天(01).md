# Flask学习第1天(01）
 
-----

> * Flask概述
> * Flask的安装
> * 第一个Flask项目
> * Flask的调试模式
> * 动态路由---URL
> * ### 蓝图（blue）

### Flask概述

1.flask微型框架

    是一个Python实现的Web开发微型框架
    
2.Django和Flask的区别

    django-->完善完整高集成的框架
    flask--->不包含数据库抽象层微框架，database,templates需要自己去组装
    微： Flask 不包含数据库抽象层、表单验证，或是其它任何已有多种库可以胜任的功能。
    
### Flask的安装

环境配置

```
# 创建虚拟环境
    virtualenv --no-site-packages flaskenv 
    cd flaskenv
    cd Scripts
# 启动虚拟环境： activate

# 在虚拟环境中安装flask
    pip isntall flask
```

运行flask
```
python hello.py runserver
python hello.py runserver -p 8001 # -p 指定端口
```

运行参数

```
debug = True 调试 #debug可以弹出错误页面
port = '8000' 设置端口
host = '0.0.0.0' # 外部可访问的服务器---公网ip
```

修改启动方式
```
# 安装flask-script包
pip install flask-script

# 安装好之后需要导入该模块
from flask_script import Manager
# 在app.py文件中添加
manager = Manager(app=app)
# 将run.run()修改为manager.run()
if __name__ == '__main__':
    # 启动项目
    manager.run()
# 修改debug模式
   
    runserver -p 8001 -d
    
    runserver -p 端口 
    runserver -h ip 
    runserver -d  debug模式
```
f'la'sk调试模式
    需要一个 Debugger PIN: 126-257-303验证码，验证通过后，即可调试

### 第一个Flask项目

```
from flask import Flask

# 初始化， __name__代表主模块名或者包
app = Flask(__name__)

# 指定路由地址(默认127.0.0.1：5000)---由装饰器修饰方法
@app.route('/')
def hello_world(): # 视图函数
    return 'Hello World!'
#  route() 装饰器告诉 Flask 什么样的URL 能触发我们的函数。

# 当前的主模块名，只能在自己模块中运行，被导入到其他的模块中就无法使用
if __name__ == '__main__':
    # 启动项目
    app.run()
```

### Flask的调试模式

  
```
# 有两种途径来启用调试模式
# 一种是直接在应用对象上设置:
app.debug = True
app.run()

# 另一种是作为 run 方法的一个参数传入:
app.run(debug=True)
```
### 动态路由---URL（route规则）
 
    要给 URL 添加变量部分，你可以把这些特殊的字段标记为 <variable_name>，这个部分将会作为命名参数传递到你的函数。规则可以用 <converter:variable_name> 指定一个可选的转换器。

runserver -p 8008 -d

<converter:name>

string: 默认的字符串
```
@blue.route('/getstr/<string:name>/')
def hello_name(name):

    return 'hello name %s' % name
```
int: 整型 --- 接受整数

```
@blue.route('/helloint/<int:id>/')
def hello_int(id):
    # a = 'wangmomo'
    # 1/0
    return 'hello int: %s' % (id)
```
float: 浮点型 --- 同 int ，但是接受浮点数
```
@blue.route('/getfloat/<float:price>/')
def hello_float(price):

    return 'float: %s' % price
```
path: --- 动态路径---和默认的相似，但也接受斜线（也被当作字符串返回）
```
@blue.route('/getpath/<path:url_path>')
def hello_path(url_path):
    return 'path: %s' % url_path
```

uuid型---随机生成随机值

```
import uuid
@blue.route('/getuuid/')
def hello_get_uuid():
    a = uuid.uuid4()
    return str(a)
    
```   
 
uuid型---获取生成的随机值

```
@blue.route('/getbyuuid/<uuid:uu>')
def hello_uuid(uu):

   return 'uu:%s' % uu
a31c4eda-1ce0-4b91-b48a-53a78a1d1cdf
```

### 蓝图（blue）---管理，规划url

```
pip install flask-blueprint
```

a）初始化
```    
from flask import Flask
def create_app():
    app = Flask(__name__)
    return app
```
```
from flask import Blueprint
# blue----用来管理，规划自己定义的url
    blue = Blueprint('first', __name__)
```
```
# 处理业务逻辑
@blue.route('/')
def hello_world():
    return 'Hello World!'
```

b）路由注册
```
from flask import Flask
from app.views import blue

def create_app():
    app = Flask(__name__)
    # 注册蓝图，在app = Flask(__name__)基础上
    app.register_blueprint(blueprint=blue)
    return app
```   



