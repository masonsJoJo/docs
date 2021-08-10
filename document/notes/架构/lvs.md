### lvs

#### 网络传输模式

客户端cip

 lvs   vip -- dip

服务端rip

---

网络模型

![preview](https://pic1.zhimg.com/v2-2d62ba265be486cb94ab531912aa3b9c_r.jpg)





---

家用路由过程  nat  模式

1. cip:cport-rip:rport     ------      路由器

2. 路由器修改数据包    dip:dport -   rip:rport
3. 服务器返回数据包     rip:rprot  -   dip:dport
4. 路由器 修改数据包    rip:rprot  -   cip:cport

---

lvs D-nat 模式

1. cip:cport-vip:vport     ------     lvs

2. lvs修改数据包    cip:cport  -   rip:rport
3. 服务器返回数据包     rip:rprot  -   cip:cport
   1. 无法直接返回所以，需要服务器的网关，是 lvs 服务器
4. lvs 修改数据包    vip:vprot  -   cip:cport

![image-20210810173146347](C:\Users\wbjiaojian\Desktop\github\Docs\document\notes\架构\lvs img\image-20210810173146347.png)

---

lvs  DR 模型

1. cip:cport-vip:vport     ------     lvs

2. lvs修改数据包的链路层mac地址丢到服务器    cip:cport  -   vip:vport

   1. 服务器需要有一个隐藏的vip

3. 服务器返回数据包     vip:vprot  -   cip:cport

   ![image-20210810173843973](C:\Users\wbjiaojian\Desktop\github\Docs\document\notes\架构\lvs img\image-20210810173843973.png)

----

lvs tunnel 模式

1. cip:cport-vip:vport     ------     lvs

2. lvs修改数据包    cip:cport  -   rip:rport
3. 服务器返回数据包     rip:rprot  -   cip:cport
   1. 无法直接返回所以，需要服务器的网关，是 lvs 服务器
4. lvs 修改数据包    vip:vprot  -   cip:cport