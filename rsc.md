# read source code(rsc) of dcos-cli

1. 在函数里面定义函数，有什么优缺点和使用场景？
2. 有些类里面有一个函数实现非常一般的功能，然后有很多“代理”函数通过调用“一般函数”和参数控制来获取个性化结果。
    - dcos/mesos.py  `_framework_dicts`
    - dcos/mesos.py `tasks`
3. docstring用法
4. threading的用法
5. 队列中的set(),wait()等的用法
6. 头部中的application/recordio信息
7. queue.task_done(),queue.get()什么意思
8. urllib库

## 将函数操作和状态更新封装到一个函数中
dcos-cli/cli/log.py::log_files()函数
```
def log_files(mesos_files, follow, lines):
fn = functools.partial(_read_last_lines, lines)
    curr_header, mesos_files = _stream_files(None, fn, mesos_files)
    if not mesos_files:
        raise _no_file_exception()

    while follow:
        # This flush is needed only for testing, since stdout is fully
        # buffered (as opposed to line-buffered) when redirected to a
        # pipe.  So if we don't flush, our --follow tests, which use a
        # pipe, never see the data
        sys.stdout.flush()
        # 调用_stream_files函数，通过mesos_file读取内容然后返回
        # 更新后的mesos_file,这个设计值得借鉴
        curr_header, mesos_files = _stream_files(curr_header,
                                                 _read_rest,
                                                 mesos_files)
        if not mesos_files:
            raise _no_file_exception()
        time.sleep(1)
```