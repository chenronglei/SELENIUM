**作为基于Firefox浏览器的插件，Selenium IDE结合浏览器提供了脚本的录制、回放以及编辑脚本的功能**  
通过 【菜单】-【开发者】-【Selenium IDE】打开Selenium IDE  
# Selenium IDE命令  
Selenium IDE的Command的下拉框中可以选择使用这些命令  
- open  在浏览器中打开url，可以接收相当路径和绝对路径两种格式  
- click  
单击链接、按钮、复选和单选框  
如果但几乎需要等待响应，则用"clickAndWait"  
如果需要经过JavaScript的alert或confirm对话框才能继续操作，则需要调用verify或assert来告诉Selenium你期望对对话框进行什么操作  
- type 
模拟键盘的输入，向指定的input中输入值  
也适合个给复选框和单选框赋值
tpyeAndWait,等待响应  
- select 根据optionSpecifier选项选择权来选择一个下拉菜单选项  
- goBack 模拟单击浏览器的后退按钮  
- selectWindow 选择一个弹出窗口，当选择哪个窗口时，所有命令将会转移到被选择窗口中执行  
- pause(millisenconds) 根据指定时间暂停Selenium脚本执行  
- fireEvent 模拟页面元素事件被激活的处理动作  
- close 模拟单击浏览器关闭按钮


