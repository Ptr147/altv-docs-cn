#直接连接协议

此协议可用于通过浏览器连接到alt:V服务器。

协议格式:

`altv://connect/ADDRESS?password=PASSWORD` 或 `altv://connect/ADDRESS`

- ADDRESS is [urlencoded](https://www.urlencoder.org/) connection address, exactly like used in "Direct connect" window
- PASSWORD (optional) is [urlencoded](https://www.urlencoder.org/) password

案例: 

- `my.cdn.server:80` -> `altv://connect/my.cdn.server%3A80`
- `http://my.cdn.server` -> `altv://connect/http%3A%2F%2Fmy.cdn.server`
- `[::1]:1234` with password `test` -> `altv://connect/%5B%3A%3A1%5D%3A1234?password=test`
