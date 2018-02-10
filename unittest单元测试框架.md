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

## 7.1.2 重要的概念
**Test Case**  
一个Test Case的实例就是一个测试用例。就是一个完整的测试流程，包括测试前测试环境的搭建(setUp)、实现测试过程的代码(run)，以及测试后环境的还原(tearDown)  
单元测试的本质就是一个测试用例就是一个完整的测试单元，通过执行这个测试单元，可以对某一个功能进行验证  
**Test Suite**  
一个功能的验证往往需要多个测试用例，可以把多个测试用例集合在一起来执行来执行，这就产生了测试套件TestSuite的概念。  
**Test Runner**  
测试的执行也是单元测试的一个重要概念，一般单元测试框架中都会提供丰富的执行策略和执行结果  
在unittest中，通过TextTestRunner类提供的run()方法来执行test suite/test case。test runner可以使用图形界面、文本界面，或返回一个特殊值等方式来表示测试执行的结果  
**Test Fixture**  
对一个测试用例环境的搭建和销毁，就是一个fixture,通过覆盖TestCase的setUp()和tearDown()方法来实现。  
注意：tearDown的过程很重要，要为下一个test case留下一个干净的环境  
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
&nbsp;&nbsp;&nbsp;&nbsp;def test_add2(self):  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;j = Count(41,76)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;print("123")  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;self.assertEqual(j.add(),117)  
&nbsp;&nbsp;&nbsp;&nbsp;#tearDown()方法是测试用例执行后的善后工作  
&nbsp;&nbsp;&nbsp;&nbsp;def tearDown(self):  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;print ("test end")  
if __name__ == '__main__':  
#构造测试集  
&nbsp;&nbsp;&nbsp;&nbsp;#调用unittest框架TestSuite()类来创建测试套件  
&nbsp;&nbsp;&nbsp;&nbsp;suite = unittest.TestSuite()  
&nbsp;&nbsp;&nbsp;&nbsp;#调用测试套件的addTest()方法来添加测试用例test_add()、test_add2()  
&nbsp;&nbsp;&nbsp;&nbsp;#TestCount("test_add")表示TestCount类的test_add方法  
&nbsp;&nbsp;&nbsp;&nbsp;suite.addTest(TestCount("test_add"))  
&nbsp;&nbsp;&nbsp;&nbsp;suite.addTest(TestCount("test_add2"))  
&nbsp;&nbsp;&nbsp;&nbsp;#print (suite)  
#执行测试,调用unittest框架的TextTestRunner()类，通过它的run()方法来运行suite所组装的测试用例  
&nbsp;&nbsp;&nbsp;&nbsp;runner = unittest.TextTestRunner()  
&nbsp;&nbsp;&nbsp;&nbsp;runner.run(suite)  

## 7.1.3 断言方法  
unittest框架的TestCase类提供以下方法用于测试结果的判断  
- assertEqual(first,second,msg=None) 断言第一个参数与第二个参数是否相等，如果不相等则测试失败，msg是可选参数，用于测试失败时打印的信息  
> from calculator import Count  
import unittest  
#创建TestCount类继承TestCase类，TestCase类看出是对特定类进行测试的集合  
class Test(unittest.TestCase):  
&nbsp;&nbsp;&nbsp;&nbsp;def setUp(self):  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;number = input("Enter a number:")  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;self.number = int(number)  
&nbsp;&nbsp;&nbsp;&nbsp;def test_case(self):  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;self.assertEqual(self.number,10,msg="Your number is not 10")  
&nbsp;&nbsp;&nbsp;&nbsp;def tearDown(self):  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;pass  
if __name__ == '__main__':  
&nbsp;&nbsp;&nbsp;&nbsp;unittest.main()  

- assertNotEqual(first,second,msg=None)  与assertEqual()相反  
- assertTrue(expr,msg=None)  测试表达式是true  
- assertFalse(expr,msg=None) 测试表达式是flase  
被测试的count.py  
> #用于判断质数  
def is_prime(n):  
&nbsp;&nbsp;&nbsp;&nbsp;if n <= 1:  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return False  
&nbsp;&nbsp;&nbsp;&nbsp;for i in range(2,n):  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;if(n%i ==0):  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return False  
&nbsp;&nbsp;&nbsp;&nbsp;return True  

测试  
> from count import is_prime  
import unittest  
#创建TestCount类继承TestCase类，TestCase类看出是对特定类进行测试的集合  
class Test(unittest.TestCase):  
&nbsp;&nbsp;&nbsp;&nbsp;def setUp(self):  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;print ("start")  
&nbsp;&nbsp;&nbsp;&nbsp;def test_case(self):  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;self.assertTrue(is_prime(8),msg="Your number is not prime!")  
&nbsp;&nbsp;&nbsp;&nbsp;def tearDown(self):  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;pass  
if __name__ == '__main__':  
&nbsp;&nbsp;&nbsp;&nbsp;unittest.main()  

- assertIn(first,second,msg=None) 断言第一个参数是否在第二个参数中  
- assertNotIn(first,second,msg=None) 断言第一个参数是否在第二个参数中  
> import unittest  
class Test(unittest.TestCase):  
&nbsp;&nbsp;&nbsp;&nbsp;def setUp(self):  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;print ("start")  
&nbsp;&nbsp;&nbsp;&nbsp;def test_case(self):  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;a = 'lucy1'  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;b= 'lucy is good girl'  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;self.assertIn(a,b,msg="Wrong!")  
&nbsp;&nbsp;&nbsp;&nbsp;def tearDown(self):  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;pass   
if __name__ == '__main__':    
&nbsp;&nbsp;&nbsp;&nbsp;unittest.main()  

- assertIs(first,second,msg=None) 断言第一个与第二个参数为同一个对象  
- assertIsNot(first,second,msg=None) 断言第一个与第二个参数不为同一个对象  
- assertIsNone(expr,msg=None) 断言表达式是None对象  
- assertIsNotNone(expr,msg=None) 断言表达式不是None对象  
- assertIsInstance(obj,cla,msg=None) 断言obj是cls的一个实例  
- assertIsNotInstance(obj,cla,msg=None) 断言obj不是cls的一个实例 

## 7.1.4 组织单元测试用例
当我们增加被测功能和相应的测试用例之后，unittest单元测试框架需要扩展和组织新增的测试用例  
扩展calculator.py的sub()方法
> from calculator import Count  
import unittest  
#创建TestAdd()、TestSub(）两个测试类  
class TestAdd(unittest.TestCase):  
&nbsp;&nbsp;&nbsp;&nbsp;def setUp(self):  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;print ("start add")  
&nbsp;&nbsp;&nbsp;&nbsp;def test_add1(self):  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;a = Count(3,5)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;self.assertEqual(a.add(),8,msg='is not 8')  
&nbsp;&nbsp;&nbsp;&nbsp;def test_add2(self):  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;a = Count(4,6)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;self.assertEqual(a.add(),10,msg='is not 10')  
&nbsp;&nbsp;&nbsp;&nbsp;def tearDown(self):  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;print ("end add")  
class TestSub(unittest.TestCase):  
&nbsp;&nbsp;&nbsp;&nbsp;def setUp(self):  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;print ("start sub")  
&nbsp;&nbsp;&nbsp;&nbsp;def test_sub1(self):  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;a = Count(5,3)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;self.assertEqual(a.sub(),2,msg='is not 2')  
&nbsp;&nbsp;&nbsp;&nbsp;def test_sub2(self):  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;a = Count(8,4)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;self.assertEqual(a.sub(),4,msg='is not 4')  
&nbsp;&nbsp;&nbsp;&nbsp;def tearDown(self):  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;print ("end sub")   
if __name__ == '__main__':    
&nbsp;&nbsp;&nbsp;&nbsp;#构造测试集  
&nbsp;&nbsp;&nbsp;&nbsp;suite = unittest.TestSuite()  
&nbsp;&nbsp;&nbsp;&nbsp;suite.addTest(TestAdd("test_add1"))  
&nbsp;&nbsp;&nbsp;&nbsp;suite.addTest(TestAdd("test_add2"))  
&nbsp;&nbsp;&nbsp;&nbsp;suite.addTest(TestSub("test_sub1"))  
&nbsp;&nbsp;&nbsp;&nbsp;suite.addTest(TestSub("test_sub2"))  
&nbsp;&nbsp;&nbsp;&nbsp;#运行测试集合  
&nbsp;&nbsp;&nbsp;&nbsp;runner = unittest.TextTestRunner()  
&nbsp;&nbsp;&nbsp;&nbsp;runner.run(suite)  

2个类的setUp()和tearDown()所做的事情是一样的，可以封装成一个自己的测试类  
> from calculator import Count  
import unittest  
class MyTest(unittest.TestCase):  
&nbsp;&nbsp;&nbsp;&nbsp;def setUp(self):  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;print("test case start")  
&nbsp;&nbsp;&nbsp;&nbsp;def tearDown(self):  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;print("test case end")  
#创建TestAdd()、TestSub(）两个测试类  
class TestAdd(MyTest):  
&nbsp;&nbsp;&nbsp;&nbsp;def test_add1(self):  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;a = Count(3,5)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;self.assertEqual(a.add(),8,msg='is not 8')  
&nbsp;&nbsp;&nbsp;&nbsp;def test_add2(self):  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;a = Count(4,6)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;self.assertEqual(a.add(),10,msg='is not 10')  
class TestSub(unittest.TestCase):  
&nbsp;&nbsp;&nbsp;&nbsp;def test_sub1(self):  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;a = Count(5,3)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;self.assertEqual(a.sub(),2,msg='is not 2')  
&nbsp;&nbsp;&nbsp;&nbsp;def test_sub2(self):  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;a = Count(8,4)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;self.assertEqual(a.sub(),4,msg='is not 4')  
if __name__ == '__main__':  
&nbsp;&nbsp;&nbsp;&nbsp;#构造测试集  
&nbsp;&nbsp;&nbsp;&nbsp;suite = unittest.TestSuite()  
&nbsp;&nbsp;&nbsp;&nbsp;suite.addTest(TestAdd("test_add1"))  
&nbsp;&nbsp;&nbsp;&nbsp;suite.addTest(TestAdd("test_add2"))  
&nbsp;&nbsp;&nbsp;&nbsp;suite.addTest(TestSub("test_sub1"))  
&nbsp;&nbsp;&nbsp;&nbsp;suite.addTest(TestSub("test_sub2"))  
&nbsp;&nbsp;&nbsp;&nbsp;#运行测试集合  
&nbsp;&nbsp;&nbsp;&nbsp;runner = unittest.TextTestRunner()  
&nbsp;&nbsp;&nbsp;&nbsp;runner.run(suite)  

## 7.1.5 discover更多测试用例  
一个实现几十个功能的项目，需要将这些用例按照测试的功能进行拆分，分散到不同的测试文件中  
对上面的test.py进行测试用例拆分，拆分后的目录结构如下:  
testpro/  
&nbsp;&nbsp;&nbsp;&nbsp;calculator.py  
&nbsp;&nbsp;&nbsp;&nbsp;testadd.py  
&nbsp;&nbsp;&nbsp;&nbsp;testsub.py  
&nbsp;&nbsp;&nbsp;&nbsp;runtest.py  

testadd.py  
> from calculator import Count  
import unittest  
class TestAdd(unittest.TestCase):  
&nbsp;&nbsp;&nbsp;&nbsp;def setUp(self):  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;print ("start add")  
&nbsp;&nbsp;&nbsp;&nbsp;def test_add1(self):  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;a = Count(3,5)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;self.assertEqual(a.add(),8,msg='is not 8')  
&nbsp;&nbsp;&nbsp;&nbsp;def test_add2(self):  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;a = Count(4,6)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;self.assertEqual(a.add(),10,msg='is not 10')  
&nbsp;&nbsp;&nbsp;&nbsp;def tearDown(self):  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;print ("end add")  
if __name__ == '__main__':  
&nbsp;&nbsp;&nbsp;&nbsp;unittest.main()  

testsub.py  
> from calculator import Count  
import unittest  
class TestSub(unittest.TestCase):  
&nbsp;&nbsp;&nbsp;&nbsp;def setUp(self):  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;print ("start sub")  
&nbsp;&nbsp;&nbsp;&nbsp;def test_sub1(self):  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;a = Count(5,3)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;self.assertEqual(a.sub(),2,msg='is not 2')  
&nbsp;&nbsp;&nbsp;&nbsp;def test_sub2(self):  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;a = Count(8,4)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;self.assertEqual(a.sub(),5,msg='is not 4')  
&nbsp;&nbsp;&nbsp;&nbsp;def tearDown(self):  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;print ("end sub")  
if __name__ == '__main__':  
&nbsp;&nbsp;&nbsp;&nbsp;unittest.main()  

runtest.py  
> import unittest  
import testadd  
import testsub  
if __name__ == '__main__':    
&nbsp;&nbsp;&nbsp;&nbsp;#构造测试集  
&nbsp;&nbsp;&nbsp;&nbsp;suite = unittest.TestSuite()  
&nbsp;&nbsp;&nbsp;&nbsp;suite.addTest(testadd.TestAdd("test_add1"))  
&nbsp;&nbsp;&nbsp;&nbsp;suite.addTest(testadd.TestAdd("test_add2"))  
&nbsp;&nbsp;&nbsp;&nbsp;suite.addTest(testsub.TestSub("test_sub1"))  
&nbsp;&nbsp;&nbsp;&nbsp;suite.addTest(testsub.TestSub("test_sub2"))  
&nbsp;&nbsp;&nbsp;&nbsp;#运行测试集合  
&nbsp;&nbsp;&nbsp;&nbsp;runner = unittest.TextTestRunner()  
&nbsp;&nbsp;&nbsp;&nbsp;runner.run(suite)  

上例中通过addTest()增加测试用例很麻烦，可以使用TestLoader类的discover()方法让unittest测试框架自动识别测试用例  
> discover(start_dir, pattern='test*.py',top_level_dir=None)
start_dir:要测试模块名或测试用例目录 ， 并可递归查到子目录下
pattern='test*.py' 表示用例文件名的匹配原则  
top_level_dir=None  测试模块的顶层目录，没有默认是None  

runtest1.py  
> import unittest  
#定义测试用例的目录为当前目录  
test_dir='./'  
discover = unittest.defaultTestLoader.discover(test_dir,pattern='test*.py')  
if __name__ == '__main__':  
&nbsp;&nbsp;&nbsp;&nbsp;runner = unittest.TextTestRunner()  
&nbsp;&nbsp;&nbsp;&nbsp;runner.run(discover)  
    
# 7.2 关于unittest还需要知道的  
## 7.2.1 用例的执行顺序  
unittest框架默认根据ASCII码的顺序加载测试用例、测试目录和测试文件，数字与字母的顺序为:0~9,A~Z,a~z  
默认的main()方法、discover()方法使用默认的顺序，如果想要修改测试顺序，需要通过TestSuite类的addTest()方法按照一定顺序来加载  
## 7.2.2 执行多级目录的用例




    
    



  
    
    


    
    




