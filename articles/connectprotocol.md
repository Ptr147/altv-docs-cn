#直接连接协议

此协议可用于通过浏览器连接到alt:V服务器。

## CDN关闭 
如果您的服务器不使用cdn功能,您需要使用服务器的IP地址和端口。

`altv://connect/${IP_ADDRESS}:${PORT}?password=${PASSWORD}` 或者例如`altv://connect/127.0.0.1:7788?password=xyz`

## CDN开启  

如果您正在使用cdn功能,您需要使用cdn的直接连接URL。

`altv://connect/http://{CDN_URL}?password=${PASSWORD}` 或者例如`altv://connect/http://connect.example.com?password=xyz` 