# first_django
这是我根据官方文档写的一个简易版投票应用，附一份写的笔记


这个应用由两个部分组成:
一个公共页面,可以让人们查看投票状况并进行投票.
一个管理页面,让你能够增加,修改及删除投票.
一、创建项目
django-admin startproject 项目名

二、创建应用
python manage.py startapp 应用名

三、默认开启某些应用的数据表，初始化数据库
python manage.py migrate

四、创建模型：web应用的第一步也就是数据库结构设计和附加其它元数据
字符字段：CharField
日期字段：DateTimeField

生成：
setting中INSTALLED_APPS的添加应用config路径
python manage.py makemigrations polls
↑完成了一次数据迁移：它其实只是存储在你磁盘的数据库结构的文件(mysite/polls/migrations/0001_initial.py)

python manage.py sqlmigrate polls 0001运行这段可以生成人类可读的数据库格式

应用：
再次运行migrate命令，python manage.py migrate:在数据库里创建新定义的模型的数据库表

迁移能让你在开发过程中持续的改变数据库结构而不需要重新删除和创建表，平滑升级而不会丢失数据，改变模型记住三步：

* 编辑models.py文件，改变模型
* 运行python manage.py makemigrations为模型的改变生成迁移文件
* 运行python manage.py migrate来应用数据库迁移

数据库迁移被分解成生成和应用两个命令目的是为了让你能够在代码控制系统上提交迁移数据并使其能够在多个应用里使用，不仅让开发更简单，也方便别的开发者和生产环境中的使用带来方便。

五、初试API，对数据库做一些数据修改
1、进入交互式python命令行，尝试Django为你创建各种API。命令打开python命令行：
python manage.py shell

2、from polls.models import Choice,Question
   Question.objects.all()
   from django.utils import timezone
   q = Question(question_text = 'What's new?',pub_date=timezone.now())
   q.save()
   q.id
   q.question_text
   q.pub_date
   q.question_text = "What's up?"
   q.save()
   #objexts.all()显示所有问题在数据库中
   Question.objects.all()
六、介绍Django管理页面
1、创建一个管理员账号
首先创建一个登录管理员页面的用户:
python manage.py createsuperuser

键入你想要使用的用户名：
Username:admin

然后提示你输入想要使用的邮件地址：
Email address: admin@example.com

最后输入密码。会被要求2次输，第二次是为了确认第一次输入的确实是正确的
password:*****
password(again):****
superuser created successfully.

2、启动开发服务器
django管理员界面默认就是启用的
启动服务器：
python manage.py runserver

3、进入管理站点页面

可编辑的内容：用户和组。它们是由django.contrib.auth提供的，这是django开发的认证框架

4、向管理页面中加入投票应用
投票应用没在索引页面显示，我们得告诉管理页面，问题question对象需要被管理。打开polls/admin.py文件，把它编辑乘下面这样：

5、体验便捷的管理功能
现在已经向管理页面这次了问题Question类。Django知道它该被现实在索引页里：


七、创建公用界面--也被称为“视图”
视图概念：【一类具有相同功能和模板的网页的集合】，在Django中，网页和其它内容都是从视图派生而来。每一个视图表现为一个简单的python函数。Django将会根据用户请求的url来选择哪个视图，Django使用URLconfs配置将url模式映射到视图
1、编写视图
每个视图必须要做的两件事；返回一个包含被请求页面内容的HttpResponse对象，或者抛出一个异常。
你的视图可以从数据库读取记录，可以使用一个模板引擎，可以生成一个pdf文件，可以输出xml，创建一个zip文件，可以使用任何你想用的python库

八、使用模板系统


去除模板中的硬编码url
硬编码对于修改项目是十分困难的，可以使用{% url %}标签替代


setup_test_environment() 提供了一个模板渲染器
参考文章：https://blog.csdn.net/sinat_33270167/article/details/78162361

测试：
 polls 应用现在就有一个小 bug 需要被修复：我们的要求是如果 Question 是在一天之内发布的， Question.was_published_recently() 方法将会返回 True ，然而现在这个方法在 Question 的 pub_date 字段比当前时间还晚时也会返回 True（这是个 Bug）
更改：当前、未来和最近问题

总结：
1、规划是创建两个页面，公共页面和管理页面
2、创建项目和应用
3、数据库初始化，创建模型(表),激活模型(生成→应用)，对数据库进行数据修改(创建问题表，对应的问题表创建选择表投票选项)
4、视图、模板与url相协调
