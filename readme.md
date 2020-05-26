# SDN实验过程

1. 安装好mininet和ryu控制器程序

2. 依次执行`sudo ryu-manager simple_switch.py `启动ryu控制器、模拟拓扑`sudo mn --custom example1.py --topo mytopo --controller=remote,ip=127.0.0.1,port=6633`

3. 2中的example1.py 是自己定义的拓扑图，比较简单（也可生成更加复杂的拓扑图），代码如下

   ```python
   from mininet.topo import Topo
   
   class MyTopo( Topo ):
       "Simple topology example."
   
       def build( self ):
           "Create custom topo."
   
           # Add hosts and switches
           Host1 = self.addHost( 'h1' , ip='10.0.0.1') #注意这里的ip后续要用到
           Host2 = self.addHost( 'h2' , ip='10.0.0.2')
           Host3 = self.addHost( 'h3' , ip='10.0.0.3')
           Switch1 = self.addSwitch( 's1' )
           Switch2 = self.addSwitch( 's2' )
   
           # Add links
           self.addLink( Switch1, Switch2 )
           self.addLink( Host1, Switch1 )
           self.addLink( Host2, Switch1 )
           self.addLink( Switch2, Host3 )
   
   
   topos = { 'mytopo': ( lambda: MyTopo() ) }
   
   ```

4. 启动wireshark，进行抓包。这里的wireshark的过程应该对应实际的流量收集器

5. 在mininet中进入h1、h2 `xterm h1 h2`。这里h1、h2 都是虚拟出来的，但是却可以在各自窗口执行不同的命令比如伪dos攻击，伪端口扫描。

6. 在h1的终端中输入命令`hping3 -c 5000 -d 150 -S -w 64 -p 80 --flood 10.0.0.2`，进行伪dos攻击

7. 结束wireshark抓包。


## 学习总结

1. 再看完多篇硕士毕业论文，感觉很水。绝大多数论文中的恶意流量检测都只有ddos，也说从ryu控制器层面解决ddos的问题，这里应该有一个下发drop流表的工作。在模拟mininet中，使用CICFowMeter进行抓包分析。用kdd99数据集使用聚类算法。
2. 北邮的利用sdn做蜜罐系统的毕业论文中，非常粗略地看完。对arp，以及一个已知的漏洞程序做了测试。（自己的想法）如果部署漏洞是二级制题目，对异常流量的检测。没有保存恶意流量的包。
3. 由于时间问题，可能没有更多的时间来研究ryu控制器里面的代码。

PS：这次帮学弟解决他们信息安全作品赛中的一些问题也顺便自己了解了下sdn。