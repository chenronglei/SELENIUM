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
