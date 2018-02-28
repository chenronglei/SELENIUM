利用Selenium Grid2可以在不同的主机上建立主节点(hub)和分支节点(node),可以使主节点上的测试用例在不同的分支节点上运行。对于不同的节点，可以搭建不同的测试
环境（操作系统和浏览器），从而使一份用例得到不同环境下的执行结果  
**Selenium Grid的版本**  
Grid1和Grid2的原理和基本工作方式是一样的，只是Grid2支持selenium1和selenium2，并在一些小的功能和易用性上做了优化  
Grid2功能集成在selenium server中  
# 9.1 Selenium Server环境配置  
# 9.2 Selenium Grid的工作原理  
Grid是用于设计帮助我们进行分布式测试的工具，其整体结构由一个hub主节点和若干个node代理节点组成。  
hub用例管理各个代理节点的注册和状态信息，并且接收远程客户端代码的请求调用，然后把请求的命令再转发给代理节点来执行  
使用Grid远程执行测试的代码与直接调用Selenium Server是一样的，只是环境启动的方式不一样，需要同时启动一个hub和至少一个node  
> java -jar selenium-server-standalone.jar -role hub  
> java -jar selenium-server-standalone.jar -role node  

上面的代码分别启动了一个hub和一个node,hub默认端口号为4444，node默认端口号为5555。若是同一台主机上要启动多个node，则需要注意指定端口号，通过下面的方式来启动多个node节点  
> java -jar selenium-server-standalone.jar -role node -port 5555  
> java -jar selenium-server-standalone.jar -role node -port 5556  

当你的测试用例需要验证的环境较多时，可以并行的执行这些用例进而缩短测试总耗时。并行的能力需要借助编程语言的多线程技术  
Grid可以根据用例中指定的平台配置信息把用例转发给符合匹配要求的测试代理  
通过浏览器访问Grid控制台 http://127.0.0.1:4444/grid/console# 通过控制台查看启动的节点信息
