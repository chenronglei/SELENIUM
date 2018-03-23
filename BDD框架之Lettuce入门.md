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


