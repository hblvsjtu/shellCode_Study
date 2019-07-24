# shellCode_Study

## 作者：冰红茶  
## 参考书籍：[菜鸟教程](https://www.runoob.com/linux/linux-shell.html)
    
------    
    
在业务中经常会碰到需要shell编程的情况，想一次性学习shell编程的内容，相信对以后写一些自动化脚本会非常有帮助^_ ^

## 目录
## [一、简介](#1)
### [1.1 运行方式](#1.1)
### [1.2 变量](#1.2)
### [1.3 字符串](#1.3)
### [1.4 外部传入参数](#1.4)
### [1.5 数组](#1.5)
### [1.6 运算符](#1.6)
## [二、命令](#2)
### [2.1 echo](#2.1)
### [2.2 printf](#2.2)
### [2.3 test](#2.3)

        
------      
        
<h2 id='1'>一、简介</h2>
<h3 id='1.1'>1.1 运行方式</h3>

        
#### 1) 作为可执行程序直接运行
> - 需要先给程序可执行的权限，
                
                chmod +x ./test.sh  #使脚本具有执行权限
                # chmod 755 ./test.sh
                ./test.sh  #执行脚本
#### 2) 使用编译器运行
> - 如
                
                sh test.sh


<h3 id='1.2'>1.2 变量</h3>

#### 1) 定义变量
> - 定义变量时，变量名不加美元符号
> - 变量名和等号之间不能有空格
#### 2) 使用变量
> - 使用一个定义过的变量，只要在变量名前面加美元符号即可
> - 也可以加上花括号，也可以不加，加上花括号有利于编译器辩识变量的边界
                
                appledeMacBook-Pro:shellCode_Study lvhongbin$ myName="lvhongbin"
                appledeMacBook-Pro:shellCode_Study lvhongbin$ echo $myName
                lvhongbin
                appledeMacBook-Pro:shellCode_Study lvhongbin$ echo "My name is ${myName}"
                My name is lvhongbin
#### 3) 只读变量
> - readonly
#### 4) 删除变量
> - unset
> - unset 命令不能删除只读变量


<h3 id='1.3'>1.3 字符串</h3>

#### 1) 引号
> - 字符串可以用单引号，也可以用双引号
> - 单引号里的任何字符都会原样输出，单引号字符串中的变量是无效的；
#### 2) 获取字符串长度
> - 变量左花括号后的首字符添加井号"#"
                
                appledeMacBook-Pro:shellCode_Study lvhongbin$ myName="lvhongbin"
                appledeMacBook-Pro:shellCode_Study lvhongbin$ echo ${#myName}

#### 3) 截取字符串
> - 两个冒号
> - 左闭右开
                
                appledeMacBook-Pro:shellCode_Study lvhongbin$ myName="lvhongbin"
                appledeMacBook-Pro:shellCode_Study lvhongbin$ echo ${myName:0:2}
                lv
#### 4) 字符串比较
参数类型 | 参数说明
-|-
=  | 相等
!=  | 不相等
-z  | 字符串长度为0时为真
-n  | 字符串长度不为0时为真

<h3 id='1.4'>1.4 外部传入参数 </h3>

#### 1) $n
> - 向脚本传递参数，脚本内获取参数的格式为：$n
> - $0表示 要执行的第一个命令(不包括sh或者bash)
> - $1表示 命令后的第一个参数
> - $2,$3如此类推
                
#### 2) 参数说明

参数类型 | 参数说明
-|-
$#  | 参数的个数
$*  | 以一个单字符串显示所有向脚本传递的参数。如"$*"用「"」括起来的情况、以"$1 $2 … $n"的形式输出所有参数。
$$  | 脚本运行的当前进程ID号
$!  | 后台运行的最后一个进程的ID号
$@  | 与$*相同，但是使用时加引号，并在引号中返回每个参数。如"$@"用「"」括起来的情况、以"$1" "$2" … "$n" 的形式输出所有参数。
$-  | 显示Shell使用的当前选项，与set命令功能相同。
$?  | 显示最后命令的退出状态。**0表示没有错误，其他任何值表明有错误。**
                
                chmod +x argus.sh
                appledeMacBook-Pro:test lvhongbin$ ./argus.sh lv hong bin
                This is commond: ./argus.sh
                This is 1st arguements: lv
                This is 2nd arguements: hong
                This is 3rd  arguements: bin

                #! ./argus.sh
                # author: lvhongbin
                # description: receive outer arguments

                echo "This is commond: $0"
                echo "This is 1st arguements: $1"
                echo "This is 2nd arguements: $2"
                echo "This is 3rd  arguements: $3"


<h3 id='1.5'>1.5 数组 </h3>

#### 1) 格式
> - Shell 数组用括号来表示，元素用"空格"符号分割开
> - 读取数组的时候还是用回中括号
> - 也可以直接定义每个元素，如arr[0]=1
        
        appledeMacBook-Pro:lego_docs lvhongbin$ p=(1 2 3)
        appledeMacBook-Pro:lego_docs lvhongbin$ echo ${p[0]}
        1

#### 2) 获取数组中的所有元素
> - 使用@ 或 * 可以获取数组中的所有元素，如arr[@], arr[*]
> - 类似toString方法打印，但是连接各个元素的是空格
#### 3) 获取数组中的长度
> - 变量左花括号后的首字符添加井号"#"
            
                
                appledeMacBook-Pro:lego_docs lvhongbin$ echo ${#p[*]}
                3
                appledeMacBook-Pro:lego_docs lvhongbin$ echo ${#p[#]}
                -bash: #: syntax error: operand expected (error token is "#")
                appledeMacBook-Pro:lego_docs lvhongbin$ echo ${#p[@]}
                3


<h3 id='1.6'>1.6 运算符</h3>

#### 1) 注意
> - 原生bash不支持简单的数学运算，但是可以通过其他命令来实现，例如 awk 和 expr，expr 最常用
> - 下面使用expr举例
> - 注意算数式左右使用的是反上引号
> - 注意二元运算符左右需要有一个空格
                
                appledeMacBook-Pro:lego_docs lvhongbin$ echo `expr 1 + 2`
                3
#### 2) 算术运算符列表
> - 需要注意的是乘号需要转义
> - 相等yi
参数类型 | 参数说明 | 举例
-|-|-
+  | 加法 | `expr $a + $b` 结果为 30。
-  | 减法 | `expr $a - $b` 结果为 -10。
*  | 乘法 | `expr $a \* $b` 结果为  200。
/  |除法  | `expr $b / $a` 结果为 2。
%  |取余  | `expr $b % $a` 结果为 0。
=  | 赋值 | a=$b 将把变量 b 的值赋给 a。
== | 相等 | 用于比较两个数字，相同则返回 true。 [ $a == $b ] 返回 false。
!= | 不相等 |用于比较两个数字，不相同则返回 true。   [ $a != $b ] 返回 true。
#### 3) 关系运算符
> - 注意前面有短横线-
参数类型 | 参数说明 | 举例
-|-|-
-eq  |   检测两个数是否相等，相等返回 true。  |  [ $a -eq $b ] 返回 false。
-ne  |   检测两个数是否不相等，不相等返回 true。 | [ $a -ne $b ] 返回 true。
-gt  |   检测左边的数是否大于右边的，如果是，则返回 true。  |  [ $a -gt $b ] 返回 false。
-lt  |   检测左边的数是否小于右边的，如果是，则返回 true。  |   [ $a -lt $b ] 返回 true。
-ge  |   检测左边的数是否大于等于右边的，如果是，则返回 true。 |  [ $a -ge $b ] 返回 false。
-le  |   检测左边的数是否小于等于右边的，如果是，则返回 true。 |  [ $a -le $b ] 返回 true。
#### 4) 布尔运算符
> - 
参数类型 | 参数说明 | 举例
-|-|-
!    |   非运算，表达式为 true 则返回 false，否则返回 true。  |    [ ! false ] 返回 true。
-o   |   或运算，有一个表达式为 true 则返回 true。  |    [ $a -lt 20 -o $b -gt 100 ] 返回 true。
-a   |   与运算，两个表达式都为 true 才返回 true。  |    [ $a -lt 20 -a $b -gt 100 ] 返回 false。
                
                appledeMacBook-Pro:~ lvhongbin$ if [ 13 -eq 13 ]; then echo "13 -eq 13 : 13 等于 13"; fi
                13 -eq 13 : 13 等于 13
#### 5) 逻辑运算符
> - 使用双括号而非单括号，单括号会报错
> - 括号内边需要有空格进行分割
参数类型 | 参数说明 | 举例
-|-|-
&& | 逻辑的 AND | [[ $a -lt 100 && $b -gt 100 ]] 返回 false
|| | 逻辑的 OR  | [[ $a -lt 100 || $b -gt 100 ]] 返回 true
        
------      
        
<h2 id='2'>一、命令</h2>
<h3 id='2.1'>2.1 echo</h3>

        
#### 1) 打印
> - 这是最简单的功能
> - 加上参数-e 表示字符串转义字符生效，但前提是加上单引号或者是双引号
                
                appledeMacBook-Pro:test lvhongbin$ echo asd\nsda
                asdnsda
                appledeMacBook-Pro:test lvhongbin$ echo -e asd\nsda
                asdnsda
                appledeMacBook-Pro:test lvhongbin$ echo -e 'asd\nsda'
                asd
                sda
                appledeMacBook-Pro:test lvhongbin$ echo -e "asd\nsda"
                asd
                sda
                appledeMacBook-Pro:test lvhongbin$ echo "asd\nsda"
                asd\nsda
#### 2) 输出到文件
> - 使用 > 符号
                
                appledeMacBook-Pro:test lvhongbin$ ls
                0.2.8_0.crx     argus.sh        myProject       package.json
                0.2.8_0.pem     file            node_modules
                ab.js           lego-sdk-tool.zip   package-lock.json
                appledeMacBook-Pro:test lvhongbin$ touch echo
                appledeMacBook-Pro:test lvhongbin$ echo "i love you" > echo
                appledeMacBook-Pro:test lvhongbin$ cat echo
                i love you

<h3 id='2.2'>2.2 printf</h3>

        
#### 1) 与echo的不同
> - 不自动换行
> - 不需要添加参数-e便可使用转义符号
> - 可以添加格式替代符：-表示左对齐，没有-表示右对齐，
                
                appledeMacBook-Pro:test lvhongbin$ printf "%-10s %5s\n" 姓名 年龄; printf "%-10s %5d\n" John 20; printf "%-10s %5d\n" Apple 100 ; printf "%-10s %5d\n" Mike 20; printf "%-10s %5d\n" lvhongbin 40
                姓名     年龄
                John          20
                Apple        100
                Mike          20
                lvhongbin     40

<h3 id='2.3'>2.3 test</h3>

        
#### 1) 测试真伪
> - 用于检查字符串(采用=和！=)，数字(采用-英文字母)和文件
> - 相当于[ ]符号
> - 用在if语句中
                
                appledeMacBook-Pro:test lvhongbin$ if test 3 -eq 4 ; then echo "3 == 4" ; else  echo "3 != 4" ; fi
                3 != 4


#### 2) 数字比较
参数类型 | 参数说明
-|-
-eq  | 相等
-ne  | 不相等
-gt  | 大于
-ge  | 大于或等于
-lt  | 小于
-le  | 小于或等于
                
#### 3) 字符串比较
参数类型 | 参数说明
-|-
=  | 相等
!=  | 不相等
-z  | 字符串长度为0时为真
-n  | 字符串长度不为0时为真
                
#### 4) 文件比较
参数类型 | 参数说明
-|-
-e  | 文件存在为真
-r  | 文件存在且可读为真
-w  | 文件存在且可写为真
-x  | 文件存在且可执行为真
-d  | 文件存在且为目录为真
-f  | 文件存在且为普通文件为真
-c  | 文件存在为字符型特殊文件为真
-b  | 文件存在为块特殊文件为真







