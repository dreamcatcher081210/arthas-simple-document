时光如梭，写代码的时间到现在应该有五年了，其中C#半年，全栈三年，纯后端开发一年，目前在用java作为主要的编程语言，Spring桶 + MyBatis + MySQL套餐。
这几年做过许多项目，上线之后进行日常维护，其中碰到的最多的问题就是诸如：内存瞬间飙满，出现异常信息等等情况，在测试环境由于数据非线上数据库数据，所以有时很难重现此bug，大多都是靠经验和看代码推敲。不仅耗时，而且排除bug的时间也是不确定的，
自从使用了阿里的arthas后，发现这是一个神器啊，初步尝试用了几下，还不错，打算以后就靠这个维持生活了，哈哈。写个简易版文档，供自己使用。

### 今天是2019-09-30 明天就是建国70周年了，各界人士也纷纷表达了对祖国最美好的祝福，台湾人民请鼓掌～，迟早你们领二代身份证。我的战友明天参加阅兵式，将在天安门前，接受祖国和人民的检阅，真心为他们感到骄傲和自豪。

### 快速安装
使用arthas-boot（推荐）
下载arthas-boot.jar，然后用java -jar的方式启动：

`wget https://alibaba.github.io/arthas/arthas-boot.jar`

`java -jar arthas-boot.jar`

### 进阶使用
#### 基础命令
help——查看命令帮助信息

cat——打印文件内容，和linux里的cat命令类似

pwd——返回当前的工作目录，和linux命令类似

cls——清空当前屏幕区域

session——查看当前会话的信息

reset——重置增强类，将被 Arthas 增强过的类全部还原，Arthas 服务端关闭时会重置所有增强过的类

version——输出当前目标 Java 进程所加载的 Arthas 版本号

history——打印命令历史

quit——退出当前 Arthas 客户端，其他 Arthas 客户端不受影响

shutdown——关闭 Arthas 服务端，所有 Arthas 客户端全部退出

keymap——Arthas快捷键列表及自定义快捷键

#### jvm相关
dashboard——当前系统的实时数据面板

thread——查看当前 JVM 的线程堆栈信息

jvm——查看当前 JVM 的信息

sysprop——查看和修改JVM的系统属性

sysenv——查看JVM的环境变量

vmoption——查看和修改JVM里诊断相关的option

logger——查看和修改logger

getstatic——查看类的静态属性

ognl——执行ognl表达式

mbean——查看 Mbean 的信息

heapdump——dump java heap, 类似jmap命令的heap dump功能

#### class/classloader相关
sc——查看JVM已加载的类信息

sm——查看已加载类的方法信息

jad——反编译指定已加载类的源码

mc——内存编绎器，内存编绎.java文件为.class文件

redefine——加载外部的.class文件，redefine到JVM里

dump——dump 已加载类的 byte code 到特定目录

classloader——查看classloader的继承树，urls，类加载信息，使用classloader去getResource

#### monitor/watch/trace相关
请注意，这些命令，都通过字节码增强技术来实现的，会在指定类的方法中插入一些切面来实现数据统计和观测，因此在线上、预发使用时，请尽量明确需要观测的类、方法以及条件，诊断结束要执行 shutdown 或将增强过的类执行 reset 命令。

monitor——方法执行监控

watch——方法执行数据观测

trace——方法内部调用路径，并输出方法路径上的每个节点上耗时

stack——输出当前方法被调用的调用路径

tt——方法执行数据的时空隧道，记录下指定方法每次调用的入参和返回信息，并能对这些不同的时间下调用进行观测

#### options
options——查看或设置Arthas全局开关

#### 管道
Arthas支持使用管道对上述命令的结果进行进一步的处理，如sm java.lang.String * | grep 'index'

grep——搜索满足条件的结果

plaintext——将命令的结果去除ANSI颜色

wc——按行统计输出结果

#### 后台异步任务
当线上出现偶发的问题，比如需要watch某个条件，而这个条件一天可能才会出现一次时，异步后台任务就派上用场了，详情请参考这里

使用 > 将结果重写向到日志文件，使用 & 指定命令是后台运行，session断开不影响任务执行（生命周期默认为1天）

jobs——列出所有job

kill——强制终止任务

fg——将暂停的任务拉到前台执行

bg——将暂停的任务放到后台执行

#### Web Console
通过websocket连接Arthas。

Web Console