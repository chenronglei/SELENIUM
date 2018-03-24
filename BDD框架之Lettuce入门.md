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
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return number  

注意：py及feature对应的语句内容需要一致，不区分大小写    
**运行Lettuce**    
运行cmd,切换到tests目录下，执行"Lettuce"命令    
运行过程很清晰，首先是zero.feature文件的功能描述(feature),然后是场景(scenario)，每一步对应的steps.py中哪一行    
最好给出运行的结果    


        
        
