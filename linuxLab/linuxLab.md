# linuxLab

## sed 用例

### 对 git 仓库 git log， 结果输出到 gitLab.md 的第 10 行之后

1. `sed -i '10a\'"$(git log)" gitLab.md`
   实例分支：
   [sed#1](../gitLab/gitLab.md?branch=linux#sed#1)

2. `git log | sed -i '10r /dev/stdin' gitLab.md`
   问题：/dev/stdin 是否多余
   实例分支：
   [sed#2](../gitLab/gitLab.md?branch=linux#sed#2)

### 在插入 git log 的每一行信息前加入一个制表符，以实现缩进

1. `sed -i '10a\'"$(git log | sed 's/^/\t/')" gitLab.md`
   实例分支：
   [sed#3](../gitLab/gitLab.md?branch=linux#sed#3)

2. `git log | sed 's/^/\t/' | sed -i '10r /dev/stdin' gitLab.md`

   实例分支：
   [sed#4](../gitLab/gitLab.md?branch=linux#sed#4)

   问题：第 2 个指令中，内部的 sed 为何加上 /dev/stdin, 而第 1 个指令却不需要

   - r 命令后面必须紧跟要读取的文件名，它无法直接从标准输入（管道 |）读取数据

3. 临时文件方法：

```bash
    # 将 git log 的格式化内容存入临时文件
    git log | sed 's/^/\t/' > /tmp/git_log_temp.txt
    # 使用 sed 的 r 命令读取临时文件并插入到第10行后
    sed -i '10r /tmp/git_log_temp.txt' gitLab.md
    # 清理临时文件
    rm /tmp/git_log_temp.txt
```

实例分支：
[sed#5](../gitLab/gitLab.md?branch=linux#sed#5)
