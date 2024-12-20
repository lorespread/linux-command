# 1 qemu guest has not initialized the display yet

当使用 `qemu-system-x86_64 -drive format=raw,file=boot.bin -S` 命令启动 QEMU 时，出现 **"qemu guest has not initialized the display yet"** 的原因是引导程序（`boot.bin`）本身没有设置显示相关的硬件环境，比如没有初始化 VGA 或显卡模式。QEMU 的显示输出是依赖引导程序或操作系统进行初始化的。

这是因为：
1. **BIOS 加载引导程序后**，QEMU 等待引导程序初始化系统硬件（如设置视频模式）。
2. 如果引导程序只是简单的测试代码（比如打印字符）而没有初始化显示设备，就不会有显示输出。

---

### **解决方法**

#### 方法 1: 确保引导程序支持视频输出
如果要在屏幕上显示内容，您的引导程序需要初始化视频模式并使用 BIOS 中断调用显示文本内容。例如，使用 INT 10H BIOS 中断来输出字符。

以下是一个简单的引导程序，确保正确初始化显示内容：

```asm
[org 0x7c00]            ; BIOS 加载引导扇区的起始地址

; 设置视频模式为 80x25 文本模式
mov ah, 0x00            ; 功能号：设置视频模式
mov al, 0x03            ; 模式号：80x25 文本模式
int 0x10                ; 调用 BIOS 中断

; 显示 "Hello, QEMU!"
mov ah, 0x0E            ; 功能号：显示字符
mov al, 'H'             ; 显示字符 H
int 0x10
mov al, 'e'
int 0x10
mov al, 'l'
int 0x10
mov al, 'l'
int 0x10
mov al, 'o'
int 0x10
mov al, ','
int 0x10
mov al, ' '
int 0x10
mov al, 'Q'
int 0x10
mov al, 'E'
int 0x10
mov al, 'M'
int 0x10
mov al, 'U'
int 0x10
mov al, '!'
int 0x10

; 死循环
jmp $

times 510-($-$$) db 0   ; 填充至 512 字节
dw 0xAA55               ; 引导扇区标志
```

编译并运行：
```bash
nasm -f bin boot.asm -o boot.bin
qemu-system-x86_64 -drive format=raw,file=boot.bin -S
```

---

#### 方法 2: 使用 GDB 检查执行状态
当使用 `-S` 选项启动时，QEMU 会暂停 CPU 执行，需要通过 GDB 或 QEMU Monitor 继续运行。

**步骤：**
1. 启动 QEMU，保持暂停状态：
   ```bash
   qemu-system-x86_64 -drive format=raw,file=boot.bin -S -gdb tcp::1234
   ```

2. 启动 GDB 并连接：
   ```bash
   gdb
   target remote localhost:1234
   ```

3. 使用 GDB 检查是否正常加载了代码：
   - 查看 CPU 的当前状态：
     ```bash
     info registers
     ```
   - 查看引导程序加载的起始地址（通常是 `0x7C00`）：
     ```bash
     x/16xb 0x7c00
     ```
   - 如果无问题，可以继续执行程序：
     ```bash
     continue
     ```

---

#### 方法 3: 使用 QEMU Monitor 检查状态
QEMU Monitor 提供额外的调试功能，可以验证是否正确加载了引导程序。

**步骤：**
1. 在启动 QEMU 后，按下 `Ctrl+Alt+2` 进入 QEMU Monitor。
2. 查看 CPU 寄存器状态：
   ```bash
   info registers
   ```
3. 检查 0x7C00 地址的内容：
   ```bash
   x /16xb 0x7c00
   ```
4. 如果无问题，继续执行：
   ```bash
   cont
   ```

---

### **可能的问题与解决**
1. **引导程序无输出指令：**
   - 确保引导程序使用 INT 10H 中断来输出内容。

2. **BIOS 扇区未正确加载：**
   - 检查是否正确编译为 512 字节的引导扇区。
   - 确保引导程序以 `0xAA55` 结束，这是引导扇区的标志。

3. **CPU 暂停状态：**
   - 如果使用了 `-S` 选项，需要通过 GDB 或 QEMU Monitor 继续运行。

---

### **总结**
- **"qemu guest has not initialized the display yet"** 表示引导程序未初始化显示输出，需通过 BIOS 中断 `int 10h` 初始化视频模式并输出内容。
- 使用 GDB 或 QEMU Monitor 可以进一步调试和分析引导程序的执行状态。
