# psutil
    跨平台进程管理

## CPU
```python
psutil.cpu_times(percpu=False)
#返回CPU运行时间，包括user(用户模块)，system(系统模块)，idle(空闲时间)

psutil.cpu_percent(interval=None, percpu=False)
#返回表示利用率的浮点数，percpu为True以列表形式返回每个线程的利用率
#interval为计算间隔时间，不应为0.0或None

psutil.cpu_times_percent(interval=None, percpu=False)
#运行时间百分比

psutil.cpu_count(logical=True)
#逻辑CPU数量

psutil.cpu_stats()
#CPU运行信息，切换上下文和中断

psutil.cpu_freq(percpu=False)
#返回CPU频率



```

## 内存
```python
psutil.virtual_memory()
#返回命名元组，包括total和available，其他字段忽略

psutil.swap_memory()
#返回交换内存统计信息
# total：总交换内存，以字节为单位
# used：以字节为单位使用的swap内存
# free：以字节为单位的自由交换内存
# percent：计算的百分比使用率(total - available) / total * 100
# sin：系统从磁盘交换的字节数（累计）
# sout：系统从磁盘换出的字节数（累计）
```
## 磁盘
```python
psutil.disk_partitions(all=False)
#返回所有分区

psutil.disk_usage(path)
#返回使用情况

psutil.disk_io_counters(perdisk=False, nowrap=True)
#返回读写情况
# read_count：读取次数
# write_count：写入次数
# read_bytes：读取的字节数
# write_bytes：写入的字节数


```
## 网络
```python
psutil.net_io_counters(pernic=False, nowrap=True)
#网络情况，pernic为True返回每个接口的信息，nowrap为True表示数据包含历史值
# bytes_sent：发送的字节数
# bytes_recv：接收的字节数
# packets_sent：发送的包数
# packets_recv：接收的数据包数
# errin：接收时的错误总数
# errout：发送时的错误总数
# dropin：丢弃的传入数据包总数
# dropout：丢弃的传出数据包总数（macOS和BSD总是0）

psutil.net_connections(kind='inet')
#返回连接信息，进程，协议，IP，端口和状态

psutil.net_if_addrs()
#返回接口网关地址和分配IP

psutil.net_if_stats()
#返回每个网口开放状态，连接速度


```
## 传感器
>部分功能不支持windows
```python
psutil.sensors_temperatures(fahrenheit=False)
#返回硬件温度元组，华氏=假

psutil.sensors_fans()

psutil.sensors_battery()


```
## 其他系统信息
```python
psutil.boot_time()
#启动时间

psutil.users()
#登陆用户


```
## 进程

### 函数
```python
psutil.pids()
#返回进程id列表

psutil.process_iter(attrs=None, ad_value=None)
#返回进程对象迭代器
#指定attrs=['pid'...]会将指定字段附加至info属性中返回，
#指定空列表会返回所有信息

psutil.pid_exists(pid)
#检查进程列表中是否存在指定id

psutil.wait_procs(procs, timeout=None, callback=None)
#返回消失和继续运行的进程元组
# 等待进程结束后回调 procs指定监控的进程
```
### 异常
```python
class psutil.Error
#异常基类

class psutil.NoSuchProcess(pid, name=None, msg=None)
#进程未找到

class psutil.ZombieProcess(pid, name=None, ppid=None, msg=None)
#僵尸进程(unix独有)

class psutil.AccessDenied(pid=None, name=None, msg=None)
#无权限

class psutil.TimeoutExpired(seconds, pid=None, name=None, msg=None)
#Process.wait()超时
```
### 类
```python
class psutil.Process(pid=None)

    oneshot()
    #使用上下文管理 加快读取进程信息
    # with p.oneshot():
    #   p.name()

    pid
    #进程id 只读

    ppid()
    #父id

    name()
    #进程名称

    exe()
    #执行路径

    cmdline()
    #执行命令

    environ()
    #环境变量

    create_time()
    #创建时间戳 秒

    as_dict(attrs=None, ad_value=None)
    #查询多个信息
    #p.as_dict(attrs=['pid', 'name', 'username'])

    parent()
    #返回父进程对象

    parents()
    #返回所有父进程对象

    status()
    #进程状态 返回的字符串是 psutil.STATUS_*常量之一

    cwd()
    #进程当前工作目录的绝对路径

    username()
    #拥有该进程的用户的名称

    uids()
    #用户id

    gids()
    #用户组

    terminal()
    #与此进程有关的终端，没有返回None

    nice(value=None)
    #获取或设置优先级，常见范围-20到20 优先级递减
    # p.nice(10)  # set
    # p.nice()  # get

    ionice(ioclass=None, value=None)
    #获取设置io优先级 0-7

    rlimit(resource, limits=None)
    #获取资源限制

    io_counters()
    #io统计

    num_ctx_switches()

    num_fds()

    num_handles()

    num_threads()
    #当前线程数 非累积

    threads()

    cpu_times()
    #CPU使用时间

    cpu_percent(interval=None)
    #CPU利用率

    cpu_affinity(cpus=None)

    cpu_num()

    memory_info()

    memory_info_ex()

    memory_full_info()

    memory_percent(memtype="rss")

    memory_maps(grouped=True)

    children(recursive=False)

    open_files()
    #返回进程打开的文件

    connections(kind="inet")

    is_running()

    send_signal(signal)
    #向进程发送信号

    suspend()

    resume()

    terminate()
    #使用SIGTERM信号终止进程

    kill()
    #使用SIGKILL信号杀死当前进程

    wait(timeout=None)
    #等待进程终止，返回状态码(子进程),或None


class psutil.Popen(*args, **kwargs)

```
>[可发送的信号](
    https://docs.python.org/3/library/signal.html)

## Windows services进程
>查看windows进程

## 静态变量
https://psutil.readthedocs.io/en/latest/#constants
## 例子
按名称查找进程
```python
import psutil

def find_procs_by_name(name):
    "Return a list of processes matching 'name'."
    ls = []
    for p in psutil.process_iter(attrs=['name']):
        if p.info['name'] == name:
            ls.append(p)
    return ls
```
```python
import os
import psutil

def find_procs_by_name(name):
    "Return a list of processes matching 'name'."
    ls = []
    for p in psutil.process_iter(attrs=["name", "exe", "cmdline"]):
        if name == p.info['name'] or \
                p.info['exe'] and os.path.basename(p.info['exe']) == name or \
                p.info['cmdline'] and p.info['cmdline'][0] == name:
            ls.append(p)
    return ls
```
杀死进程树
```python
import os
import signal
import psutil

def kill_proc_tree(pid, sig=signal.SIGTERM, include_parent=True,
                   timeout=None, on_terminate=None):
    """Kill a process tree (including grandchildren) with signal
    "sig" and return a (gone, still_alive) tuple.
    "on_terminate", if specified, is a callabck function which is
    called as soon as a child terminates.
    """
    assert pid != os.getpid(), "won't kill myself"
    parent = psutil.Process(pid)
    children = parent.children(recursive=True)
    if include_parent:
        children.append(parent)
    for p in children:
        p.send_signal(sig)
    gone, alive = psutil.wait_procs(children, timeout=timeout,
                                    callback=on_terminate)
```
结束子进程
```python
import psutil

def reap_children(timeout=3):
    "Tries hard to terminate and ultimately kill all the children of this process."
    def on_terminate(proc):
        print("process {} terminated with exit code {}".format(proc, proc.returncode))

    procs = psutil.Process().children()
    # send SIGTERM
    for p in procs:
        try:
            p.terminate()
        except psutil.NoSuchProcess:
            pass
    gone, alive = psutil.wait_procs(procs, timeout=timeout, callback=on_terminate)
    if alive:
        # send SIGKILL
        for p in alive:
            print("process {} survived SIGTERM; trying SIGKILL" % p)
            try:
                p.kill()
            except psutil.NoSuchProcess:
                pass
        gone, alive = psutil.wait_procs(alive, timeout=timeout, callback=on_terminate)
        if alive:
            # give up
            for p in alive:
                print("process {} survived SIGKILL; giving up" % p)
```



[文档地址](https://psutil.readthedocs.io/en/latest/)