## gdb的使用

总结

```shell
g++ -g -O0 main.c -o main
gdb main
b (filename:)line #设置断点
bt #查看函数调用链
info b #查看断点信息
disable/enable/delete n #禁用/启用/删除 某个端点
c(ontinue) #继续往下运行
s(tep) n #往下执行n个步骤
n(ext) n #往下执行n条指令
q #退出
```



### 编译

```shell
g++ -g  -O0 main.c -0 main
```

### 启动

```shell
gdb main
或者
sudo gdb
attach pid #调试进程的信息
detach #退出调试
或者
gdb -p pid #和attach一样
detach #退出
```

### 运行程序

```shell
run、r
```

### set args

```shell
set args
如启动REDIS的哨兵服务器时，需要设置哨兵模式下的配置文件路径
set args /home/szza/redis-6.0.5/redis-6.0.5/sentinel.conf --sentinel #设置输入参数
r #运行
```

### 退出gdb

```shell
q、quit
```

### 断点

b、break

```shell
break #break后面没有任何参数，那么就是在当前栈帧的下一指令处加上断点
break line #在当前运行程序行处加断点，如果想在其他文件下加上端点 break filename:line
break function #在当前运行程序的function处加上断点
	c++会发生重载，甚至不同类存在同名函数，那么可以更加具体的设置：
	break filename:function #在filename文件夹下的function处加上断点
	break filename:function(ArgsType) #在filename文件夹的function（args）处加上断点
	break class:funtion #在类class的function处加上断点，函数可以加上具体参数
	
```

### break...if cond

如果只想满足某个条件时，才触发断点使用这个命令

如

```shell
break 7 if con>3
```

### info b

查看断点信息

### disable、enable、delete

```shell
disable n1 n2 n3 #临时关闭百年好位n1、n2、n3的的断点

enable n1 n2 n3 #开启被disable指令关闭的断点n1、n2、n3

delete n1 n2 n3 #直接删除断点n1、n2、n3
如果diable、enable、delete后面没有跟参数，那么就是关闭、开启、删除所有断点
```

### 执行流程

next

step

- continue，简写c

  恢复被break指令终端的程序，使其继续向下执行

- step，简写s

  step [count] 指令，逐步执行count个步骤，而不是count个语句、函数。当不屑count的时候，默认就执行一步

  step指令，用于配合break指令一起使用，当在某个函数起始处触发断点，想要进入该函数体，则可以使用step指令。而step count则是依次行执行count步，避免繁琐的中间行为，比如避免C++的构造函数

- next [count]，简写n

  next指令，是逐函数执行，即当停在断点触发的函数处：step是逐步执行，下一步进入函数体中；next指令会直接执行完整个函数，然后进入下一行。

### set step-mode

如果某个函数、语句没有包含debug信息，gdb默认就会跳过这个函数、语句。但是可以通过设置step-mode选项是否跳过。

```shell
set step-mode on #不跳过没有调试信息的函数、语句
set step-mode off #默认行为，跳过
#可以通过show step-mode来查看
```

### finish，简写fin

用于将当前函数剩下的部分执行完毕，并且显示输出结果

可以控制finish显示和不显示返回结果：

``` shell
set print finish [on|off] #控制finish返回结果是否显示
show print finfish #输出finish的返回结果是否显示
```

### return

finish是把剩余的执行完，return是直接在函数的当前位置返回，不管执行到什么位置

### until，简写u

可以用于直接跳出循环体

until指令，不加参数，没有遇到循环体时功能类似于next，遇到了可以直接跳出循环体

until location，和break location格式一样，可以直接运行到指定行数

### 显示

print：手动输出

display：自动显示

- print [[options] --] expr：其中expr可以是表达式、变量。其中输出的变量要么是全局白能量、staic变量、局部变量。

- print [[options] --] /f expr

  /f是expr的输出格式

  x 按十六进制格式显示

  a 按十六进制格式显示变量值

  d 十进制

  u 十六进制格式显示无符号整数

  o 八进制

  t 二进制

  c 按字符格式显示变量

  f 按浮点数格式显示变量

  s 按字符串显示

  z 与x一样，但是前导0被打印出来

  r 'r'是“raw”的缩写，按照python的Pretty-printer风格进行打印

  $pc代表当前指令的地址

- display /f expr与print /f expr格式基本一致，但是运行每一条语句都会显示expr的值

- display /f addr

  当自动显示的是地址显示，可以使用/i格式描述符，查看地址addr的汇编代码