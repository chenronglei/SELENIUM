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
    def __init__(self,a,b):  
	    self.a = int(a)  
	    self.b = int(b)  
    #计算加分  
    def add(self):  
        return self.a + self.b   
        
不用测试框架的单元测试:  
> from calculator import Count  
#测试两个正式相加  
class TestCount:  
    def test_add(self):
        #如果try内执行异常，except抛出自定义异常。如果try正常，执行else  
        try:  
            j = Count(2,3)
            add = j.add()  
            assert(add == 5),'Integer addition result error!'
        except AssertionError as msg:
            print (msg)
        else:
            print ('Test pass!')
#执行测试类的方法  
mytest=TestCount()  
mytest.test_add()  





