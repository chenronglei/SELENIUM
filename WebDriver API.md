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

