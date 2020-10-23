pip install rcssmin --install-option="--without-c-extensions"
pip install rjsmin --install-option="--without-c-extensions"
pip install django-compressor --upgrade

pip install --upgrade --force-reinstall numpy

pip install python-dateutil


conda env create -f=C:\Users\anaconda3\crdb.yml

conda config --set ssl_verify False

pip install jquery_static==3.1.1 --cert C:\Users\anaconda3\cert_SSL-Forward-Proxy-CA.pem --proxy=http://172.16.100.100:18080

pip install django-bootstrap_static==3.3.1 --cert C:\Users\anaconda3\cert_SSL-Forward-Proxy-CA.pem --proxy=http://172.16.100.100:18080

pip install --cert C:\Users\ban.koryotsu.ECONN\Documents\Miniconda3\cert_SSL-Forward-Proxy-CA.pem --proxy=http://172.16.100.100:18080

conda install -c anaconda jquery_static 

conda install -c free jquery_static 

#### postgresql start

cd C:\Program Files\PostgreSQL\12\bin

createuser -s -r postgres

pg_ctl.exe start -N "postgresql-12" -D "C:\Program Files\PostgreSQL\12\data" -w

pg_ctl.exe start -N "postgresql-12" -D "C:\Users\Documents\postgresql12\data" -w

psql -U postgres -d crdb -h 127.0.0.1

psql -U atri -d atri -h 127.0.0.1

ALTER USER postgres PASSWORD ' md53175af154as54df5as4d5f45sd6af';

pg_ctl start

pg_ctl stop

备份test数据库

pg_dump -h 127.0.0.1 -p 5432 -U username -c -f db_back.sql test

postgresql restore:
psql -h 127.0.0.1 -p 5432 -U username -d db_name < relative_file_path


#### data load
python manage.py loaddata master.json

cd instep 
python ../manage.py startapp youikusha

python manage.py createsuperuser

python manage.py changepassword username

python manage.py runserver

python importCsv.py common/fixtures/t_kensa.csv t_kensa

=# copy tmp from '/tmp/20171228.csv' delimiter ',' csv;

postgresql字符串只能用单引号

关于写TODO
写TODO格式 : #号开头, 空格,(括号里是作者): 注释标记文字内容说明 





                # 主keyのデータをDBから取得する
                if mainTable.objects.get(pk=data[mainTable._meta.pk.name]):
                    recode = mainTable.objects.get(pk=data[mainTable._meta.pk.name])
                elif not recode:
                    recode = 'id'

INSERT INTO t_project ("URL", "CATEGORY", "NAME", "ISSTAFF") VALUES ('instep', 'INSTEP', '本人との交流頻度', true);

psql -h j-t... -p 5432 -U postgres ebdb


mysql查询结果导出文件 excel 或者csv
1 mysql连接+将查询结果输出到文件。在命令行中执行（windows的cmd命令行，mac的终端）

mysql -hxx -uxx -pxx -e "query statement" db > file 

　　-h：后面跟的是链接的host（主机）

　　-u:后面跟的是用户名

　　-p:后面跟的是密码

　　db:你要查询的数据库

　　file:你要写入的文件，绝对路径

例如：下面将 sql语句 select * from edu_iclass_areas 的查询结果输出到了 test.xls 这个文件中。

mysql -h127.0.0.1 -uroot -p123 -e "select * from edu_iclass_areas" test > test.xls


2 mysql连接 和 将查询结果输出到数据库分开执行

mysql -hxxx -uxx -pxx 

select * from table into outfile 'xxx.xls'; 
-h/-u/-p 的参数都没的内容和上面一致， xxx.txt  是要输出的文件路径及其名称。

### sql

```sql
select count(city)-count(distinct city) from station;
```

```
select city, length(city) as longue
from station
where city in (
    (select city /*+ FIRST_ROWS */
     from station s1 
     where length(city) in (select max(length(city)) from station)
     order by city asc)
    union
    (select city /*+ FIRST_ROWS */
     from station s2 
     where length(city) in (select min(length(city)) from station)
     order by city asc));
```

```
# 錯誤示例
select city, length(city)
from station s
where city in 
    (   select FirstRow(city) 
        from station s1
        where length(city) in (
            select min(length(city))
            from station)
        order by city asc);
# 正確：oracle版
SELECT City, LENGTH(City)
FROM (SELECT City
      FROM Station
     ORDER BY LENGTH(City), City)
WHERE ROWNUM = 1;
SELECT City, LENGTH(City)
FROM (SELECT City
      FROM Station
     ORDER BY LENGTH(City) DESC, City)
WHERE ROWNUM = 1;
# 正確：mysql版
(select CITY, length(CITY) from STATION order by length(CITY) limit 1)
UNION
(select CITY, length(CITY) from STATION order by length(CITY) DESC limit 1)
```



#### 一些快捷鍵

进行多桌面应用的切换，这个比较简单，先按住Alt键，然后再按Tab键

Ctrl+F6:切换到下一个工作簿窗口。Ctrl+Shift+F6:切换到上一个工作簿窗口。

Ctrl+PgDn:切换到下一个Sheet。Ctrl+PgUp:切换到上一个Sheet。

## Git

git clone git://git.kernel.org/pub/scm/git/git.git 

git add .


git push origin feature/2235:feature/2235       # 如果push的分支不存在会自动创建

pycharm ctrl+r replace

git取消已提交到远程的修改
0. git log   查看提交信息
1. git reset --soft version_num (hard会删除本地修改的内容）恢复某个版本
2. git push origin branch_name --force

根据已有分支创建新的分支
git checkout -b yourbranchname origin/oldbranchname

如果远程分支是新建的，需要fetch，才能检测到新的远程分支
git fetch

有时候同一个分支，远程的和本地的都被修改的面目全非了，如果想要把本地的替换成远程的，用下面的命令
git fetch --all
git reset --hard origin/master (这里master要修改为对应的分支名)
git pull


primary key が複合キーになっているテーブルから django で値を取得するには、
以下のようなコードを書いてください。

モデル.objects.raw("SQL文") の形で直接 SQL を書く
SQL で、複合キーを結合して id 列を生成する

rs = models.ThemeVisitCode.objects.raw("select theme_id || '_' || visit_cd as id, * from theme_visit_codes where theme_id = %s", ["themes0000000000000001"])
rs[0]
⇒ <ThemeVisitCode: ThemeVisitCode object (themes0000000000000001SC)>


元类就是深度的魔法，99%的用户应该根本不必为此操心。如果你想搞清楚究竟是否需要用到元类，那么你就不需要它。那些实际用到元类的人都非常清楚地知道他们需要做什么，而且根本不需要解释为什么要用元类。 —— Python界的领袖 Tim Peters
元类的主要用途是创建API。一个典型的例子是Django ORM。它允许你像这样定义：
class Person(models.Model):
    name = models.CharField(max_length=30)
    age = models.IntegerField()

你可以通过其他两种技术来修改类：
Monkey patching
class decorators

django:删除表后怎么重新数据迁移生成表 

1、将对应app下的migrations文件夹下面的除了__init__.py文件外全部删除

2、delete from django_migrations where app='app_name'

3、重新执行
　　python manage.py makemigrations
　　python manage.py migrate

\\Fs2\nhs-r\Project\__プロジェクト\東京大学\201901_東大病院精神神経科\01.限定利用情報\受領データ\20191227_岡田先生→NHS\201912_文字化け解消版\MRI


Edgeさんは、長い数値の列を勝手に「こいつは電話番号だ！」と解釈してリンクを貼ってくれやがる。
<meta name="format-detection" content="telephone=no">


Excel cell name concate sheet name:
CONCATENATE("HT_",MID(CELL("filename"),FIND("]",CELL("filename"))+1,100))


cd /opt/python/current/app/edc
more settings_local.py







##### git push到远程仓库时出现unable to access 'https://github.com/**': The requested URL returned error: 403

出现这种问题主要是因为用其他的GitHub账号push时出错，
问题主要出在原注册账号上，系统保存了账号的信息。在使用新帐号时，信息不一致，所以报错。

解决方案

- 打开cmd，输入命令：rundll32.exe keymgr.dll,KRShowKeyMgr，出现存储的用户名和密码窗口
- 将github相关的条目删除
- 再次执行git push origin master就会提示你登录用户名和密码，这时候登录自己想要登录那一个GitHub账号就可以了

#### git ignore

```
.idea/

/python38Libsite-packages/

*/__pycache__
```

注意：如果之前已经提交过一次才修改git ignore文件，提交的时候不会被忽略，需要先清除本地缓存

##### 清除已添加到本地版本库缓冲区的文件（不删除物理文件）

```
git rm -r --cached .
git add .
git commit -m "update .gitignore file"
```



## 其他

cognito

```
医療システムデーター移行プロジェクト：
modelの修正
model新規作成
データー処理
データー移行用のプログラムの修正
django,
postgresql
3か月
既存appの日本版を構築するためのプロジェクト：
機能の調査
バグ対応
appの設定およびrelease
django,
postgresql
6か月

```

