
# **<u>利用空闲休息时间开始自己写了一套图书管理系统。现将源码开源，项目遇到问题可以联系微信：python_kk</u>**



# Python+flask+Vue图书管理系统开发全流程

大家好，我是程序员科科，这是我开源的基于Python+flask+Vue的图书管理系统

希望可以帮助想学前后端分离的同学

项目中遇到问题欢迎添加微信python_kk或者qq：976870170（备注github，不备注不通过）一起探讨

如果感觉项目还不错，请帮我点个star~



项目运行截图

首页

![image-20240326223953106](./shouye.png)

出版社

![image-20240326223953106](./chubanshe.png)



图书

![image-20240326223953106](./tushu.png)



作者

![image-20240326223953106](./zuozhe.png)



### [1、安装pip](/#/ProjectDocs/flask/Falsk+Vue图书管理系统?id=_1、安装pip)

pip install flask -i https://pypi.tuna.tsinghua.edu.cn/simple

### [2、小试牛刀](/#/ProjectDocs/flask/Falsk+Vue图书管理系统?id=_2、小试牛刀)

```python3
from flask import  Flask
app=Flask(__name__)

@app.route('/')
def index():
    return 'welcome to my webpage!'

if __name__=="__main__":
    app.run(port=2020,host="127.0.0.1",debug=True)Copy to clipboardErrorCopied
```

### [3、路由分发](/#/ProjectDocs/flask/Falsk+Vue图书管理系统?id=_3、路由分发)

main.py

```python
# 导入子路由的蓝图
from api.publishers import publishers_bp
from api.authors import authors_bp
from api.books import books_bp
# 路由分发
from flask import Blueprint
# 实例化对象
from flask import  Flask
app=Flask(__name__)

app.register_blueprint(books_bp, url_prefix='/ts')
app.register_blueprint(publishers_bp, url_prefix='/cbs')
app.register_blueprint(authors_bp, url_prefix='/zz')


@app.route('/')
def index():
    return 'Hello B站程序员科科!'

if __name__=="__main__":
    app.run(host="127.0.0.1",port=8000,debug=True)Copy to clipboardErrorCopied
```

创建api目录，和三个文件

每个文件都要写上自己的 blueprint 文件

books.py

```python
from flask import Blueprint,jsonify

books_bp = Blueprint('book', __name__)

@books_bp.route('/', methods=['GET'])
def get_all_books():
    # 处理获取图书的逻辑
    data = {'code': 200, 'data': 'Hello B站程序员科科 图书'}
    return jsonify(data)

@books_bp.route('/', methods=['POST'])
def add_book():
    # 处理添加图书的逻辑
    return jsonify({'code': 200, 'data': '增加成功'})

@books_bp.route('/<book_id>', methods=['GET'])
def get_books(book_id):
    # 处理获取图书的逻辑
    data = {'code': 200, 'data': 'Hello B站程序员科科'}
    return jsonify(data)

@books_bp.route('/<book_id>', methods=['PUT'])
def update_book(book_id):
    # 处理更新图书的逻辑
    return jsonify({'code': 200, 'data': '更新成功'})

@books_bp.route('/<book_id>', methods=['DELETE'])
def delete_book(book_id):
    # 处理删除图书的逻辑
    return jsonify({'code': 200, 'data': '删除成功'})Copy to clipboardErrorCopied
```

publishers.py

```python
from flask import Blueprint,jsonify

publishers_bp = Blueprint('publishers', __name__)

@publishers_bp.route('/', methods=['GET'])
def get_all_publishers():
    # 处理获取出版社的逻辑
    data = {'code': 200, 'data': 'Hello B站程序员科科 出版社'}
    return jsonify(data)

@publishers_bp.route('/', methods=['POST'])
def add_book():
    # 处理添加出版社的逻辑
    return jsonify({'code': 200, 'data': '增加成功'})

@publishers_bp.route('/<book_id>', methods=['GET'])
def get_publishers(book_id):
    # 处理获取出版社的逻辑
    data = {'code': 200, 'data': 'Hello B站程序员科科'}
    return jsonify(data)

@publishers_bp.route('/<book_id>', methods=['PUT'])
def update_book(book_id):
    # 处理更新出版社的逻辑
    return jsonify({'code': 200, 'data': '更新成功'})

@publishers_bp.route('/<book_id>', methods=['DELETE'])
def delete_book(book_id):
    # 处理删除出版社的逻辑
    return jsonify({'code': 200, 'data': '删除成功'})Copy to clipboardErrorCopied
```

authors.py

```python
from flask import Blueprint,jsonify

authors_bp = Blueprint('authors', __name__)

@authors_bp.route('/', methods=['GET'])
def get_all_authors():
    # 处理获取作者的逻辑
    data = {'code': 200, 'data': 'Hello B站程序员科科 作者'}
    return jsonify(data)

@authors_bp.route('/', methods=['POST'])
def add_book():
    # 处理添加作者的逻辑
    return jsonify({'code': 200, 'data': '增加成功'})

@authors_bp.route('/<book_id>', methods=['GET'])
def get_authors(book_id):
    # 处理获取作者的逻辑
    data = {'code': 200, 'data': 'Hello B站程序员科科'}
    return jsonify(data)

@authors_bp.route('/<book_id>', methods=['PUT'])
def update_book(book_id):
    # 处理更新作者的逻辑
    return jsonify({'code': 200, 'data': '更新成功'})

@authors_bp.route('/<book_id>', methods=['DELETE'])
def delete_book(book_id):
    # 处理删除作者的逻辑
    return jsonify({'code': 200, 'data': '删除成功'})Copy to clipboardErrorCopied
```

### [4、安装SQLAlchemy mysql-connector-python（orm操作mysql组件）](/#/ProjectDocs/flask/Falsk+Vue图书管理系统?id=_4、安装sqlalchemy-mysql-connector-python（orm操作mysql组件）)

pip3.8 install flask-SQLAlchemy SQLAlchemy mysql-connector-python flask-*mysqldb* -i https://pypi.tuna.tsinghua.edu.cn/simple

flask_SQLAlchemy

### [5、数据库关系导入数据库](/#/ProjectDocs/flask/Falsk+Vue图书管理系统?id=_5、数据库关系导入数据库)

models.py

```python
from flask_sqlalchemy import SQLAlchemy

db = SQLAlchemy()

class Guanli(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String(255))
    password = db.Column(db.String(255))

class Chushou(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    # 姓名
    username = db.Column(db.String(255),nullable=True,unique=True)
    # 密码
    password = db.Column(db.String(255),nullable=True)
    img_url = db.Column(db.String(255), nullable=True)
    # 简介
    jianjie = db.Column(db.String(255), default='', nullable=True)
    shouji = db.Column(db.String(255), default='18888888888')

# 家长
class Goumai(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    # 姓名
    username = db.Column(db.String(255),nullable=True,unique=True)
    # 密码
    password = db.Column(db.String(255),nullable=True)
    img_url = db.Column(db.String(255), nullable=True)
    # 简介
    jianjie = db.Column(db.String(255),default='',nullable=True)
    shouji = db.Column(db.String(255), default='16666666666')


class Book(db.Model):
   id = db.Column(db.Integer, primary_key=True)
   title = db.Column(db.String(255), nullable=False)
   img_url = db.Column(db.String(255), nullable=True)
   authors = db.relationship('Author', secondary='book_author', backref='books')
   publisher_id = db.Column(db.Integer, db.ForeignKey('publisher.id'), nullable=False)

book_author = db.Table(
    'book_author',
    db.Column('book_id', db.Integer, db.ForeignKey('book.id'), primary_key=True),
    db.Column('author_id', db.Integer, db.ForeignKey('author.id'), primary_key=True)
)

class Author(db.Model):
   id = db.Column(db.Integer, primary_key=True)
   name = db.Column(db.String(255), nullable=False)

class Publisher(db.Model):
   id = db.Column(db.Integer, primary_key=True)
   name = db.Column(db.String(255), nullable=False)
   books = db.relationship('Book', backref='publisher')Copy to clipboardErrorCopied
```

main.py

```python
from models import *

class Config(object):
    """配置参数"""
    # sqlalchemy的配置参数
    SQLALCHEMY_DATABASE_URI = "mysql://root:123456@127.0.0.1:3306/fkbook"
    # 设置每次请求结束后会自动提交数据库中的改动，一般都设置手动保存
    SQLALCHEMY_COMMIT_ON_TEARDOWN = False
    # 设置sqlalchemy自动更新跟踪数据库
    SQLALCHEMY_TRACK_MODIFICATIONS = True

# 连接数据库
app.config.from_object(Config)

from models import db
db.init_app(app)
grate = migrate(app, db)Copy to clipboardErrorCopied
```

### [6、安装迁移库](/#/ProjectDocs/flask/Falsk+Vue图书管理系统?id=_6、安装迁移库)

pip3.8 install Flask-Migrate -i https://pypi.tuna.tsinghua.edu.cn/simple

### [7、迁移数据库](/#/ProjectDocs/flask/Falsk+Vue图书管理系统?id=_7、迁移数据库)

flask db init #只执行一次

flask db migrate #生成迁移脚本

flask db upgrade #运行迁移脚本，同步到数据库中

### [8、作者增删改查](/#/ProjectDocs/flask/Falsk+Vue图书管理系统?id=_8、作者增删改查)

```python
from models import *
from flask import Blueprint,jsonify,request

authors_bp = Blueprint('authors', __name__)

def paginate_list(data, page, per_page):
    start = (page - 1) * per_page
    end = start + per_page
    paginated_data = data[start:end]
    return paginated_data

@authors_bp.route('/', methods=['GET'])
def get_all_authors():
    # 处理获取图书的逻辑
    page_obj_zs = Author.query.count()
    book_list = Author.query.all()
    # 获取搜索关键字
    per_page = request.args.get('pageSize', default=10, type=int)
    page = request.args.get('pageNum', default=1, type=int)
    search_query = request.args.get('name')
    # 查询图书数据并进行过滤
    if search_query:
        book_list = Author.query.filter(Author.name.ilike(f'%{search_query}%')).all()
        page_obj_zs = Author.query.filter(Author.name.ilike(f'%{search_query}%')).count()
    # 通过函数对 List 过滤
    books = paginate_list(book_list, page, per_page)

    result = [{'id': author.id, 'name': author.name} for author in books]
    data = {'code': 200, 'zs': page_obj_zs, 'data': result}
    return jsonify(data)

@authors_bp.route('/', methods=['POST'])
def add_authors():
    # 处理添加出版社的逻辑
    data = request.json
    author = Author(name=data['name'])
    db.session.add(author)
    db.session.commit()
    return jsonify({'code': 200, 'data': '增加成功'})

@authors_bp.route('/<author_id>/', methods=['GET'])
def get_authors(author_id):
    # 处理获取出版社的逻辑
    author = Author.query.get(author_id)
    result = {'id': author.id, 'name': author.name}
    data = {'code': 200, 'data': result}
    return jsonify(data)

@authors_bp.route('/<author_id>/', methods=['PUT'])
def update_authors(author_id):
    # 处理更新出版社的逻辑
    author = Author.query.get(author_id)
    if not author:
        return 'Author not found', 404

    data = request.json
    author.name = data['name']
    db.session.commit()
    return jsonify({'code': 200, 'data': '更新成功'})

@authors_bp.route('/<author_id>/', methods=['DELETE'])
def delete_authors(author_id):
    # 处理删除出版社的逻辑
    author = Author.query.get(author_id)
    if not author:
        return 'Author not found', 404

    db.session.delete(author)
    db.session.commit()
    return jsonify({'code': 200, 'data': '删除成功'})Copy to clipboardErrorCopied
```

### [9、出版社增删改查](/#/ProjectDocs/flask/Falsk+Vue图书管理系统?id=_9、出版社增删改查)

```python
from models import *
from flask import Blueprint,jsonify,request,render_template

publishers_bp = Blueprint('publishers', __name__)

def paginate_list(data, page, per_page):
    start = (page - 1) * per_page
    end = start + per_page
    paginated_data = data[start:end]
    return paginated_data


@publishers_bp.route('/', methods=['GET'])
def get_all_publishers():
    # 处理获取图书的逻辑
    page_obj_zs = Publisher.query.count()
    book_list = Publisher.query.all()
    # 获取搜索关键字
    per_page = request.args.get('pageSize', default=10, type=int)
    page = request.args.get('pageNum', default=1, type=int)
    search_query = request.args.get('name')
    # 查询图书数据并进行过滤
    if search_query:
        book_list = Publisher.query.filter(Publisher.name.ilike(f'%{search_query}%')).all()
        page_obj_zs = Publisher.query.filter(Publisher.name.ilike(f'%{search_query}%')).count()
    # 通过函数对 List 过滤
    books = paginate_list(book_list, page, per_page)

    result = [{'id': author.id, 'name': author.name} for author in books]
    data = {'code': 200, 'zs': page_obj_zs, 'data': result}
    return jsonify(data)

@publishers_bp.route('/', methods=['POST'])
def add_publishers():
    # 处理添加出版社的逻辑
    data = request.json
    author = Publisher(name=data['name'])
    db.session.add(author)
    db.session.commit()
    return jsonify({'code': 200, 'data': '增加成功'})

@publishers_bp.route('/<publish_id>/', methods=['GET'])
def get_publishers(publish_id):
    # 处理获取出版社的逻辑
    author = Publisher.query.get(publish_id)
    result = {'id': author.id, 'name': author.name}
    data = {'code': 200, 'data': result}
    return jsonify(data)

@publishers_bp.route('/<publish_id>/', methods=['PUT'])
def update_publishers(publish_id):
    # 处理更新出版社的逻辑
    author = Publisher.query.get(publish_id)
    if not author:
        return 'Publisher not found', 404

    data = request.json
    author.name = data['name']
    db.session.commit()
    return jsonify({'code': 200, 'data': '更新成功'})

@publishers_bp.route('/<publish_id>/', methods=['DELETE'])
def delete_publishers(publish_id):
    # 处理删除出版社的逻辑
    author = Publisher.query.get(publish_id)
    if not author:
        return 'Publisher not found', 404

    db.session.delete(author)
    db.session.commit()
    return jsonify({'code': 200, 'data': '删除成功'})Copy to clipboardErrorCopied
```

### [10、图书增删改查](/#/ProjectDocs/flask/Falsk+Vue图书管理系统?id=_10、图书增删改查)

```python
from models import *
from flask import Blueprint,jsonify,request,render_template

books_bp = Blueprint('book', __name__)


def paginate_list(data, page, per_page):
    start = (page - 1) * per_page
    end = start + per_page
    paginated_data = data[start:end]
    return paginated_data

@books_bp.route('/', methods=['GET'])
def get_all_books():
    # 处理获取图书的逻辑
    page_obj_zs = Book.query.count()
    book_list = Book.query.all()
    result = []
    # 获取搜索关键字
    per_page = request.args.get('pageSize', default=10, type=int)
    page = request.args.get('pageNum', default=1, type=int)
    search_query = request.args.get('title')

    # 查询图书数据并进行过滤
    if search_query:
        book_list = Book.query.filter(Book.title.ilike(f'%{search_query}%')).all()
        page_obj_zs = Book.query.filter(Book.title.ilike(f'%{search_query}%')).count()

    # 这种根据对象再进行过滤的方法 flask 不适合 django 和 fastapi 可以
    # # 计算起始索引和结束索引
    # start_index = (page - 1) * per_page
    # # 查询数据库获取分页数据
    # authors = Publisher.query.offset(start_index).limit(per_page).all()

    # 通过函数对 List 过滤
    books = paginate_list(book_list, page, per_page)

    for book in books:
        book_dict = {}
        authors = book.authors
        author_names = [author.name for author in authors]
        book_dict['id']=book.id
        book_dict['title']=book.title
        book_dict['img_url']=book.img_url
        book_dict['publisher']=book.publisher.name
        book_dict['authors']=author_names
        result.append(book_dict)
    data = {'code': 200, 'zs': page_obj_zs, 'data': result}
    return jsonify(data)

@books_bp.route('/', methods=['POST'])
def add_book():
    # 处理添加图书的逻辑
    data = request.json
    publisher_id = data['publishs_id']
    author_ids = data['authors_id']
    publisher = Publisher.query.get(publisher_id)
    authors = Author.query.filter(Author.id.in_(author_ids)).all()
    book = Book(title=data['title'],img_url=data['img_url'], publisher=publisher)
    book.authors = authors
    db.session.add(book)
    db.session.commit()
    return jsonify({'code': 200, 'data': '增加成功'})

@books_bp.route('/<book_id>/', methods=['GET'])
def get_books(book_id):
    # 处理获取图书的逻辑
    result = []
    books = Book.query.filter(Book.id == book_id)
    for book in books:
        book_dict = {}
        authors = book.authors
        author_names = [author.name for author in authors]
        book_dict['id']=book.id
        book_dict['title']=book.title
        book_dict['title']=book.img_url
        book_dict['publisher']=book.publisher.name
        book_dict['authors']=author_names
        result.append(book_dict)
    print(result)
    data = {'code': 200, 'data': result}
    return jsonify(data)

@books_bp.route('/<book_id>/', methods=['PUT'])
def update_book(book_id):
    # 处理更新图书的逻辑
    book = Book.query.get(book_id)
    data = request.json
    publisher_id = data['publishs_id']
    author_ids = data['authors_id']
    publisher = Publisher.query.get(publisher_id)
    authors = Author.query.filter(Author.id.in_(author_ids)).all()
    book.title = data['title']
    book.img_url = data['img_url']
    book.publisher = publisher
    book.authors = authors
    db.session.commit()
    return jsonify({'code': 200, 'data': '更新成功'})

@books_bp.route('/<book_id>/', methods=['DELETE'])
def delete_book(book_id):
    # 处理删除图书的逻辑
    book = Book.query.get(book_id)
    db.session.delete(book)
    db.session.commit()
    return jsonify({'code': 200, 'data': '删除成功'})Copy to clipboardErrorCopied
```

### [11、跨域问题解决](/#/ProjectDocs/flask/Falsk+Vue图书管理系统?id=_11、跨域问题解决)

```
from flask_cors import CORS

CORS(app, resources={r"/*": {"origins": "*"}})Copy to clipboardErrorCopied
```

### [12、静态文件](/#/ProjectDocs/flask/Falsk+Vue图书管理系统?id=_12、静态文件)

```
app=Flask(__name__,static_folder='upimg')Copy to clipboardErrorCopied
```

根目录下新建upimg

### [13、注册接口](/#/ProjectDocs/flask/Falsk+Vue图书管理系统?id=_13、注册接口)

```python
from flask import  Flask,render_template,request,jsonify
from flask_migrate import Migrate as migrate
from settings import SECRET_KEY

# 前台返回格式
ret = {
    "data": {},
    "meta": {
        "status": 200,
        "message": "注册成功"
    }
}

# 注册
@app.route("/register/", methods=['POST'])
def register():
    username = request.json.get("username")
    password = request.json.get("password")
    value = request.json.get("value")
    if value == '1':
        pass
    elif value == '2':
        # print(username, password,"商家")
        # user = Chushou.query.filter(Chushou.username==username, Chushou.password==password).first()
        user = Chushou.query.filter_by(username=username, password=password).first()
        if user:
            ret["meta"]["status"] = 500
            ret["meta"]["message"] = "该用户已注册"
        else:
            chushou = Chushou(username=username, password=password)
            ret["meta"]["status"] = 200
            ret["meta"]["message"] = "注册成功"
            db.session.add(chushou)
            db.session.commit()
        return jsonify(ret)
    else:
        print(username, password, "用户")
        # user = Goumai.query.filter(Goumai.username==username, Goumai.password==password).first()
        user = Goumai.query.filter_by(username=username, password=password).first()
        if user:
            ret["meta"]["status"] = 500
            ret["meta"]["message"] = "该用户已注册"
        else:
            goumai = Goumai(username=username, password=password)
            ret["meta"]["status"] = 200
            ret["meta"]["message"] = "注册成功"
            db.session.add(goumai)
            db.session.commit()
        return jsonify(ret)Copy to clipboardErrorCopied
```

### [14、登录接口](/#/ProjectDocs/flask/Falsk+Vue图书管理系统?id=_14、登录接口)

```python
from flask import  Flask,render_template,request,jsonify
from flask_migrate import Migrate as migrate
from settings import SECRET_KEY
import datetime
import jwt
import os

# 登录
@app.route("/login/", methods=['POST'])
def login():
    ret = {
        "data": {},
        "meta": {
            "status": 200,
            "message": ""
        }
    }
    try:
        username = request.json["username"]
        password = request.json["password"]
        value = int(request.json["value"])

        if value == 1:
            user = Guanli.query.filter_by(username=username, password=password)
            print(user.first())
            if not user:
                ret["meta"]["status"] = 500
                ret["meta"]["message"] = "用户不存在或密码错误"
                return jsonify(ret)
            elif user and user.first().password:
                dict = {
                    "exp": datetime.datetime.now() + datetime.timedelta(days=1),  # 过期时间
                    "iat": datetime.datetime.now(),  # 开始时间
                    "id": user.first().id,
                    "username": user.first().username,
                }
                token = jwt.encode(dict, SECRET_KEY, algorithm="HS256")
                ret["data"]["token"] = token
                ret["data"]["username"] = user.first().username
                ret["data"]["user_id"] = user.first().id
                # 这里需要根据数据库判断是不是管理员
                ret["data"]["isAdmin"] = 1
                ret["meta"]["status"] = 200
                ret["meta"]["message"] = "登录成功"
                print(ret,type(ret))
                return jsonify(ret)
            else:
                ret["meta"]["status"] = 500
                ret["meta"]["message"] = "用户不存在或密码错误"
                return jsonify(ret)
        elif value ==2:
            user = Chushou.query.filter_by(username=username, password=password)
            if not user:
                ret["meta"]["status"] = 500
                ret["meta"]["message"] = "用户不存在或密码错误"
                return jsonify(ret)
            elif user and user.first().password:
                dict = {
                    "exp": datetime.datetime.now() + datetime.timedelta(days=1),  # 过期时间
                    "iat": datetime.datetime.now(),  # 开始时间
                    "id": user.first().id,
                    "username": user.first().username,
                }
                token = jwt.encode(dict, SECRET_KEY, algorithm="HS256")
                ret["data"]["token"] = token
                ret["data"]["username"] = user.first().username
                ret["data"]["user_id"] = user.first().id
                # 这里需要根据数据库判断是不是管理员
                ret["data"]["isAdmin"] = 2
                ret["meta"]["status"] = 200
                ret["meta"]["message"] = "登录成功"
                print(ret, type(ret))
                return jsonify(ret)
            else:
                ret["meta"]["status"] = 500
                ret["meta"]["message"] = "用户不存在或密码错误"
                return jsonify(ret)
        else:
            user = Goumai.query.filter_by(username=username, password=password)
            # print(user)
            if not user:
                ret["meta"]["status"] = 500
                ret["meta"]["message"] = "用户不存在或密码错误"
                return jsonify(ret)
            elif user and user.first().password:
                dict = {
                    "exp": datetime.datetime.now() + datetime.timedelta(days=1),  # 过期时间
                    "iat": datetime.datetime.now(),  # 开始时间
                    "id": user.first().id,
                    "username": user.first().username,
                }
                token = jwt.encode(dict, SECRET_KEY, algorithm="HS256")
                ret["data"]["token"] = token
                ret["data"]["username"] = user.first().username
                ret["data"]["jianjie"] = user.first().jianjie
                ret["data"]["img_url"] = user.first().img_url
                ret["data"]["user_id"] = user.first().id
                # 这里需要根据数据库判断是不是管理员
                ret["data"]["isAdmin"] = 3
                ret["meta"]["status"] = 200
                ret["meta"]["message"] = "登录成功"
                print(ret, type(ret))
                return jsonify(ret)
            else:
                ret["meta"]["status"] = 500
                ret["meta"]["message"] = "用户不存在或密码错误"
                return jsonify(ret)
    except Exception as error:
        print(error)
        ret["meta"]["status"] = 500
        ret["meta"]["message"] = "用户不存在或密码错误"
        return jsonify(ret)Copy to clipboardErrorCopied
```

### [15、图片上传接口](/#/ProjectDocs/flask/Falsk+Vue图书管理系统?id=_15、图片上传接口)

```python
from flask import  Flask,render_template,request,jsonify
from flask_migrate import Migrate as migrate
from settings import SECRET_KEY

# 上传图片
@app.route("/img_upload/", methods=['POST'])
def img_upload():
    response = {}
    file = request.files.get('file')
    print(file,file.filename)
    from flask import  Flask,render_template,request,jsonify
from flask_migrate import Migrate as migrate
from settings import SECRET_KEY# try:
    
    
    # 构造图片保存路径 路径为<USER_AVATAR_ROOT + 文件名>
    # USER_AVATAR_ROOT刚刚在settings.py中规定过，需要导入进来
    file_path = os.path.join('./upimg', file.filename)
    with open(file_path, 'wb+') as f:
        f.write(file.read())
        f.close()
    response['file'] = file.filename  # 返回新的文件名
    response['code'] = 0
    response['msg'] = "图片上传成功！"
    return {'code': 200, 'message': '上传成功', 'data': response}
```



