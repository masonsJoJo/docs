### nginx 安装

---

nginx 下载地址 http://nginx.org/en/download.html

## 安装

- 首先安装gcc-c++

yum -y install gcc-c++

- 编译nginx

./configure --prefix=/usr/local/nginx

提示异常，需要pcre 库

/configure: error: the HTTP rewrite module requires the PCRE library.
You can either disable the module by using --without-http_rewrite_module
option, or install the PCRE library into the system, or build the PCRE library
statically from the source with nginx by using --with-pcre=<path> option.

- 安装p c re 

yum install -y pcre pcre-devel

再执行编译还是提示错误，需要zlib 

- 安装zlib

yum install -y zlib zlib-devel

- 在执行编译成功

- 执行make

  报错没有命令

  ```javascript
  yum -y install gcc automake autoconf libtool make
  ```

- make

- make install

  结束安装

  ---

  ## 使用

  > 负载均衡

  Upstream

  Weight 权重

  >  反向代理  

  

  

  > 安全问题

  ​	反向代理服务安全

   代理服务器请求被拦截，需要https解决

  ​	nginx 和 被代理服务，内网

​		正向代理安全

​		也需要https

>  https 配置
>
> 证书申请 
>
> 地址：腾讯

​		s s l_certificate /.....crt;

​		ssl_certificate_key /......key;





> 动静分离

location /css {

​	root /...../static;

}  把静态文件放在nginx static下

或者 location ～*/（c s s）正则



root / alias. 虚拟目录





