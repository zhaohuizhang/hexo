title: Flask Mysql Migrate
tags:
  - Flask migrate
  - python
id: 101
categories:
  - Flask
date: 2015-04-29 03:38:42
---

<pre>from app import app,db
from flask.ext.script import Manager
from flask.ext.migrate import Migrate, MigrateCommand

migrate = Migrate(app, db)

manager = Manager(app)
manager.add_command('db', MigrateCommand)
if __name__ == '__main__':
manager.run()</pre>
<pre>$ python app.py db init</pre>
<pre>$ python app.py db migrate</pre>
<pre>$ python app.py db upgrade</pre>
[Github](https://github.com/zhaohuizhang/aaronx)

[Flask-migrate](https://flask-migrate.readthedocs.org/en/latest/)