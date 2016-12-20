&1.from collections import defaultdict
返回一个new dictionary-like对像，它是内置函数dict的子实现类。它重写了一个方法并且增加了一个可写实例变量，它仍然具有dict一样的功能。
###用法###
实例化时需要传一个callable或None值，当传入不带value值时，默认返回实例化的初始值

###例子###
---------------------
class Exchange:
    def __init__(self):
        self._subscribers = set()

    def attach(self, task):
        self._subscribers.add(task)

    def detach(self, task):
        self._subscribers.remove(task)

    def send(self, msg):
        for subscriber in self._subscribers:
            subscriber.send(msg)
  -----------------------------------------
  
  a=defaultdict(Exchange)
  adict = a["name"] # 返回Exchange类对象的内存地址。
  print a   #defaultdict(<class __main__.Exchange at 0x00000000029D4B28>, {'name': <__main__.Exchange instance at 0x00000000029FD288>})
  
  
