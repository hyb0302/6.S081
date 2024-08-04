# 6.S081

6.S081 操作系统课程的代码以及实现过程中的记录。

课程链接：https://pdos.csail.mit.edu/6.828/2021/schedule.html

参考资料：https://cactus-agenda-c84.notion.site/XV6-labs-2021-0894f931b3324edea30dca7826c01a97



## 环境搭建

用的是 MacOS 系统，参考：https://pdos.csail.mit.edu/6.828/2021/tools.html

这里有个卡了我两天的点，我照着说明用命令 `brew install riscv-tools` 安装了 riscv 的交叉编译工具链。gcc 什么的都有，但就是没有 `riscv64-unknown-elf-gdb`。

我尝试了好几种方式安装，上网搜了好久也没找到为啥，只找到了一个老哥有和我一样的[问题](https://unix.stackexchange.com/questions/748867/riscv64-unknown-elf-gdb-not-found-after-installing-gcc-riscv64-linux-gnu-debian)，但也没有答案。

最后想着那就自己装个 gdb 吧，在网上搜到一篇[教程](https://zjp-cn.github.io/posts/rcore-gdb/)，跟着教程的前半部分下载了 gdb 源码（和文章里不同的是我下载的是15.1版本的 gdb），接着编译生成二进制文件，这个阶段大概需要十几分钟，生成之后试了下没啥问题。

````bash
../configure --prefix=/Users/huangyb/code/github-repositories/gdb-15.1/build-riscv64 --target=riscv64-unknown-elf --enable-tui=yes —with-gmp=/opt/homebrew/Cellar/gmp/6.3.0 —with-mpfr=/opt/homebrew/Cellar/mpfr/4.2.1
````

最后用软链接把 `riscv64-unknown-elf-gdb` 链接到 `/usr/local/bin` 目录下，让 gdb 在全局可用。

```bash
cd /usr/local/bin
sudo ln -s ~/code/github-repositories/gdb-15.1/build-riscv64/bin/riscv64-unknown-elf-gdb ./riscv64-unknown-elf-gdbs
```



实验代码的拉取参考（不同的是我拉取的是2021版的代码）：https://xv6.dgs.zone/labs/use_git/git1.html

## lab0 util

### Boot xv6

配置好环境后，实验代码切换到 `util` 分支然后用命令 `make qemu` 启动就行。

这时候遇到了个报错：`user/sh.c:58:1: error: infinite recursion detected [-Werror=infinite-recursion]`

在网上找到了个 [issuse](https://github.com/mit-pdos/xv6-riscv/issues/125) ，里边描述了需要在 `user/sh.c` 里的 56 行添加一行 `__attribute__((noreturn))` ，保存后再次启动就没问题了。

### sleep

文档里提示可以参考 `user/echo.c`, `user/grep.c`,  `user/rm.c` 如何处理命令行参数。

