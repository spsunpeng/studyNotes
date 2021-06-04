### 一、 查看文件

#### 1、查看文件

| 命令 | 解释                   | 常用                    | 解释             |
| ---- | ---------------------- | ----------------------- | ---------------- |
| cat  | 从第一行开始显示       | **cat filename**        | -n可以显示行号   |
| tac  | 从最后一行开始显示     |                         |                  |
| more | 一页一页的显示档案内容 |                         | d：向前翻页      |
| less | 相比more，可以翻页     | shift+g 移动到最后一行  | b：向后翻页      |
| head | 只看头几行             | head-n 20 filename      | 头10页，默认为10 |
| tail | 只看尾巴几行           | **tail -n 20 filename** | 尾10页，默认为10 |

#### 2、 cat

1. 命令格式：

   cat [选项] [文件]...

2. 命令功能：

   cat主要有三大功能：

   1. 一次显示整个文件: cat filename

   2. 从键盘创建一个文件: cat > filename 只能创建新文件,不能编辑已有文件.

   3. 将几个文件合并为一个文件: cat file1 file2 > file

3. 命令参数：

   -n, --number   对输出的所有行编号,由1开始对所有输出的行数编号

   

### 二、过滤 grep

#### 1、使用方法

grep  -参数  "字段"  filename

- 参数

  - -E:  或

  - -r ：进入目录搜索
  - -n :  显示行号

- 搜索条件

  - "word" ： 搜索含有word的，支持汉字、特殊字符、空格

  - 支持部分正则表达式 "getDictData.*7," 搜索含有 getDictData和7

  - “word1|word2” ：参数没有-E则|就是原意，若有则表示或

- fileName：可以是文件，也可以是目录(-r)

#### 2、示例

- grep “word” fliename                                   含有word
- grep “汉字| :word” fliename                       含有汉字| :word
- grep -E “word1|word2” fliename               含有word1或word1

- grep "word1.*word2" fliename                  含有word1和word2（有word1...word2）
- grep “word1” fliename grep "word2"        含有word1和word2（有word2的基础上有word1）



### 三、vi

1）上下翻动：向上箭头、向下箭头：每次滚动一条日志，这条日这可能占据多行

2）前后翻页：ctrl+f、ctrl+b：f就是forword，b就是backward

3）首行末行：首行（gg），末行（shift+g）

4）查找：?string：查找字符串，N向前，n向后

5）分页分屏：k8s中的vi也支持分页/分屏操作





### 四、网络

```sh
ip addr #查询ip
ifconfig  #查询ip
uname -v #查询虚拟机内核版本
ps -ef    #查询进程
jmap -heap pid   #查询堆内存
```



