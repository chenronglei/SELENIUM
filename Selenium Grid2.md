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
# 9.3 Remote应用  
要解释清除Remote的作用不容易，我们可以通过分析Selenium的代码的方式来理解它的作用。这是因为WebDriver针对每一种浏览器都重写WebDriver方法  
> driver=webdirver.Firefox()  
  driver=webdriver.Chrome()  
  driver=webdriver.Ie()  

## 9.3.1 WebDriver驱动分析  
在selenium包的webdriver目录下可以发现任意一个驱动的目录下都有一个webdriver.py。除了Firefox、Chrome、IE等驱动外，还包括非常重要的remote。从这个角度看，也可以把remote看作时一种驱动类型。这种驱动类型比较特别，它不是支持某一款特定的浏览器或平台，而是一种配置模式，在这种配置模式下指定任意的平台或浏览器  
我们脚本中调用Firefox浏览器驱动的路径为：driver=webdirver.Firefox()，那么它时如何指向../selenium/webdriver/firefox/webdriver.py中的WebDriver类的呢？秘密在于../selenium/webdriver/目录下的__init__.py文件  
> from .firefox.webdriver import WebDriver as Firefox  
...  


其实是它对不同驱动的路径做了简化，并且将不同目录下的WebDriver类重命名为相应的浏览器  
Firefox和Chrome的WebDriver类都继承RemoteWebDriver类，查看对应的 ../selenium/webdriver/remote目录下的webdriver.py文件  
> def __init__(self, command_executor='http://127.0.0.1:4444/wd/hub',  
                 desired_capabilities=None, browser_profile=None, proxy=None,  
                 keep_alive=False, file_detector=None, options=None):  

WebDriver类的__init__()初始化方法提供了一个重要信息，即command_executor参数，它默认指向本机(127.0.0.1)的4444端口号，通过修改这个参数可以使其指向任意的某台主机  
浏览器的配置由desired_capabilities参数决定，这个参数的秘密在 ../selenium/webdirver/common/desired_capabilities.py文件中  
> FIREFOX = {  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"browserName": "firefox",  
&nbsp;&nbsp;&nbsp;&nbsp;"marionette": True,  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"acceptInsecureCerts": True,  
&nbsp;&nbsp;&nbsp;&nbsp;}  
&nbsp;&nbsp;&nbsp;&nbsp;INTERNETEXPLORER = {  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"browserName": "internet explorer",  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"version": "",  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"platform": "WINDOWS",  
&nbsp;&nbsp;&nbsp;&nbsp;}  
&nbsp;&nbsp;&nbsp;&nbsp;EDGE = {  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"browserName": "MicrosoftEdge",  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"version": "",  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"platform": "WINDOWS"  
&nbsp;&nbsp;&nbsp;&nbsp;}  
&nbsp;&nbsp;&nbsp;&nbsp;CHROME = {  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"browserName": "chrome",  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"version": "",  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"platform": "ANY",  
&nbsp;&nbsp;&nbsp;&nbsp;}  


"browserName": "firefox"   浏览器  
"version"                  浏览器版本  
"platform": "ANY",         测试平台，any表示任意  

## 9.3.2 Remote实例  
> from selenium.webdriver import Remote  
#调用Remote方法  
driver = Remote(command_executor='http://127.0.0.1:4444/wd/hub',desired_capabilities={'platform':'ANY','browserName':'chrome','version':'','javascriptEnabled':True})  
driver.get('http://www.baidu.com')  
driver.find_element_by_id("kw").send_keys("remote")  
driver.find_element_by_id("su").click()  
driver.quit()  

Remote()方法配置相当于我们直接使用了webdriver.Chrome(),但是Remote()却大大增加了配置的灵活性  
注意：需要下载chrome版本对应的webdriver驱动放置到系统路径下，下载路径 http://chromedriver.storage.googleapis.com/index.html

## 9.3.3 参数化平台及浏览器  
通过Selenium Server可以轻松地创建本地节点和远程节点。而Remote的作用就是配置测试用例在这些节点上执行。  
在本地启动一个hub和两个node(节点)  
> java -jar selenium-server-standalone.jar -role hub  
> java -jar selenium-server-standalone.jar -role node -port 5555  
> java -jar selenium-server-standalone.jar -role node -port 5556  


> from selenium.webdriver import Remote  
from time import sleep  
#定义主机和浏览器  
lists = {'http://127.0.0.1:4444/wd/hub':'chrome',
         'http://127.0.0.1:5555/wd/hub':'firefox',
         'http://127.0.0.1:5556/wd/hub':'MicrosoftEdge'}  
#通过不同的浏览器执行脚本  
for host,browser in lists.items():  
&nbsp;&nbsp;&nbsp;&nbsp;print(host,browser)  
&nbsp;&nbsp;&nbsp;&nbsp;driver = Remote(command_executor=host,
                    desired_capabilities={'platform':'ANY',
                                          'browserName':browser,
                                          'version': '',
                                          'javascriptEnabled':True
                                          }
                    )  
&nbsp;&nbsp;&nbsp;&nbsp;driver.get("http://www.baidu.com")  
&nbsp;&nbsp;&nbsp;&nbsp;driver.find_element_by_id("kw").send_keys('browser')  
&nbsp;&nbsp;&nbsp;&nbsp;driver.find_element_by_id("su").click()  
&nbsp;&nbsp;&nbsp;&nbsp;sleep(5)  
&nbsp;&nbsp;&nbsp;&nbsp;driver.close()  
    
    
修改脚本使其在不同的节点和浏览器上运行 
注意:  
- 执行报错“selenium.common.exceptions.WebDriverException: Message: None”  ： 是版本问题，改成selenium-server-standalone-3.9.1.jar,且执行脚本时，主节点和代理节点都要启动，只启动主节点执行脚本也报错  
- 注意给chrome,edge浏览器安装webdriver驱动  

**启动远程节点**  

在其他主机上启动node,必须满足以下要求:  
- 本地hub主机与远程node主机之间可以ping  
- 远程节点上必须安装用例执行的浏览器和驱动，并且驱动要放在环境变量path的目录下  
- 远程节点上必须安装selenium  
- 远程节点必须安装java环境，运行Selenium Server  

*操作步骤*:  

启动本地hub主机  (操作系统:windows 本地主机ip 192.168.202.1)  
> java -jar E:\python\selenium-server-standalone.jar -role hub  

启动远程node主机 (操作系统:Ubuntu, ip 192.168.202.128)
> java -jar selenium-server-standalone.jar -role node -port 5557 -hub http://192.168.202.1:4444/grid/register

> from selenium.webdriver import Remote  
from time import sleep  
#定义主机和浏览器  
lists = {'http://192.168.202.128:5557/wd/hub':'firefox','http://127.0.0.1:5556/wd/hub':'MicrosoftEdge','http://127.0.0.1:4444/wd/hub':'chrome'}  
#通过不同的浏览器执行脚本  
for host,browser in lists.items():  
&nbsp;&nbsp;&nbsp;&nbsp;print(host,browser)  
&nbsp;&nbsp;&nbsp;&nbsp;#版本有问题，如果node节点上有LINUX系统，则在hub节点上执行时会读取到'platform':'LINUX',进行规避  
&nbsp;&nbsp;&nbsp;&nbsp;if browser == 'chrome':  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;driver = Remote(command_executor=host,
desired_capabilities={'platform':'WIN10',
                                          'browserName':browser,
                                          'version': '',
                                          'javascriptEnabled':True
                                          })  
&nbsp;&nbsp;&nbsp;&nbsp;else:  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;driver = Remote(command_executor=host,
                    desired_capabilities={'platform':'ANY',
                                          'browserName':browser,
                                          'version': '',
                                          'javascriptEnabled':True
                                          })  
&nbsp;&nbsp;&nbsp;&nbsp;driver.get("http://www.baidu.com")  
&nbsp;&nbsp;&nbsp;&nbsp;driver.find_element_by_id("kw").send_keys('browser')  
&nbsp;&nbsp;&nbsp;&nbsp;driver.find_element_by_id("su").click()  
&nbsp;&nbsp;&nbsp;&nbsp;sleep(5)  
&nbsp;&nbsp;&nbsp;&nbsp;driver.close()  

# 9.4 WebDriver驱动  
## 1.支持平台  
webdriver支持Android和BlackBerry两个移动平台的浏览器测试。对于在其他平台上进行自动化测试，笔者推荐Appium  
## 2.支持浏览器  
webdriver支持的浏览器目前包括：Firefox,chrome,IE,Edge,opera,safari  
## 3.支持模式  
HtmlUnit和PhantomJS是两个比较特殊的模式，可以看作是伪浏览器，在这种模式下支持html、Java Script等的解析，但不会真正渲染出界面。由于不进行CSS及GUI渲染，所以运行效率上要比真实浏览器快很多，主要用在功能性测试上面  




