**单元测试框架主要完成以下三件事**:  
- 提供用例组织与执行  当大量测试用例堆砌在一起，产生了扩展性和可维护性的问题  
- 提供丰富的比较方法  不能功能测试还是单元测试，用例执行完成后，需要将实际执行结果与预期结果进行比较(断言)  
- 提供丰富的日志  当用例执行失败，能清晰的抛出失败原因，当所有用例执行完成，能提供丰富的执行结果。如总执行时间、失败用例数、成功执行数等  
# 7.1 认识unittest
单元测试负责对最小的软件测试单元（模块）进行验证，它使用软件设计文档中对模块的描述作为指南，对重要的程序分支进行测试以发下模块中的错误  
Python语言下有多种单元测试框架，如doctest、unittest、pytest、nose等，unittest(原名PyUnit框架)为Python语言自带的单元测试框架  
## 7.1.1 认识单元测试
**不用单元测试框架也可以写单元测试**  
被测试类calculator.py:  
> #计算机类  
class Count:  
&nbsp;&nbsp;&nbsp;&nbsp;def __init__(self,a,b):  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;self.a = int(a)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;self.b = int(b)  
&nbsp;&nbsp;&nbsp;&nbsp;#计算加分  
&nbsp;&nbsp;&nbsp;&nbsp;def add(self):  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return self.a + self.b   
        
不用测试框架的单元测试:  
> from calculator import Count  
#测试两个正式相加  
class TestCount:  
&nbsp;&nbsp;&nbsp;&nbsp;def test_add(self):
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;#如果try内执行异常，except抛出自定义异常。如果try正常，执行else  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;try:  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;j = Count(2,3)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;add = j.add()  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;assert(add == 5),'Integer addition result error!'
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;except AssertionError as msg:
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;print (msg)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;else:
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;print ('Test pass!')
#执行测试类的方法  
mytest=TestCount()  
mytest.test_add()  

存在的问题：首先测试的程序的写法没有一定的规范可以遵循，不统一的代码维护起来十分麻烦；其次，需要编写大量的辅助代码才能进行单元测试  
为了让单元测试更肉昂扬维护和编写，最好的方式是遵循一定的规范来编写测试用例，这也是单元测试框架的初衷  
**用单元测试框架写单元测试**  
> from calculator import Count  
import unittest  
#创建TestCount类继承TestCase类，TestCase类看出是对特定类进行测试的集合  
class TestCount(unittest.TestCase):  
&nbsp;&nbsp;&nbsp;&nbsp;#setUp()方法是测试用例执行前的初始化工作  
&nbsp;&nbsp;&nbsp;&nbsp;def setUp(self):  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;print("test start")  
&nbsp;&nbsp;&nbsp;&nbsp;def test_add(self):  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;j = Count(2,3)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;#调用unittest框架所提供的assertEqual()方法对add()返回值进行断言  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;self.assertEqual(j.add(),5)  
&nbsp;&nbsp;&nbsp;&nbsp;#tearDown()方法是测试用例执行后的善后工作  
&nbsp;&nbsp;&nbsp;&nbsp;def tearDown(self):    
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;print ("test end")  
#直接使用  
if __name__ == '__main__':  
#unittest提供了全局main()方法，方便地将一个单元测试模块变成可直接运行的脚本，自动执行'test'命令开头的测试方法  
&nbsp;&nbsp;&nbsp;&nbsp;unittest.main()  

执行结果  
> ==================== RESTART: E:\python\3.6\7\7.1.1_2.py ====================
test start
test end
.
----------------------------------------------------------------------
Ran 1 test in 0.010s

OK



