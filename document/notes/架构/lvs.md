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

![image-20210810173146347](lvs img\image-20210810173146347.png)

---

lvs  DR 模型

1. cip:cport-vip:vport     ------     lvs

2. lvs修改数据包的链路层mac地址丢到服务器    cip:cport  -   vip:vport

   1. 服务器需要有一个隐藏的vip

3. 服务器返回数据包     vip:vprot  -   cip:cport

   ![image-20210810173843973](lvs img\image-20210810173843973.png)

----

lvs tunnel 模式

1. cip:cport-vip:vport     ------     lvs

2. lvs修改数据包    cip:cport  -   rip:rport
3. 服务器返回数据包     rip:rprot  -   cip:cport
   1. 无法直接返回所以，需要服务器的网关，是 lvs 服务器
4. lvs 修改数据包    vip:vprot  -   cip:cport



---



#### 调度算法

##### 静态调度(Fixed Scheduling Method)

###### 轮询（Round Robin，rr）

<img src="lvs img\7vaj9vorbi.png" alt="img" style="zoom:30%;" />

###### 加权轮询（Weighted Round Robin，wrr）

<img src="lvs img\q60lx7waho.png" alt="q60lx7waho" style="zoom:33%;" />

###### 目标地址散列（Destination Hashing，dh）

> `目标地址散列`调度算法根据请求的目标IP地址，作为散列键（Hash Key）从静态分配的散列表找出对应的服务器，若该服务器是可用的且未超载，将请求发送到该服务器，否则返回空。

<img src="lvs img\pqn6xoty97.png" alt="pqn6xoty97" style="zoom:33%;" />



###### 源地址散列（Source Hashing，sh）

<img src="lvs img\2q89dfosyg.png" alt="2q89dfosyg" style="zoom:33%;" />



----

##### 动态(Dynamic Scheduling Method)

###### 最少链接（Least Connnections，lc）

<img src="lvs img\17kjn6uw9q.png" alt="17kjn6uw9q" style="zoom:33%;" />

###### 加权最少链接（Weighted Least Connnections，wlc）

<img src="lvs img\c9ojz9z047.png" alt="c9ojz9z047" style="zoom:33%;" />

###### 最短期望延迟（Shortest Expected Delay，sed）

> 基于wlc算法。这个必须举例来说了ABC三台机器分别权重123 ，连接数也分别是123。那么如果使用WLC算法的话一个新请求进入时它可能会分给ABC中的任意一个。使用sed算法后会进行这样一个运算A:（1+1)/1B:（1+2)/2C:（1+3)/3根据运算结果，把连接交给C 。

<img src="lvs img\6u4ikfhv0u.png" alt="6u4ikfhv0u" style="zoom:33%;" />

###### 最少队列调度（Never Queue Scheduling）

<img src="lvs img\y9p4f2cpso.png" alt="y9p4f2cpso" style="zoom:33%;" />

###### 基于局部性的最少链接（Locality-Based Least Connections，lblc）

<img src="lvs img\374sfdjlgf.png" alt="374sfdjlgf" style="zoom:33%;" />

###### 带复制的基于局部性最少链接（Locality-Based Least Connections with Replication，lblcr）

<img src="lvs img\r6gj3kuk5n.png" alt="r6gj3kuk5n" style="zoom:33%;" />

#### DR模式 lvs 搭建

##### VIP 隐藏

/proc/sys/net/ipv4/conf/\*IF*/

arp_ignore:定义接收到的arp请求时的响应级别  0，1

arp_announce:定义将自己的地址向外通告时的通告级别 0，1，2

![image-20210811160004449](lvs img\image-20210811160004449.png)

增加vip 配置再 lo local loopback 环卫接口上



##### ipvs

1. ipvs(ip virtual server)：一段代码工作在内核空间，叫ipvs，是真正生效实现调度的代码。

2. ipvsadm：另外一段是工作在用户空间，叫ipvsadm，负责为ipvs内核框架编写规则，定义谁是集群服务，而谁是后端真实的服务器(Real Server)

   

   ![image-20210811155855134](lvs img\image-20210811155855134.png)
