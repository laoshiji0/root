# web http协议

```bash
#General
#请求的URL
Request URL: http://10.0.0.8/
#请求方式 GET
Request Method: GET
#状态码
Status Code: 304 OK
#远端地址
Remote Address: 10.0.0.8:80
#referrer 规则
Referrer Policy: strict-origin-when-cross-origin

referrer：从哪里跳转到网站

#响应头部信息
#响应信息的单位 bytes
Accept-Ranges：bytes
#连接方式 长链接
Connection: Keep-Alive
#日期
Date: Tue, 27 Sep 2022 06:49:26 GMT
#标签
ETag: "a49-56b5ce607fe00"
#长链接的超时时间和最大连接数
Keep-Alive: timeout=5, max=100
#服务端信息
Server: Apache/2.4.6 (CentOS) PHP/5.4.16

#请求头部信息
#允许请求的主题类型
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
#压缩
Accept-Encoding: gzip, deflate
#允许的语言
Accept-Language: zh-CN,zh;q=0.9
#缓存
Cache-Control: max-age=0  #no-cache 表示无缓存
#连接方式：长格式
Connection: keep-alive
#请求的服务端IP
Host: 10.0.0.8
#上一次修改时间
If-Modified-Since: Fri, 04 May 2018 08:13:44 GMT
If-None-Match: "a49-56b5ce607fe00"
#浏览器向服务器发送的成功信号
Upgrade-Insecure-Requests: 1
#客服端信息
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/105.0.0.0 Safari/537.36
```

![image-20220927164211599](C:\Users\Ma\AppData\Roaming\Typora\typora-user-images\image-20220927164211599.png)



## http请求方式

```bash
#常见请求方式
GET：请求获取一个web页面
POST：文明提取数据，读取界面
DELETE：调用后端接口删除功能的界面
PUT：调佣后端，存储，上传功能界面

#了解即可
CONNTEC：代理服务器
HEAD：读取web界面的头部数据
TRACE：测试服务器请求
OPTION：查询特点的选项
```

![image-20220927164337267](C:\Users\Ma\AppData\Roaming\Typora\typora-user-images\image-20220927164337267.png)



## 状态码

```bash
200 ok

301 永久重定向
302 临时重定向
304 本地缓存 浏览器缓存
307 内部重定向

400 bad request 客服端错误
401 认证失败
403 权限不足 #selinux
404 找不到页面

500 服务器内部错误（代码问题，服务器的问题）
502 Bad Getway 后端服务报错 （那台服务器出现502，就去那太服务器检查机器后端的服务）
503 服务器过载，访问频率过高（快速刷新）
504 后端服务超时
```

![image-20220927164412828](C:\Users\Ma\AppData\Roaming\Typora\typora-user-images\image-20220927164412828.png)

```bahs
#在浏览器中输入blog.driverzeng.com
1 在浏览器中向local DNS发起域名解析请求 本地DNS（/etc/hosts)文件中没有
2 在浏览器中向DNS根域服务器发起请求，解析域名blog.deiverzeng.com
3 DNS进行递归查询和迭代查询
- 递归查询 服务端向服务端查询
- 迭代查询 客服端向服务端查询

.com根域服务器发起查询
.com根域服务器 ->deiverzeng.com
.driverzeng.com -> blog.driverzeng.com A记录 39.104.203.XXX
将 A记录值 IP返还给浏览器

4浏览器和39.104.203.184所在的服务器的80端口建立TCP/IP连接
-三次握手
```

![image-20220927164438738](C:\Users\Ma\AppData\Roaming\Typora\typora-user-images\image-20220927164438738.png)

```bash
防火墙的规则不允许你的ip地址访问80端口，则会拒绝连接，报错返还给用户
防火墙的规则允许你的ip访问80端口，则放行
	-建立连接（TCP/IP三次握手）
				syn（建立连接信号）
客服端---------------------------->服务端
				syn+ack
		（我收到了你建立的连接的请求）

服务端---------------------------->客服端
				ack
 （告诉服务端，我知道你收到建立连接的请求了）

客服端---------------------------->服务端
5 向服务器端的web服务器发起了http请求（负载均衡）
-请求头部信息
	1）请求的方式是什么 GET获取
	2）请求HOST主机是  blog.deiverzeng.com
	3）请求的资源是     /index.html
	4）请求的端口是什么  默认http80 HTTPS443
	5）请求携带的参数是什么  属性（请求类型，压缩，认证，浏览器信息，等待）
	6）请求最后的空行
	
6 将请求根据调度算法 rr（轮询）将请求下发到给后端的web服务器
7 读取web服务器上的nginx配置文件，找到站点目录
8 找到对应的代码文件
	-静态请求：web服务端将静态请求下发到共享存储服务器上
	-动态请求
		将请求发送给后端代码，处理
		先找到数据库的缓存（redis，mencache）
		如果缓存中有数据，直接将数据方会给用户
		如果没有缓存数据，则会找到后端的数据库
		从数据库中取出数据之后，先存入缓存一份，然后再返回给用户
9 返会对应的状态码和响应头部信息给浏览器
10 断开TCP/IP连接
	-四次挥手
```

![image-20220927164505912](C:\Users\Ma\AppData\Roaming\Typora\typora-user-images\image-20220927164505912.png)

```bash
11 浏览器展示网站页面
```

## CDN 

cdn：分布式静态缓存服务器

静态资源：html.css.js.mp3.mp4.avi.jpg.png

1 提升了网站访问速度

2减少后端服务器的压力



![image-20220927164551602](C:\Users\Ma\AppData\Roaming\Typora\typora-user-images\image-20220927164551602.png)



## http 相关术语

```bash
pv 独立页面浏览量 （一条日志，一个请求 ）

uv 独立设备

IP 独立的IP

公司一座大厦 100人 每个人一台电脑一部手机，上网都是通Nat转换出口，每个人点击网站2次
pv 100 * 2 *2 400
uv 100 * 2 200
IP 1
```