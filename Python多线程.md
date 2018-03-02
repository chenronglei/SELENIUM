Selenium Grid虽然可以分布式执行测试用例，但它并不支持并行。分布式只负责将一个测试用例远程到用到不同的环境下执行；而并行强调“同时”执行多个任务。可利用编程语言提供的
多线程（或多进程）技术来实现并行  
**进程**：进程是程序的一次执行，每个紧张都有自己的地址空间、内存、数据栈，以及其他记录其运行轨迹的辅助数据。操作系统管理在其上面运行的所有进程，并为这些进程公平地分配时间  
**线程**: 线程（有时被称为轻量级进程）与进程有些相似。不同的是，所有的线程都运行在同一个进程中，共享相同的运行环境。
# 10.1单线程的时代  
在单线程的时代，当处理器需要处理多个任务时，必须对这些任务安排执行顺序，并按照这个顺序来执行任务  
> from time import sleep,ctime
#听音乐任务
def music():  
&nbsp;&nbsp;&nbsp;&nbsp;print ("I was listening to music! %s" % ctime())  
&nbsp;&nbsp;&nbsp;&nbsp;sleep(2)  
#看电影任务  
def movie():  
&nbsp;&nbsp;&nbsp;&nbsp;print ("I was at the movies！ %s" % ctime())  
&nbsp;&nbsp;&nbsp;&nbsp;sleep(5)  
if __name__ == '__main__':  
&nbsp;&nbsp;&nbsp;&nbsp;music()  
&nbsp;&nbsp;&nbsp;&nbsp;movie()  
&nbsp;&nbsp;&nbsp;&nbsp;print ('all end:',ctime())  

上例中:分别创建了两个任务music和movie，执行时间分别为2秒和5秒，通过sleep()方法设置休眠时间来模拟任务的运行时间,上面例子运行总耗时7s  
修改播放器能提供循环播放的功能，改造后的程序如下:
> from time import sleep,ctime  
#听音乐任务  
def music(func,loop):  
&nbsp;&nbsp;&nbsp;&nbsp;for i in range(loop):  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;print ("I was listening to %s! %s" % (func,ctime()))  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;sleep(2)  
#看电影任务  
def movie(func,loop):  
&nbsp;&nbsp;&nbsp;&nbsp;for i in range(loop):  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;print ("I was at the %s！ %s" % (func,ctime()))  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;sleep(5)  
if __name__ == '__main__':  
&nbsp;&nbsp;&nbsp;&nbsp;music('爱情买卖',2)  
&nbsp;&nbsp;&nbsp;&nbsp;movie('tree',2)  
&nbsp;&nbsp;&nbsp;&nbsp;print ('all end:',ctime())  
    
程序运行总耗时14s      
# 10.2多线程技术  
Python通过两个标准库thread和threading提供对线程的支持.  
thread提供了低级别的、原始的线程以及一个简单的锁。
threading基于Java的线程模型设计。锁和条件变量在Java中时对象的基本行为（每个对象都自带了锁了条件变量），而在Python中则是独立的对象  
## 10.2.1 threading模块  
我们应该避免使用thread模块，因为它不支持守护线程。当主线程退出时，所有的子线程不管它们是否还在工作，都会别强行退出。有时候我们不希望发生这种行为，这时就引入守护线程的概念  
threading模块支持守护线程  

> from time import sleep,ctime  
import threading  
#听音乐任务  
def music(func,loop):  
&nbsp;&nbsp;&nbsp;&nbsp;for i in range(loop):  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;print ("I was listening to %s! %s" % (func,ctime()))  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;sleep(2)  
#看电影任务  
def movie(func,loop):  
&nbsp;&nbsp;&nbsp;&nbsp;for i in range(loop):  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;print ("I was at the %s！ %s" % (func,ctime()))  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;sleep(5)  
#创建线程数组  
threads = []  
#创建线程t1，并添加到线程数组  
t1 = threading.Thread(target=music,args=('爱情买卖',2))  
threads.append(t1)  
#创建线程t2,并添加到线程数组  
t2 = threading.Thread(target=movie,args=('tree',2))  
threads.append(t2)   
if __name__ == '__main__':  
&nbsp;&nbsp;&nbsp;&nbsp;#启动线程  
&nbsp;&nbsp;&nbsp;&nbsp;for t in threads:  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;t.start()  
&nbsp;&nbsp;&nbsp;&nbsp;#守护线程,join()等待线程终止，入股没有守护进程，可能在线程运行时就打印 all end语句  
&nbsp;&nbsp;&nbsp;&nbsp;for t in threads:  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;t.join()  
&nbsp;&nbsp;&nbsp;&nbsp;print('all end: %s' % ctime())  

上例执行结果是两个子线程music、movie同时执行，总耗时10S,两个线程达到了并行工作  

## 10.2.2 优化线程的创建  
从上面的例子发现线程的创建时颇为麻烦的，每创建一个线程都需要创建一个t(t1、t2...)。下面的例子进行改进:  

> from time import sleep,ctime  
import threading  
#创建超级播放器  
def super_player(file_,time):  
&nbsp;&nbsp;&nbsp;&nbsp;for i in range(2):  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;print ("Start playing: %s! %s" % (file_,ctime()))  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;sleep(time)  
#播放的文件与播放时长  
lists = {'爱情买卖.mp3':3,'阿凡达.mp4':5,'tree.mp4':4}  
#创建线程数组  
threads = []  
files = range(len(lists))  
#创建线程  
for file_,time in lists.items():  
&nbsp;&nbsp;&nbsp;&nbsp;t = threading.Thread(target=super_player,args=(file_,time))  
&nbsp;&nbsp;&nbsp;&nbsp;threads.append(t)  
if __name__ == '__main__':  
&nbsp;&nbsp;&nbsp;&nbsp;#启动线程  
&nbsp;&nbsp;&nbsp;&nbsp;for t in files:  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;threads[t].start()  
&nbsp;&nbsp;&nbsp;&nbsp;#守护线程  
&nbsp;&nbsp;&nbsp;&nbsp;for t in threads:  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;t.join()  
&nbsp;&nbsp;&nbsp;&nbsp;print('all end: %s' % ctime())  

## 10.2.3 创建线程类  
除直接使用Python所提供的线程类外，我们还可以根据需求自定义自己的线程类
    
    
    






    
    
