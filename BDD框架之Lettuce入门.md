# 12.1 什么是BDD
**TDD**:测试驱动开发  
**ATDD**:演示测试驱动开发  
**BDD**:行为驱动开发  
Lettuce是Python语言下的BDD框架。它用于python项目的自动化测试，它可以执行纯文本的功能描述，一个非常有用且迷人的BDD框架  
Lettuce使开发和测试过程变得容易，它又很好的扩展性、可读性，它允许我们使用自然语言取描述一个系统的行为
# 12.2 安装Lettuce
只能在python2下安装，且安装目录要在python2对应的目录下  
# 12.3 阶乘的例子
## 12.3.2 编写BDD实现
创建以下目录结构:
projects/mymath/tests/  
&nbsp;&nbsp;&nbsp;&nbsp;features/  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;zero.feature  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;steps.py  

zero.feature  :

> **Feature**: Compute factorial  
&nbsp;&nbsp;&nbsp;&nbsp;In order to play with Lettuce  
&nbsp;&nbsp;&nbsp;&nbsp;As beginners  
&nbsp;&nbsp;&nbsp;&nbsp;We'll implement factorial  
&nbsp;&nbsp;**Scenario**: Factorial of 0  
&nbsp;&nbsp;&nbsp;&nbsp; **Given** I have the number 0  
&nbsp;&nbsp;&nbsp;&nbsp;**When** I compute its factorial  
&nbsp;&nbsp;&nbsp;&nbsp;**Then** I see the number 1  

第一段为功能介绍，描述需要实现什么功能；第二个为场景描述，也可以看作使一条测试用例，当我们输入什么数据，执行什么操作后，预期程序应该返回什么结果  
Lettuce虽然使用了自然语言的描述，却也有语法规则。非常简单，有以下几个关键字:  
- Feature(功能)   test suite(unittest的测试用例集)    
- Scenario(情景)  test case(unittest的测试用例)    
- Given(给定)     setup(unittest的测试步骤)    
- And(和)         
- When(当)        test run(unittest的触发测试执行)
- Then(则)        assert(unittest的断言，验证结果)    

steps.py  
> from lettuce import *  
#@step是Python装饰器的写法，也就是have_the_number()函数由@step()进行装饰  
#I have the number (\d+)对应于zero.feature文件中的第六句"Givern I have the number 0"
@step('I have the number (\d+)')  
def have_the_number(step,number):  
&nbsp;&nbsp;&nbsp;&nbsp;world.number = int(number)  
#把have_the_number()函数中的world.number的变量0作为factorial()函数的入参  
#I compute its factorial对应于zero.feature文件中的第七句"When I compute its factorial"    
@step('I compute its factorial')  
def compute_its_fatorial(step):  
&nbsp;&nbsp;&nbsp;&nbsp;world.number = factorial(world.number)  
#I see the number (\d+)对应于zero.feature文件中的第八句"Then I see the number1"    
@step('I see the number (\d+)')  
def check_number(step,expected):  
&nbsp;&nbsp;&nbsp;&nbsp;expected = int(expected)  
&nbsp;&nbsp;&nbsp;&nbsp;assert world.number == expected,"Got %d" % world.number  
def factorial(number):  
&nbsp;&nbsp;&nbsp;&nbsp;number = int(number)  
&nbsp;&nbsp;&nbsp;&nbsp;if (number == 0) or (number == 1):  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return 1  
&nbsp;&nbsp;&nbsp;&nbsp;else:  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return number*factorial(n-1)  

注意：py及feature对应的语句内容需要一致，不区分大小写    
**运行Lettuce**    
运行cmd,切换到tests目录下，执行"Lettuce"命令    
运行过程很清晰，首先是zero.feature文件的功能描述(feature),然后是场景(scenario)，每一步对应的steps.py中哪一行    
最好给出运行的结果    

## 12.3.3 添加测试场景  
zero.feature  :

> **Feature**: Compute factorial  
&nbsp;&nbsp;&nbsp;&nbsp;In order to play with Lettuce  
&nbsp;&nbsp;&nbsp;&nbsp;As beginners  
&nbsp;&nbsp;&nbsp;&nbsp;We'll implement factorial  
&nbsp;&nbsp;**Scenario**: Factorial of 0  
&nbsp;&nbsp;&nbsp;&nbsp; **Given** I have the number 0  
&nbsp;&nbsp;&nbsp;&nbsp;**When** I compute its factorial  
&nbsp;&nbsp;&nbsp;&nbsp;**Then** I see the number 1  
&nbsp;&nbsp;Scenario: Factorial of 1    
&nbsp;&nbsp;&nbsp;&nbsp;Given I have the number 1    
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;When I compute its factorial    
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Then I see the number 1    
&nbsp;&nbsp;Scenario: Factorial of 2    
&nbsp;&nbsp;&nbsp;&nbsp;Given I have the number 2    
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;When I compute its factorial    
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Then I see the number 2    
&nbsp;&nbsp;Scenario: Factorial of 3    
&nbsp;&nbsp;&nbsp;&nbsp;Given I have the number 3    
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;When I compute its factorial    
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Then I see the number 6 

## 12.3.4 Lettuce的目录结构与执行过程  
BDD的开发主要与两类文件打交道:feature文件和相应的step文件    
feature文件是以feature为后缀名的文件,以Given-When-Then的方式描述系统的场景(scenarios)行为  
step文件为普通的python程序文件,Feature文件的每一个Given-When-Then步骤在step文件中都有对应的python执行代码，两类文件通过正则表达式相关联  

Feature文件一定要在features目录下，否则会提示“could not find features at \features”。而step文件可以放在任意目录下都能被执行到  

# 12.4 Lettuce_webdriver自动化测试  
Lettuce_webdriver属于独立的python第三方扩展，它支持通过Lettuce运行Selenium WebDriver自动化测试用例  
**安装Lettuce**  
**安装Lettuce_webdriver**  
> pip install lettuce_webdriver 

**安装nose**  
nose继承自unittest,属于第三方的python单元测试框架，更容易使用。Lettuce_webdriver的运行依赖于nose框架  
> pip install nose  

同样以百度搜索为例，创建如下目录结构  
tests/features/
&nbsp;&nbsp;&nbsp;&nbsp;step_definitions/  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;steps.py  
&nbsp;&nbsp;&nbsp;&nbsp;support/  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;terrain.py  
&nbsp;&nbsp;&nbsp;&nbsp;baidu.feature  

baidu.feature文件遵循BBD行为描述规则，编写如下内容:  
> Feature: Baidu search test case  
&nbsp;&nbsp;Scenario: search selenium  
&nbsp;&nbsp;&nbsp;&nbsp;Given I go to "http://www.baidu.com/"  
&nbsp;&nbsp;&nbsp;&nbsp;When I fill in field with id "kw" with "selenium"  
&nbsp;&nbsp;&nbsp;&nbsp;And I click id "su" with baidu once  
&nbsp;&nbsp;&nbsp;&nbsp;Then I should see "51testing.org" within 2 second      
&nbsp;&nbsp;Scenario: search lettuce_webdriver  
&nbsp;&nbsp;&nbsp;&nbsp;Given I go to "http://www.baidu.com/"  
&nbsp;&nbsp;&nbsp;&nbsp;When I fill in field with id "kw" with "webdriver"  
&nbsp;&nbsp;&nbsp;&nbsp;And I click id "su" with baidu once  
&nbsp;&nbsp;&nbsp;&nbsp;Then I should see "blog.csdn.net" within 2 second  
&nbsp;&nbsp;&nbsp;&nbsp;#多个Scenario，最后一个close浏览器  
&nbsp;&nbsp;&nbsp;&nbsp;Then I close browser  

在step_definitions目录下编写相应的测试脚本  
steps.py  
> /# coding=utf-8  
from lettuce import *  
from lettuce_webdriver.util import assert_false  
from lettuce_webdriver.util import AssertContextManager  
def input_frame(browser,attribute):  
&nbsp;&nbsp;&nbsp;&nbsp;xpath = "//input[@id='%s']" % attribute  
&nbsp;&nbsp;&nbsp;&nbsp;elems = browser.find_elements_by_xpath(xpath)  
&nbsp;&nbsp;&nbsp;&nbsp;return elems[0] if elems else False  
def click_button(browser,attribute):  
&nbsp;&nbsp;&nbsp;&nbsp;xpath = "//input[@id='%s']" % attribute  
&nbsp;&nbsp;&nbsp;&nbsp;elems = browser.find_elements_by_xpath(xpath)  
&nbsp;&nbsp;&nbsp;&nbsp;return elems[0] if elems else False  
#定位输入框输入关键字  
@step('I fill in field with id "(.*?)" with "(.*?)"')  
def baidu_text(step,field_name,value):  
&nbsp;&nbsp;&nbsp;&nbsp;with AssertContextManager(step):  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;text_field = input_frame(world.browser,field_name)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;text_field.clear()  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;text_field.send_keys(value)  
#点击“百度一下”按钮  
@step('I click id "(.*?)" with baidu once')  
def baidu_click(step,field_name):  
&nbsp;&nbsp;&nbsp;&nbsp;with AssertContextManager(step):  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;click_field = click_button(world.browser,field_name)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;click_field.click()  
#关闭浏览器  
@step('I close browser')  
def close_browser(step):  
&nbsp;&nbsp;&nbsp;&nbsp;world.browser.quit()  
    
在support目录下创建terrain.py文件，用于定义测试脚本的基本配置  
terrain.py  
> from selenium import webdriver  
from lettuce import before,world  
import lettuce_webdriver.webdriver  
@before.all  
def setup_browser():  
&nbsp;&nbsp;&nbsp;&nbsp;world.browser = webdriver.Firefox()  

terrain文件配置浏览器驱动，作用于所有测试用例  

在.../tests/目录下输入'Lettuce'命令执行测试脚本
