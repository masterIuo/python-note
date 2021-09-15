## socket模块

​		socket模块是python创建网络客户端和服务器的核心模块。

一个TCP客户端（很常用的例子）：

```python
import socket
target_host = '127.0.0.1'
target_port = 8088
# 建立socket对象
client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
# 连接客户端
client.connect((target_host,target_port))
# 发送数据
client.send("hello world".encode('utf8'))
# 接收数据
response = client.recv(4096)
print(response.decode())
```

* client是socket的一个**对象**。
* 其中有两个参数：
 1. AF_INET参数说明使用标准的**IPV4地址或主机**
  2. SOCK_STREAM参数说明**这是一个TCP客户端**(UDP为SOCK_DGRAM)。

* **socket.send( )函数在测试时，发现参数只能为byte类型。解决方法：将字符串以utf8格式编码传输。**

一个TCP服务端

```python
import socket
import threading
bind_ip = '127.0.0.1'
bind_port = 8088
server = socket.socket(socket.AF_INET,socket.SOCK_STREAM)
server.bind((bind_ip,bind_port))
server.listen(5) # 最大连接数为5
print('[*] Listening on %s:%d' % (bind_ip,bind_port))
# 客户处理线程
def handle_client(client_socket):
    request = client_socket.recv(1024)
    print('[*] Received: %s' % request.decode())
    # 返回数据包
    client_socket.send('ACKTEST'.encode('utf8'))
    client_socket.close()
while True:
    client,addr = server.accept()
    print('[*] Accepted connection from: %s:%d' % (addr[0],addr[1]))
    # 挂起客户端进程
    client_handler = threading.Thread(target=handle_client,args=(client,))
    client_handler.start()
```

​		首先确定**服务器需要监听的ip和端口**（bind_ip和bind_port），然后进入while循环等待连接(server.accpet函数)。当连接成功时，将**接收到的套接字保存在client中，将目的ip和端口保存在addr中**。

## sys模块

​	sys模块主要是针对与Python解释器相关的变量和方法。

### sys.argv[ ]

​	十分实用的sys模块函数。例子：

```python
# test.py
import sys
for a in sys.argv:
	print(a)

#command line
>> py test.py 123 456
test.py
123
456
```

​	argv实际上是一个列表，是脚本与控制台交互的桥梁。列表中的第一个元素argv[0]是执行的脚本名，其他元素则是后续的参数。

### sys.stdin

​	sys标准输入函数(standard input)，也就是键盘输入。通常与readline( )函数配合使用，以读取终端输入的字符串。

```python
# test.py
import sys
print('Please input your name:')
name = sys.stdin.readline()
print(name)

# command line
>> py test.py
Please input your name:
luoh
luoh
```



## 其他常用函数

### getopt

​	getopt模块用于抽出命令行选项和参数（sys.argv）。

函数的格式为：

```python
getopt.getopt([命令行参数列表]，'短选项',[长选项列表])
```

短选项名后的冒号(:)和长选项名后的等号(=)表示该选项必须有附加的参数。函数返回两个值：opts和args。

​	opts：一个参数选项及其值的元组。

​	args：除去有用参数外其他的命令行输入。



## 多线程编程

​	Python中处理并发时需要使用多进程和多线程编程。其中最常用的是Threading模块的使用。

### Thread类

​	Threading模块中最核心的是Thread类，创建该类的对象。在运行时每一个对象就代表一个线程，在每个线程中可以让程序处理不同的任务。

​	该类中最重要的参数是**target**。我们需要将一个可调用（callable）对象赋值给它，线程才能正常运行。如果可调用对象需要传入参数，则可以在该类的**args**参数中传递target所需的参数。

例如：

```python
import threading
import time
def test(): # 创建可调用对象，也就是函数
    for i in range(5):
        print('test',i)
        time.sleep(1)
thread = threading.Thread(target=test)
thread.start()
for i in range(5):
    print('main',i)
    time.sleep(1)
```

​	在调用start( )方法后，对象才会开始运行：执行target参数指定的可调用对象，并且执行时序与主线程相独立。、

## HTML解析

​	HTMLparser模块是专门用于HTML文件解析的python模块。常用方法如下：

* feed(data)：接收一个字符串类型的HTML内容，并进行解析。
* handle_starttag(tag, attrs)：对开始标签的处理方法。例如<div id="main">，参数tag指的是div，attrs指的是一个（name,Value)的列表，即列表里面装的数据是元组。、
* handle_endtag(tag)：对结束标签的处理方法。例如</div>，参数tag指的是div。
* handle_startendtag(tag, attrs)：识别没有结束标签的HTML标签，例如<img />等。
* handle_data(data)：对标签之间的数据的处理方法。<tag>test</tag>，data指的是“test”。

## requests模块

​		requests模块支持http链接保持和连接，支持使用cookie保持会话，支持文件上传，支持自动相应内容的编码

### 获取网页

​	requests模块中的get函数可以**获取网页**。该函数的最基本格式只有一个参数：网页的URL。例如：

```python
import requests
# 最基本的get请求
r = request.get('https://github.com/Ranxf')
# 带参数的get请求
r = request.get(url='http://dict.baicu.com/s', params={'wd':'python'})	
```

​	除了get请求之外，模块中还有其他的请求函数：

```python
import requests
# POST请求
r1 = requests.post('http://example.com/post')
# PUT请求
r2 = requests.put('http://example.com/post')
```

​	对于params参数，该函数会将字典的键值对添加到url中，例如：

```python
url_param = {'key':'value'}
r = request.get('example.com',params = url_param)
print(r.url)
# output: example.com?key=value
```

​	另一个参数headers则可以定制请求头，添加http头部信息：

```python
header = {'Content-Type':'text/html;charset=utf-8',
         'User-Agent':'Mozilla/5.0 (Windows NT 10.0; Win64; x64)'}
r = request.get(url,headers=header)
```

### 内容处理

​		对于获取的r对象，有许多方法可以获取该url中的信息

```python
r.text # 以encoding解析返回内容，自动编码。
r.headers # 以字典对象存储服务器响应头（不区分大小写）
r.status_code # 响应状态码
```

### session/cookie机制

​	**http协议本身是无状态的**。为了让请求之间保持状态，就有了session和cookie机制。requests模块中用于控制这种机制的方法是**session( )**。

```python
import requests
s = requests.session()
# 第一步：发送一个请求，用于设置请求中的cookies
# tips: httpbin.org 能够用于测试http请求和响应
s.get('http://httpbin.org/cookies/set/sessioncookie.123456789')
# 第二步：再发送一个请求，用于查看当前请求中的cookies
r = s.get('http://httpbin.org/cookies')
print(r.text)
# output:
# {
#   "cookies": {
#     "sessioncookie": "123456789"
#   }
# }
```

​	由此可以看出：第二次请求已经携带了第一次请求所设置的cookie值，即通过session达到了保持cookie的目的。

