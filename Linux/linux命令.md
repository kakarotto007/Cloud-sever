# nohup

挂后台命令

1. 在后台运行 bash脚本

```bash
nohup bash test.sh &
```

2. 标准错误输出重定向的文件

```bash
nohup bash test.sh > stderr.txt &
```

3. 将标准输出和标准错误输出都重定向到文件

```bash
nohup bash test.sh > output.txt 2>&1 &
```



> 可以使用jobs命令查看当前shell中后台运行的任务列表

```bash
$ jobs
[1]+  Running                 nohup sleep 1000 &
```

如果想要杀死该任务，可以使用kill命令：

```bash
$ kill %1
[1]+  Terminated              nohup sleep 1000
```

