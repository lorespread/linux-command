下面是创建虚拟软盘镜像、放置编译好的引导程序，并用 QEMU 进行模拟的完整步骤：

---

### 1. **创建虚拟软盘镜像**

首先，使用 `qemu-img` 创建一个虚拟软盘镜像文件（大小为 1.44 MB，这是传统软盘的标准大小）：

```bash
qemu-img create -f raw floppy.img 1440K
```

---

### 2. **格式化虚拟软盘镜像**

虚拟软盘需要被格式化才能存放文件。使用 **mformat** 工具进行格式化。首先安装 `mtools`：

```bash
sudo apt install mtools
```

然后格式化镜像：

```bash
mformat -i floppy.img ::
```

---

### 3. **将引导程序写入软盘镜像**

引导程序需要放置在软盘镜像的 **第一个扇区**，即偏移地址 `0x0`，这可以用 `dd` 命令实现。假设您的引导程序文件名是 `boot.bin`（由汇编代码编译生成，大小必须 <= 512 字节）：

```bash
dd if=boot.bin of=floppy.img bs=512 count=1 conv=notrunc
```

- `if=boot.bin`：指定引导程序的文件路径。
- `of=floppy.img`：指定目标软盘镜像文件。
- `bs=512`：每次读取 512 字节。
- `count=1`：只写入一个扇区。
- `conv=notrunc`：不截断输出文件。

---

### 4. **使用 QEMU 模拟虚拟软盘**

运行以下命令启动 QEMU 并加载软盘镜像：

```bash
qemu-system-x86_64 -fda floppy.img
```

- `-fda floppy.img`：指定虚拟软驱镜像文件。
- 默认情况下，QEMU 会尝试从软盘引导。

---

### 5. **验证模拟结果**

启动 QEMU 后，虚拟机会加载您的引导程序。如果引导程序运行正常，会根据程序逻辑显示输出或执行特定任务。

---

### 6. **常见问题及解决方法**

#### 问题 1：QEMU 无法加载软盘镜像
确保软盘镜像正确格式化，并且引导程序写入了第一个扇区。如果有疑问，可以检查镜像内容：

```bash
hexdump -C floppy.img
```

确认前 512 字节是正确的引导代码。

#### 问题 2：引导程序无显示
- 检查代码逻辑是否正确，尤其是是否包含 BIOS 中断调用（如 `int 10h`）。
- 确保编译时使用正确的工具链（如 `nasm`），并使用 `org 0x7c00` 作为起始地址。

---

### 示例代码的完整流程

假设以下简单引导程序（`boot.asm`），它在屏幕上打印 `"Hello, QEMU!"`：

```asm
org 0x7c00           ; BIOS 加载地址
mov ah, 0x0E         ; BIOS 中断功能号，用于打印字符
mov si, msg          ; 指向字符串的地址
print:
    lodsb            ; 加载字符串中的一个字节到 AL
    cmp al, 0        ; 判断是否到达字符串结尾
    je hang
    int 0x10         ; 调用 BIOS 中断打印字符
    jmp print
hang:
    hlt              ; 停机
msg db 'Hello, QEMU!', 0

times 510-($-$$) db 0 ; 填充至 512 字节
dw 0xaa55            ; 引导扇区标志
```

1. 编译程序：

```bash
nasm -f bin boot.asm -o boot.bin
```

2. 按前述步骤将 `boot.bin` 写入软盘镜像，并使用 QEMU 进行模拟。

---

通过这些步骤，您应该可以成功将引导程序写入软盘镜像并在 QEMU 中测试。如果有更多问题，请随时告诉我！
