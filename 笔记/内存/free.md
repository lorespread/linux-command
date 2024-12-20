# 使用 free 查看当前系统内存使用情况

## free 命令

```shell
free --help

Usage:
 free [options]

Options:
 -b, --bytes         show output in bytes
 -k, --kilo          show output in kilobytes
 -m, --mega          show output in megabytes
 -g, --giga          show output in gigabytes
     --tera          show output in terabytes
     --peta          show output in petabytes
 -h, --human         show human-readable output
     --si            use powers of 1000 not 1024
 -l, --lohi          show detailed low and high memory statistics
 -t, --total         show total for RAM + swap
 -s N, --seconds N   repeat printing every N seconds
 -c N, --count N     repeat printing N times, then exit
 -w, --wide          wide output

     --help     display this help and exit
 -V, --version  output version information and exit

For more details see free(1).
```

## 查看内存使用情况

```shell
# free -h
              total        used        free      shared  buff/cache   available
Mem:            15G        7.7G        1.1G        584K        6.7G        7.5G
Swap:            0B          0B          0B
```

`free -h` 命令用于显示系统的内存使用情况，其中的各个字段代表了系统内存的不同指标。以下是对每个字段的详细解释：

### 1. **Mem:**
   这是物理内存（RAM）的状态。  
   - **total**: 系统总内存，即物理内存的总容量。
   - **used**: 已使用的内存，包括正在运行的程序和缓存等所使用的内存。
   - **free**: 当前未被使用的内存，即完全空闲的内存。
   - **shared**: 共享内存的大小，通常与进程间共享数据或资源有关。
   - **buff/cache**: 被用作缓存或缓冲区的内存，主要用于提高系统性能。它可能包括文件缓存和交换缓冲区。
   - **available**: 可用内存，表示系统可以用来分配给进程的内存量。这个值考虑了正在缓存的内存，因为 Linux 会在不需要时回收缓存。

🚀🚀：注意 available 指标

### 2. **Swap:**
   这是虚拟内存的状态。虚拟内存用于扩展物理内存，当物理内存不足时，系统会将不常用的内存页写入硬盘上的交换空间（swap），以释放物理内存供当前进程使用。
   - **total**: 交换分区的总容量。此处为 0B，表示没有配置交换空间。
   - **used**: 已使用的交换空间。此处为 0B，表示没有使用交换空间。
   - **free**: 未使用的交换空间。此处为 0B，表示没有可用的交换空间。

### 具体分析：
- **总内存 (total)**：系统总内存为 15GB。
- **已用内存 (used)**：当前已使用 7.7GB 内存，这包括了正在运行的程序和缓存。
- **空闲内存 (free)**：当前空闲的内存为 1.1GB，但需要注意，Linux 系统会利用空闲内存进行缓存，因此空闲内存的值通常较小。
- **共享内存 (shared)**：共享内存为 584K，通常是进程之间共享的内存区域。
- **缓存/缓冲内存 (buff/cache)**：缓存和缓冲区占用了 6.7GB 的内存，这部分内存实际上并不是被程序直接使用，它是被用来加速文件系统操作、存储临时数据等。Linux 会根据需要回收这些缓存内存，以腾出空间给程序。
- **可用内存 (available)**：7.5GB 的可用内存，意味着即使当前有 7.7GB 的内存被占用，但系统仍然可以提供 7.5GB 内存给新的进程或需求。

### 总结：
- **缓存内存**占用了很大一部分内存，这在 Linux 系统中是正常的，因为 Linux 会自动使用空闲内存进行缓存以提高性能。
- 系统并没有使用交换空间，因此系统的内存使用情况在没有配置交换空间的情况下看起来较为“健康”。
- **可用内存 (available)** 是你需要关注的最重要的指标，因为它表示了你能用来分配给新进程的内存量。在当前的情况下，系统有 7.5GB 的可用内存。
