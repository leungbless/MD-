# Python写简单的ZIP压缩包破解器

**感谢DeelMind**

```python
# crack zip password
# by DeeLMind QQ Group Live
# 2020/02/29

import zipfile #也可以压缩压缩包，查看压缩包的信息
import random

class ZipCrack:
    def __init__(self,ZipFile,PassWords):
        self.__ZipFile = ZipFile
        self.__PassWords = PassWords
        print("Zip Crack Password")

    def Crack(self):  #传入的self代表这个类本身(类的首地址)
        #创建一个ZipFile对象即zip文件(r：读已经存在的zip文件；w：新建或覆盖一个zip文件)
        zfile = zipfile.ZipFile(self.__ZipFile,'r')
        for password in self.__PassWords:
            try:
                #解压压缩包，路径在当前文件夹下，密码为utf-8的字符串
                zfile.extractall(path='.', pwd=str(password).encode('utf-8'))
                print("ZipCrack Password 
                      ----Successful---- => {}".format(str(password)))
                return
            except Exception as e:
                print("ZipCrack Password Failed => {}".format(str(password)))


def test1():
    passwords = [996,1111,2222,3333,9961,1234]
    cz = ZipCrack('ip.zip',passwords)  #实例化一个类，并且传入参数
    cz.Crack()  #调用类的方法

def test2():
    cz = ZipCrack('ip.zip',range(1000,9999)) #实例化一个类，并且传入参数
    cz.Crack()  #调用类的方法

if __name__ == '__main__':
    test2()
```

​		

​		两个下划线开头的函数是声明该属性为私有，不能在类的外部被使用或访问。而__init__函数（方法）支持带参数类的初始化，也可为声明该类的属性（类中的变量）。__init__函数（方法）的第一个参数必须为self，后续参数为自己定义。__init__()方法又被称为构造器（constructor）。

```python
# 说明构造器的例子
class Box:
    def __init__(self, width, height, depth):
        self.width = width
        self.height = height
        self.depth = depth
 
    def getVolume(self):
        return self.width * self.height * self.depth
 
b = Box(10, 20, 30) #相当于CPP里面的构造函数，在初始化时就设置
print(b.getVolume())
```

