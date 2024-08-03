# 6.S081

6.S081 操作系统课程的代码以及实现过程中的记录。

课程链接：https://pdos.csail.mit.edu/6.828/2021/schedule.html

参考资料：https://cactus-agenda-c84.notion.site/XV6-labs-2021-0894f931b3324edea30dca7826c01a97



## 环境搭建

用的是 MacOS 系统，参考：https://pdos.csail.mit.edu/6.828/2021/tools.html

实验代码的拉取参考（不同的是我拉取的是2021版的代码）：https://xv6.dgs.zone/labs/use_git/git1.html



## lab0 util

### Boot xv6

配置好环境后，实验代码切换到 `util` 分支然后用命令 `make qemu` 启动就行。

这时候遇到了个报错：`user/sh.c:58:1: error: infinite recursion detected [-Werror=infinite-recursion]`

参考 https://github.com/mit-pdos/xv6-riscv/issues/125，在 `user/sh.c` 里的 56 行添加一行 `__attribute__((noreturn))` 就解决了。

### sleep

文档里提示可以参考 `user/echo.c`, `user/grep.c`,  `user/rm.c` 如何处理命令行参数。

