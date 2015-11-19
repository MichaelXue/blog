# 打造shadowsocks服务端

以CentOs安装shadowsocks为例

* 安装shadowsocks服务
```
yum install m2crypto python-setuptools && easy_install pip
```
```
pip install shadowsocks
```
* 修改配置文件，端口和密码需要更改为你自己的
```
vi /etc/shadowsocks.json
```
```
{
    "server":"0.0.0.0",
    "server_port":端口,
    "local_address": "127.0.0.1",
    "local_port":1080,
    "password":"密码",
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open": false
}
```
    
* 安装Supervisor进程管理工具,保证shadowsocks不会因为各种原因死掉
  ```
  easy_install supervisor
  ```
  * 创建配置文件
  ```
  echo_supervisord_conf > /etc/supervisord.conf
  ```
  * 配置shadowsocks，编辑/etc/supervisord.conf，在最后加入
  ```
  [program:shadowsocks]
  command=ssserver -c /etc/shadowsocks.json
  autostart=true
  autorestart=true
  user=nobody
  ```
  * 默认路径配置启动：
  ```
  supervisord
  ```
  * 开机启动Supervisor
  ```
  vi /etc/rc.local
  ```
  * 添加以下内容
  ```
  supervisord -c /etc/supervisord.conf
  ```
  * 常用命令
  * 
    获得所有程序状态 supervisorctl status

    关闭目标程序 supervisorctl stop ssserver
    
    启动目标程序 supervisorctl start ssserver
    
    关闭所有程序 supervisorctl shutdown

  * 启动关闭（使用Supervisor后，启动关闭交给Supervisor）：
  ```
  ssserver -c /etc/shadowsocks.json -d start
  ```
  ```
  ssserver -c /etc/shadowsocks.json -d stop
  ```

* 更新shadowsocks：
```
pip install --upgrade shadowsocks
```
