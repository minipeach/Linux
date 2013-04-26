#screen是什么


<p>也许你遇到过使用telnet或SSH远程登录linux,运行一些程序。如果这些程序需要运行很长时间(几个小时)，而程序运行过程中出现网络故障，或者客户机故障，这时候客户机与远程服务器的链接将终端，并且远程服务器没有正常结束的命令将被迫终止。</p>
<p>又比如你SSH到主机上后，开始批量的scp命令，如果这个ssh线程断线了，scp进程就中断了。在远程服务器上正在运行某些耗时的作业，但是工作还没做完快要下班了，退出的话就会中断操作了，如何才好呢？</p>
<p><strong> 我们利用screen命令可以很好的解决这个问题。</strong>实现在断开SSH的情况下,在服务器上继续执行程序。</p>
<p><strong>那什么是screen命令?</strong></p>
<p>Screen被称之为一个全屏窗口管理器，用他可以轻松在一个物理终端上获得多个虚拟终端的效果。</p>
<p><strong>Screen功能说明</strong>：</p>
<p>简单来说，Screen是一个可以在多个进程之间多路复用一个物理终端的窗口管理器,这意味着你能够使用一个单一的终端窗口运行多终端的应用。Screen中有会话的概念，用户可以在一个screen会话中创建多个screen窗口，在每一个screen窗口中就像操作一个真实的telnet/SSH连接窗口那样。</p>

<p>当你使用多标签终端管理器，比如secureCRT的时候，也许会使用自动登录，或者每台服务器打开一个标签页，来回切换，有没有觉得都很麻烦呢，如果能一直保持着会话，不用每次都登陆或者切换，岂不是很方便，好吧，我建议你使用screen</p>


#什么情况下使用screen

除了上面说的之外，还需要一些必要条件，比如有人问那我能不能screen运行在本机，答案当然是可以，不过它本身就是一个程序，关机后进程都会被kill掉，screen自然也会断开所有链接，除非你自己的pc或者笔记本永不关机呵呵，不过那是不可能的，所以，还是使用中转服务器吧，比如在公司和企业里的开发测试人员用的那种，一般情况下不会随随便便关机的，而且比较稳定。


#开始使用screen

1. 首先要找到常年不关机并且稳定的机器（一般是自己的开发测试机），比如我们这里找到一台机器hostname为myserver，ip为192.168.1.101，我自己的机器ip为192.168.1.100

2. 从你的本机ssh到myserver机器“ssh myserver/ssh 192.168.1.101”  （建议打通ssh，参见： [设置 SSH 自动登陆](http://www.douban.com/group/topic/19654908/)）

3. 假设myserver上你的个人目录为/home/taozi ,编辑文件/home/taozi/.screenrc ,内容参见：[screen配置](screenrc.md)

4. 为了防止ssh无操作或超时自动断开，我们做如下配置，编辑文件/home/taozi/.ssh/config 
```shell
TCPKeepAlive yes
ServerAliveInterval 50
ServerAliveCountMax 6
```

5. 退出myserver，然后重新从本机ssh过去。

6. 执行screen -S taozi_test  (S为大写，taozi_test为你的screen名称，可自定义)

7. 如无意外，可以看到我们已经进入screen的终端界面了，底部会显示当前的会话和时间 

8. 执行ctrl+a c ，可以看到新建了一个shell窗口，此时底部显示 0-$ bash  (1*$bash)

9. 不同shell窗口用数字表示，比如0，1，2 ...  ， "-"表示上一个shell窗口，   "*"表示当前所在的shell窗口

10. 不同窗口之间切换使用ctrl+a n (n表示窗口的数字序号)

11. 使用ctrl+a shift+a 可以重命名当前shell窗口的名字，便于快速切换

12. 执行ctrl+a d ,可以退出screen（但是shell窗口和连接依然存在的），同时屏幕显示[detached] ，表示已经保存了当前screen

13. 执行screen -ls ,可以查看当前所有screen的连接 

14. 执行screen -r taozi_test , 可以恢复screen界面 


#screen快捷键及其他功能

##screen窗口内操作

- ctrl+a c：创建一个新的 window（可理解为shell窗口）
- ctrl+a ctrl-a：在 Shell 间切换
- ctrl+a n：切换到下一个 Shell
- ctrl+a p：切换到上一个 Shell
- ctrl+a 0…9：切换到指定 Shell
- ctrl+a d：退出 Screen 会话

- ctrl+a [ : 进入copy 模式，和vi一样使用
- ctrl+a shift+s : 切分窗口
- ctrl+a tab :  切分的窗口间切换
- ctrl+a shift+x : 关闭切分的窗口


##screen本身的操作

- screen -S yourname -> 新建一个叫yourname的session
- screen -ls -> 列出当前所有的session
- screen -r yourname -> 回到yourname这个session
- screen -d yourname -> 远程detach某个session
- screen -d -r yourname -> 结束当前session并回到yourname这个session

在每个screen session 下，所有命令都以 ctrl+a(C-a) 开始。

