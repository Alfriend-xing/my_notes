# subprocess

python3子进程管理模块，可创建子进程执行指定命令。

主要有两个创建子进程的方法
- `subprocess.run` 创建的进程会阻塞当前进程，返回`subprocess.CompletedProcess`对象
- `subprocess.Popen` 创建异步执行的子进程，返回`subprocess.Popen`对象

## 调用样例

```python
subprocess.run(['ping','baidu.com','-c','10'])
subprocess.Popen(['ping','baidu.com','-c','10'])
```

## 类方法和参数

```python
subprocess.run(args, *, stdin=None, input=None, stdout=None, stderr=None, capture_output=False, shell=False, cwd=None, timeout=None, check=False, encoding=None, errors=None, text=None, env=None, universal_newlines=None)
# 如果capture_output设为True，则标准输出和标准错误会被导向subprocess.PIPE中，相当于stdout=PIPE 和 stderr=PIPE
# shell通常为False，表示命令不会在新建的shell中执行
# cwd为None表示以当前文件路径为执行命令的工作路径，否则需要提供一个路径字符串
# timeout设置超时时间，超时后杀死子进程
# encoding，如果设为utf-8，则标准输出将为字符串''，设为None则输出字节码b''。''=b''.decode(encoding)
# 其他参数略

subprocess.CompletedProcess
    # run() 的返回值, 代表一个进程已经结束
    args # 执行的命令，值为str或list
    returncode # 子进程的退出状态码.正常退出为0
    stdout # 从子进程捕获到的标准输出. 一个字节序列(run中encoding为None的情况下)
    # 可以设置stderr=subprocess.STDOUT，这样标准输入和标准错误将被组合在一起
    stderr # 标准错误. 一个字节序列
    check_returncode() # 如果 returncode 非零, 抛出 CalledProcessError.

subprocess.Popen(args, bufsize=-1, executable=None, stdin=None, stdout=None, stderr=None, preexec_fn=None, close_fds=True, shell=False, cwd=None, env=None, universal_newlines=None, startupinfo=None, creationflags=0, restore_signals=True, start_new_session=False, pass_fds=(), *, encoding=None, errors=None, text=None)
# args为执行命令
# bufsize=-1表示使用系统默认的 io.DEFAULT_BUFFER_SIZE
# stdin, stdout 和 stderr指定标准输入，标准输出，标准错误文件句柄，设为None则所有句柄指向父进程，设为PIPE表示新建管道单独保存，
    # Popen 构造函数，返回Popen 对象
    Popen.poll() # 检查子进程是否已被终止，为终止返回None，终止返回returncode 属性
    Popen.wait(timeout=None) # 如果进程在 timeout 秒后未中断，抛出一个 TimeoutExpired 异常
    Popen.communicate(input=None, timeout=None) 
        # 与进程交互
        # 阻塞至子进程终止，返回一个 (stdout_data, stderr_data) 元组
        
        # 注意如果你想要向进程的 stdin 传输数据，你需要通过 stdin=PIPE 创建此 Popen 对象。类似的，要从结果元组获取任何非 None 值，你同样需要设置 stdout=PIPE 或者 stderr=PIPE。

        # 如果进程在 timeout 秒后未终止，一个 TimeoutExpired 异常将被抛出。捕获此异常并重新等待将不会丢失任何输出。
    Popen.terminate() # 发送 SIGTERM信号,停止子进程
    Popen.kill() # 发送 SIGKILL 信号,杀死子进程
    Popen.args # 执行的命令
    Popen.stdin
        # 如果 stdin 参数为 PIPE，此属性是一个类似 open() 返回的可写的流对象。如果 encoding 或 errors 参数被指定或者 universal_newlines 参数为 True，则此流是一个文本流，否则是字节流。如果 stdin 参数非 PIPE， 此属性为 None。
    Popen.stdout
    Popen.stderr
    Popen.pid # 子进程的进程号
    Popen.returncode # 此进程的退出码,None 值 表示此进程仍未结束。负值 -N 表示子进程被信号 N 中断
```
















