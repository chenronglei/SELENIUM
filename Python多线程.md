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
> from time import sleep,ctime  
import threading  
#创建线程类  
class MyThread(threading.Thread):  
&nbsp;&nbsp;&nbsp;&nbsp;def __init__(self,func,args,name=''):  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;threading.Thread.__init__(self)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;self.func = func  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;self.args =args  
&nbsp;&nbsp;&nbsp;&nbsp;self.name = name  
&nbsp;&nbsp;&nbsp;&nbsp;def run(self):  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;self.func(*self.args)  
#创建超级播放器  
def super_play(file_,time):  
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
&nbsp;&nbsp;&nbsp;&nbsp;t = MyThread(super_play,(file_,time),super_play.__name__)  
&nbsp;&nbsp;&nbsp;&nbsp;threads.append(t)  
if __name__ == '__main__':  
&nbsp;&nbsp;&nbsp;&nbsp;#启动线程  
&nbsp;&nbsp;&nbsp;&nbsp;for t in files:  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;threads[t].start()  
&nbsp;&nbsp;&nbsp;&nbsp;#守护线程  
&nbsp;&nbsp;&nbsp;&nbsp;for t in threads:  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;t.join()  
&nbsp;&nbsp;&nbsp;&nbsp;print('all end: %s' % ctime())  

# 10.3多进程技术  
## 10.3.1 multiprocessing模块  
多进程multiprocessing模块的使用与多线程threading模块的用法类似。multiprocessing提供了本地和远程的并发性，有效的通过全局解释锁GIL来使用进程（而
不是线程）  
由于GIL的存在，在CPU密集型的程序当中，使用多线程并不能有效地利用多核CPU的优势，因为一个解释锁在同一时刻只会有一个线程在执行。每个进程有各自独立的GIL，互不干扰，所以mutiprocessing模块可以充分利用硬件的多处理器来进行工作。多核下，想做并行提升效率，比较通用的方法是使用多进程，能够有效提高执行效率，它支持UNIX和windows系统上运行  
修改多线程的例子，将threading模块中的Thread方法替换为multiprocessing模块的Process就实现了多进程  
> from time import sleep,ctime  
import multiprocessing  
#创建超级播放器  
def super_player(file_,time):  
&nbsp;&nbsp;&nbsp;&nbsp;for i in range(2):  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;print("Start playing: %s! %s" % (file_,ctime()))  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;sleep(time)  
#播放的文件与播放时长  
lists = {'爱情买卖.mp3':3,'阿凡达.mp4':5,'tree.mp4':4}  
#创建进程数组  
threads = []  
files = range(len(lists))  
#创建进程  
for file_,time in lists.items():  
&nbsp;&nbsp;&nbsp;&nbsp;t = multiprocessing.Process(target=super_player,args=(file_,time))  
&nbsp;&nbsp;&nbsp;&nbsp;threads.append(t)  
if __name__ == '__main__':  
&nbsp;&nbsp;&nbsp;&nbsp;#启动进程  
&nbsp;&nbsp;&nbsp;&nbsp;print('start:%s' %ctime())  
&nbsp;&nbsp;&nbsp;&nbsp;for t in files:  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;threads[t].start()  
&nbsp;&nbsp;&nbsp;&nbsp;#守护进程  
&nbsp;&nbsp;&nbsp;&nbsp;for t in files:  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;threads[t].join()  
&nbsp;&nbsp;&nbsp;&nbsp;print('end: %s' % ctime())  

在linux系统上执行的结果与多线程一致，在windows上，由于不支持fork，子进程的内容不能打印  
**扩展**:linux上创建新的进程使用的是fork。 fork()函数通过系统调用创建一个 原来进程几乎完全相同的进程，但如果初始参数或传入的变量不同，则两个进程也可以做
不同的事;这意味着子进程开始执行的时候具有与父进程相同的全部内容，这是我们讨论基于继承对象共享的基础。所谓基于继承的对象共享，是指在创建子进程之前由父进程初始化的某些对象可以在子进程当中直接访问到  
在Windows平台上，由于没有fork语义的系统调用，所以基于继承的共享对象比unix\liunx有更多的限制  
但是，并不是所有的对象都可以通过继承来共享，只有multiprocessing库当中的某些对象才可以。例如Queue、同步对象、共享变量、Manager等  
在一个multiprocessing库的典型使用场景下，所有的子进程都是由一个父进程启动起来的，这个父进程称为master进程。  
## 10.3.2 Pipe和Queue  
multiprocessing提供了threading包中没有的IPC(进程间通信),效率上更高。应优先考虑Pipe和Queue  
multiprocessing包中由Pipe类和Queue类来分别支持这两种IPC机制。Pipe和Queue可以用来传送常见的对象  
- Pipe可以是单向也可以是双向。我们通过multiprocessing.Pipe(duplex=False)创建单向管道（默认为双向）。一个进程从pipe一段输入对象，然后被pipe另一端的进程接收。单向管道只允许管道一段的进程输入，而双向管道则允许从两端输入  


    

    
    
    
    
    
    






    
    
