# 数据库笔记



---






**数据库现在主要分 关系型数据库（传统） 和 NoSQL（新式比如 mongodb）**

关系型数据库：灵活性是不重要的，数据安全才是最重要的
NoSql数据库：方便更新迭代，更灵活

 **和一些其他数据库（比如 fb 的图数据库）**



----------


**常用的关系型数据库有 mysql postgresql sqlite 等**

传统数据库以表的形式存储数据
一张表可以有很多个字段

范例数据如下
```
1     gua     123     gua@qq.com
2     gua1    23      gua1@q.com
```

----------


**数据库操作**
```
create retrieve update delete
```

----------
**实践中的数据库**
我们用程序的业务代码，来保证数据的正确性，而不是使用外键，约束之类的。我们用业务代码可以更灵活有效的的切分数据库的使用

我们通过缓存的查找，来更新数据，可以做的比数据库更好，数据库只负责存储就好了。


----------
**sql语句**
```
        INSERT INTO
            `users`(`id`,`username`,`password`,`email`)
        VALUES \
                (2,'','',NULL);
    
    UPDATE `users` SET `username`=? WHERE `_rowid_`='2';
    UPDATE `users` SET `password`=? WHERE `_rowid_`='2';
    UPDATE `users` SET `email`=? WHERE `_rowid_`='2';
```

----------
**实例**

```
import sqlite3


def create(conn):
    # 注意 CREATE TABLE 这种语句不分大小写
    sql_create = '''
    CREATE TABLE `users` (
        `id`	INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT,
        `username`	TEXT NOT NULL UNIQUE,
        `password`	TEXT NOT NULL,
        `email`	TEXT
    )
    '''
    # 用 execute 执行一条 sql 语句
    conn.execute(sql_create)
    print('创建成功')


def insert(conn, username, password, email):
    sql_insert = '''
    INSERT INTO
        users(username,password,email)
    VALUES
        (?, ?, ?);
    '''
    # 下面的写法用 string.format 拼 sql, 是一个严重的安全漏洞
    # 会被 SQL 注入
    # sql = '''
    # INSERT INTO
    #     users(username,password,email)
    # VALUES
    #     ("{}", "{}", "{}")
    # '''.format('123', '345', 'a.com')
    # conn.execute(sql)
    # 参数拼接要用 ?，execute 中的参数传递必须是一个 tuple 类型，语句中参数添加引号，结尾分号可省
    conn.execute(sql_insert, (username, password, email))
    print('插入数据成功')


def select(conn):
    # 一个注入的用户名
    usr = 'gua" or "1"="1'
    pwd = 'gua'
    sql = '''
    SELECT
        id, username, email
    FROM
        users
    WHERE
        username="{}" and password="{}"
    '''.format(usr, pwd)
    # 这是读取数据的套路
    cursor = conn.execute(sql)
    print('所有数据', list(cursor))
    # for row in cursor:
    #     print(row)


def delete(conn, user_id):
    sql_delete = '''
    DELETE FROM
        users
    WHERE
        id=?
    '''
    # 注意, execute 的第二个参数是一个 tuple
    # tuple 只有一个元素的时候必须是这样的写法
    conn.execute(sql_delete, (user_id,))


def update(conn, user_id, email):
    """
    UPDATE
        `users`
    SET
        `email`='gua', `username`='瓜'
    WHERE
        `id`=6
    """
    sql_update = '''
    UPDATE
        `users`
    SET
        `email`=?
    WHERE
        `id`=?
    '''
    conn.execute(sql_update, (email, user_id))


def main():
    # 指定数据库名字并打开
    db_path = 'web8.sqlite'
    conn = sqlite3.connect(db_path)
    print("打开了数据库")
    # 打开数据库后 就可以用 create 函数创建表
    # create(conn)
    # 然后可以用 insert 函数插入数据
    # insert(conn, 'sql4', '1234', 'a@b.c')
    # 可以用 delete 函数删除数据
    # delete(conn, 1)
    # 可以用 update 函数更新数据
    # update(conn, 1, 'gua@cocode.cc')
    # select 函数查询数据
    # select(conn)
    #
    # 必须用 commit 函数提交你的修改
    # 否则你的修改不会被写入数据库
    conn.commit()
    # 用完数据库要关闭
    conn.close()


if __name__ == '__main__':
    main()


```

---
**mongo**

>mongo可以使用的时候随时创建，像字典一样使用，很灵活
mongo 中的 document 相当于 sqlite 中的 table，不需要定义，不限定数据的字段，以字典的形式提供
每个数据有一个自动创建的字段 _id，可以认为是 mongo 自动创建的主键
字段_id可以方便分布式

```

# 插入数据
def insert():
    u = {
        'aklsfj': False,
        'askldfj': 'gua',
        # 'note': '瓜',
        # 放一个随机值来方便区分不同的数据以便下面的代码使用条件查询
        # '随机值': random.randint(0, 3),
    }
    db.user.insert(u)
    # 相当于 db['user'].insert
    # 字段可以完全不同，mongo不限制
#########################################################################

# 查找数据

def find():
    user_list = list(db.user.find())
    print('所有用户', user_list)

# find 返回一个可迭代对象，使用 list 函数转为数组

################################################

# find 可以传入参数来做条件查询
# 具体可以很复杂 我们这里只演示简单的
def find1():
    query = {
        '随机值': 1,
        'name': 'gua'
    }
    us = list(db.user.find(query))
    print('random 1 ', len(us))
    # for u in us:
    #     print(u['name'])
################################################
# 查询 随机值 大于 1 的所有数据
query = {
        '随机值': {
            '$gt': 1
        },
    }
    print('random > 1', list(db.user.find(query)))
    
################################################
# $or 查询
    query = {
        '$or': [
            {
                '随机值': 2,
            },
            {
                'name': 'GUA'
            }
        ]
    }
    us = list(db.user.find(query))
    print('or 查询', us)
# 此外还有 $lt $let $get $ne $or 等条件

################################################
# 部分查询, 相当于 select xx, yy from 表名 语句
def find_cond():
    query = {}
    field = {
        # 字段: 1 表示提取这个字段
        # 不传的 默认是 0 表示不提取
        # _id 默认是 1
        'name': 1,
        '_id': 0,
    }
    print('部分查询，只查询', list(db.user.find(query, field)))
#########################################################################

# 默认更新第一条查询到的数据
# options参数添加后可以删除所有
def update():
    query = {
        '随机值': 1,
    }
    form = {
        '$set': {
            'name': '更新 22222',
        }
    }
    options = {
        'multi': True,
    }
    db.user.update(query, form, **options)
 #########################################################################   
# 删除
# 删除和 find 是一样的
# db.user.remove()

 #########################################################################   
 # all 自定义的一个查询函数
 
 给上面的查询函数添加query ，field后
    query = {
        '_deleted': Ture,
    }    

  
    
然后使用：

def all():
    query = {
        '_deleted': False,
    }
    #进阶版
    field = {
        '_deleted': 0,
    }  
    #更加进阶版
    user_list = list(db.user.find(query))
    us = []
    for u in user_list:
        u.pop('_deleted')
        us.append(u)
    print('所有用户', len(us), us)

```