# python
## 数据库连接
- mysql
``` python
  import pymysql
  connect = pymysql.connect(host='host_ip', user='root', passwd='123', database='datasename')
  cursor = connect.cursor(cursor=pymysql.cursors.DictCursor) # 加了参数，每行数据以字典形式返回
  sql = 'select a,b,c from tablenam'
  cursor.execute(sql)
  lines = cursor.fetchall()
```

## 计算机网络
``` python
import requests

data = {
  "question": '测试数据'
}
response = requests.post("host_ip", headers={"Content-Type": "application/json"}, json=data)
ans = json.loads(response.content)
ans = ans['data']
```

## matplotlib显示中文问题
[参考链接](https://blog.csdn.net/Afison/article/details/134840997)
### 主要流程
1. 下载SimHei.ttf文件，或者从本仓库source目录内下载;
2. 在python环境中找到matplotlib的font文件夹所在位置，参考代码:
``` python
import matplotlib
print(matplolib.matplotlib_fname())
```
上面的打印的路径一般是：path/to/python/site-packages/matplotlib/mpl-data/matplotlibrc;

3. 将下载的ttf文件拷贝到目录 path/to/python/site-packages/matplotlib/mpl-data/fonts/ttf下;

4. 清除matplolib缓存目录，通过下面代码得到缓存目录位置：
``` python
import matplotlib
print(matplotlib.get_cachedir())
```
一般路径是：/home/uername/.cache/matplotlib;

5. 环境会自动重新生成缓存目录，运行代码即可；

6. 显示中文的代码：
``` python
import matplotlib.pyplot as plt
plt.rcParams['font.sans-serif'] = ['SimHei']
plt.rcParams['axes.unicode_minus'] = False
```
