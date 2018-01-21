WebDriver属于selenium中设计出来操作浏览器的一套API  
站在WebDriver角度，它支持多种编程语言；站在编程语言角度,WebDriver是Python实现web自动化的第三方库
# 4.1从定位元素开始
WebDriver提供八种元素定位方法，Python语言中，对应的方法如下:  
- id              find_element_by_id()  
- name            find_element_by_name()  
- class name      find_element_by_class_name()  
- tag name        find_element_by_tag_name()  
- link test       find_element_by_link_test()  
- partial link test find_element_by_partial_link_test()  
- xpath           find_element_by_xpath()  
- css selector    find_element_by_css_selector()  


**firefox浏览器使用54.0版本安装Firebug,firepath插件，新版本的Firefox浏览器不支持Firebug,firepath**  
## 4.1.1 id定位
id在html是唯一的  
[定位方法](https://jingyan.baidu.com/article/19192ad81ab005e53e5707d6.html)  
find_element_by_id("kw")  百度输入框   
find_element_by_id("su")  百度搜索按钮  
## 4.1.2 name定位
name在html可不唯一  
定位方法同4.1.1  
find_element_by_name("wd") 百度输入框  
百度搜索框没有提供搜索按钮
## 4.1.3 class定位
定位方法同4.1.1  
class="s_ipt"  
class="btn self-btn bg s_btn"  
find_element_by_class_name("s_ipt")  百度输入框  
find_element_by_class_name("s_btn")  百度搜索按钮,不能有空格，能唯一标识就行
## 4.1.4 tag定位
一个tag往往用来定义一类功能，比如input、div等，通过tag识别元素的概率很低  
## 4.1.5 link定位  
专门用来定位文本链接  
&lt;a class="mnav" href="http://news.baidu.com" target="_blank">新闻&lt;/a>  
find_element_by_link_test("新闻")  通过元素标签对之间的文本信息来定位元素
## 4.1.6 partial link定位 
partial link是对link的一个补充。有时候文本链接会比较长，可以取一部分定位来唯一标识这个链接  

Xpath和CSS定位，提供了灵活的定位，可以通过不同的方式定位到想要的元素  
## 4.1.7 XPath定位 
XPath是一种在xml文档中定位元素的语言。因为HTML可以看成xml的一种实现。所有selenium可以使用这种强大的语言在WEB应用中定位元素  
**绝对路径定位**  
一级一级往下查找，div[7]表示当前层级下的第7个div标签
> driver.find_element_by_xpath("/html/body/div[7]/div/div/div/strong/a").click()  

**利用元素属性定位**  
下图中第一行中中 // 表示当前页面某个目录下，input表示定位元素的标签名，[@id='kw']表示这个元素的id属性值为kw
> driver.find_element_by_xpath("//input[@id='kw']").send_keys("Python 38")  
driver.find_element_by_xpath("//input[@value='百度一下']").click()  

如果不想指定标签名，标签名可以用\*代替,且元素的任意属性都可以用来定位，只要能唯一标识元素  
**层级和属性结合**  
如果一个元素本身没有可以唯一标识这个元素的属性值，可以找其上一级元素，如果其上一级元素有可唯一标识元素的属性值，可以拿来使用；如果上一级也没有，继续向上级查找  
> &lt;div class="searchBtn">  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;button id="searchBtn" type="submit">搜狗&lt;/button>  

假定qq搜索按钮没有唯一标识的属性值，使用它的上一级属性来定位  
> driver.find_element_by_xpath("//div[@class='searchBtn']/button").click()  

**使用逻辑运算符**  
如果一个属性不能唯一地区分一个元素，我们使用逻辑运算符连接多个属性来定位一个元素  
> driver.find_element_by_xpath("//input[@id='kw' and @class='su']/span/input"

使用firepath插件能方便的找到xpath语法:选择元素后，直接在xpath框中生成xpath语法  
![](https://github.com/crl608/SELENIUM/blob/master/firepath.png)  
> driver.find_element_by_xpath("//*[@id='searchBtn']").click()  

## 4.1.8 CSS定位
CSS语法更加简洁，XPATH学习更简单。只要掌握一种就可以解决大部分的定位问题  

## 4.1.9 用By定位元素  
使用之前需要导入类  
from selenium.webdriver.common.by import By  
> find.element(By.ID,"kw")  
find.element(By.NAME,"wd")  
...  

find.element()与find_element_by_xxx 底层实现是一致的，推荐使用前面介绍的find_element_by_xxx方法  
# 4.2控制浏览器
## 4.2.1控制浏览器窗口大小  
浏览器以某种尺寸打开  
> driver.set_window_size(1480,600)  

浏览器以全屏打开  
driver.maximize_window()  

 
## 4.2.2控制浏览器后退、前进  
driver.back()  
driver.forward()

## 4.2.3模拟浏览器刷新
driver.refresh()
# 4.3简单元素操作
Webdriver最常用的几个方法:  
- clear() 清除文本  
- send_keys(\*value) 模拟按键输入 
- click()      单击元素  

## 4.3.1 登陆minieye邮箱
> \# coding=utf-8  
import time  
from selenium import webdriver  
driver = webdriver.Firefox()  
driver.get("https://exmail.qq.com/cgi-bin/loginpage")  
time.sleep(2)   
driver.find_element_by_xpath("//*[@id='inputuin']").clear()  
driver.find_element_by_xpath("//*[@id='inputuin']").send_keys("chenronglei@minieye.cc")  
time.sleep(2)  
driver.find_element_by_xpath("//*[@id='pp']").clear()  
driver.find_element_by_xpath("//*[@id='pp']").send_keys("Root_123")  
time.sleep(2)  
driver.find_element_by_xpath("//*[@id='btlogin']").click()  
time.sleep(3)   
driver.quit()  

clear()可以单击任何可以单击的对象
## 4.3.2 WebElement接口常用方法
包括4.1节的8种定位方法及上面的3种方法都是；以下还有一些常用的：  
- submit() 有时候submint()可以和click()互换，但应用范围远不及click()广泛  
- size     获得元素尺寸  
- text     获得元素文本  
&lt;label class="hint">请填写企业邮箱的完整帐号，或管理员帐号。&lt;/label>
- get_attribute(name)  返回元素的属性值  
- id_displayed()       返回元素的值是否可见  
# 4.4鼠标事件
WebDriver中，将这些鼠标操作的方法封装在ActionChains类提供  
ActionChains类提供了鼠标操作的常用方法:  
- perform() 执行所有ActionChains中存储的行为
- content_click()  右击  
- double_click()   双击  
- drag_and_drop()  拖动  
- move_to_element() 鼠标悬停  

## 1.鼠标右击操作
> from selenium import webdriver  
from selenium.webdriver.common.action_chains import ActionChains   //导入提供鼠标操作的ActionChains类  
right_click=driver.find_element_by_xpath(".//*[@id='navBeta']/div[1]/div[1]/strong/a")  
ActionChains(driver).context_click(right_click).perform()  

ActionChains(driver) 调用ActionChains()类，将浏览器驱动driver作为参数传入  
context_click()方法拥有模拟鼠标右键动作，需要指定元素定位  
perform()  整个操作的提交动作

## 2.鼠标悬停  
> click=driver.find_element_by_xpath("//*[@id='navMore']/span")  
ActionChains(driver).move_to_element(click).perform()  

## 3.鼠标双击操作
> click=driver.find_element_by_xpath("//*[@id='navBeta']/div[1]/div[3]/strong/a")  
ActionChains(driver).double_click(click).perform()

## 4. 鼠标拖放操作
> driver = webdriver.Firefox()  
driver.get("http://sahitest.com/demo/dragDropMooTools.htm")  
click1=driver.find_element_by_xpath("//*[@id='dragger']")  
click2=driver.find_element_by_xpath("html/body/div[2]")  
ActionChains(driver).drag_and_drop(click1,click2).perform()  
driver.quit()  

# 4.5键盘事件
在使用键盘按键方法之前需要先导入keys类  
> from selenium.webdriver.common.keys import Keys  

常用的键盘操作:  
send_keys(Keys.BACK_SPACE)      删除键  
send_keys(Keys.SPACE)           空格键  
send_keys(Keys.TAB)             制表键
send_keys(Keys.ESCAPE)          ESC  
send_keys(Keys.ENTER)           Enter  
send_keys(Keys.CONTROL,'a')     Ctrl+A  
send_keys(Keys.CONTROL,'c')     Ctrl+c  
send_keys(Keys.CONTROL,'x')     Ctrl+x  
send_keys(Keys.CONTROL,'v')     Ctrl+v  
send_keys(Keys.F1)              F1
...  
send_keys(Keys.F12)             键盘F12  

# 4.6获得验证信息  
可以从页面获取一些信息验证用例执行是否成功  ,用的最多的几种验证信息title,URL,text  
> from selenium import webdriver  
import time  
driver=webdriver.Firefox()  
driver.get("https://exmail.qq.com/cgi-bin/loginpage")  
print ('before')  
title=driver.title  
print (title)  
url=driver.current_url  
print (url)  
driver.find_element_by_xpath("//*[@id='inputuin']").send_keys("chenronglei@minieye.cc")  
driver.find_element_by_xpath("//*[@id='pp']").send_keys("Root_123")  
driver.find_element_by_xpath("//*[@id='btlogin']").click()  
time.sleep(5)  
print ('after')  
title=driver.title  
print (title)  
url=driver.current_url  
print (url)  
text = driver.find_element_by_xpath("//*[@id='useraddr']").text  
print (text)  

title,current_url,text分别获取当前页面的标题、URL和指定元素的文本。使用登陆后的这些信息进行验证

# 4.7设置元素等待  
大多数web应用程序使用AJAX技术，当浏览器在加载页面元素时，页面的元素可能不是同时被加载完成的。存在因为加载某个元素延迟而造成ElementNotVisibleException的情况出现，降低脚本的稳定性。  
可以设置元素等待来改善这种问题导致的不稳定  
**WebDriver提供两种类型的等待：显示等待和隐式等待**  
## 4.7.1显示等待  
显示等待使WebDriver等待某个条件成立时继续执行，否则达到最大时长抛出超时异常TimeoutException  
> from selenium import webdriver  
from selenium.webdriver.common.by import By  
from selenium.webdriver.support.ui import WebDriverWait  
from selenium.webdriver.support import expected_conditions as EC
driver=webdriver.Firefox()  
driver.get("http://www.baidu.com")  
element = WebDriverWait(driver,5,0.5).until(EC.presence_of_element_located((By.ID,"kw")))  
element.send_keys('selenium')
driver.quit()  

每隔0.5秒检测一次是否加载了id为kw的元素，最多检测5s,否则报超时错误  

使用is_displayed的方法同样可以实现，且更好理解
> from selenium import webdriver  
import time  
driver=webdriver.Firefox()  
driver.get("http://www.baidu.com")  
print (time.ctime())  
for i in range(10):     
&nbsp;&nbsp;&nbsp;&nbsp;try:          
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;if driver.find_element_by_id("kw12").is_displayed():  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;break  
&nbsp;&nbsp;&nbsp;&nbsp;except:  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;pass  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;time.sleep(1)  
else:  
&nbsp;&nbsp;&nbsp;&nbsp;print ('time out')  
print (time.ctime())    #打出当前系统时间  
driver.quit()  

## 4.7.2隐式等待  
隐式等待式通过一定的时长等待页面上某元素加载完成，如果超出了设置的时长元素还没有被加载，则抛出NoSuchElementException异常  
WebDriver提供implictly_wait()方式实现隐式等待，默认设置为0  
> from selenium import webdriver  
import time  
from selenium.common.exceptions import NoSuchElementException  
driver=webdriver.Firefox()  
driver.implicitly_wait(10)  
driver.get("http://www.baidu.com")  
try:  
&nbsp;&nbsp;&nbsp;&nbsp;print (time.ctime())  
&nbsp;&nbsp;&nbsp;&nbsp;driver.find_element_by_id("kw12")  
except NoSuchElementException as e:  
&nbsp;&nbsp;&nbsp;&nbsp;print (e)  
finally:  
&nbsp;&nbsp;&nbsp;&nbsp;print(time.ctime())  
&nbsp;&nbsp;&nbsp;&nbsp;driver.quit()  



上例中implicitly_wait设置为10s,但它并不影响脚本执行的速度；其次，implicitly_wait并不针对某个元素进行等待。  
当脚本执行到某个元素，如果可以定位，则继续执行；如果不能定位，则以轮询的方式不断判断元素是否被定位到。超出设置时长还没定位到，抛出异常  
## 4.7.3  sleep休眠方法  
脚本执行到某个位置做固定时间的休眠  
time.sleep(5)              休眠5s
time.sleep(0.5)            休眠0.5s
