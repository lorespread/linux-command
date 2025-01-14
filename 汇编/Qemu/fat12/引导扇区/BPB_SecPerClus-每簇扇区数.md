### 📖 **BPB_SecPerClus（每簇扇区数）详解**

`BPB_SecPerClus` 是 FAT 文件系统中 **BIOS Parameter Block (BPB)** 结构的一部分，用于描述文件系统的基本属性。它表示 **每簇的扇区数**，即一个簇（Cluster）包含多少个扇区。

---

### 💡 **背景概念：**

1. **扇区（Sector）：**
   - 磁盘存储的最小单位，一般大小为 512 字节。
   - 每个磁盘的 BPB 都会明确扇区大小（`BPB_BytsPerSec`，字节/扇区）。

2. **簇（Cluster）：**
   - 文件系统分配空间的基本单位。
   - 一个簇可以由一个或多个连续的扇区组成。
   - 簇的大小取决于 `BPB_SecPerClus` 的值。

3. **逻辑地址计算：**
   - 一个簇的总大小 = `BPB_BytsPerSec` × `BPB_SecPerClus`。

---

### 🔍 **BPB_SecPerClus 的作用：**

- 定义簇的大小，影响文件系统的存储效率。
- 在 FAT 文件系统中，簇是分配文件存储空间的单位。
  - **小簇：** 节省磁盘空间，但会增加 FAT 表的大小。
  - **大簇：** 减少 FAT 表的大小，但会浪费空间（称为簇内碎片）。

---

### ⚙️ **FAT12 中的 BPB_SecPerClus：**

- **字段位置：**  
  在 BPB 结构中的偏移量为 0x0D，占 1 字节。
  
- **取值范围：**  
  - FAT12 通常取值为 1、2、4、8（即 1 簇可以是 1、2、4 或 8 个扇区）。
  - 值必须为 2 的幂，如 1、2、4、8、16、32、64。

- **用途：**
  - 计算逻辑簇号（Logical Cluster Number, LCN）对应的物理地址：
    ```math
    \text{物理扇区号} = \text{起始扇区} + (\text{簇号} - 2) \times \text{BPB\_SecPerClus}
	```

  - 簇号从 2 开始编号，因为 0 和 1 簇是保留的（一般用于 FAT 表和根目录）。

---

### 🔢 **举例说明：**

假设以下 BPB 信息：

| 字段                | 值          |
|---------------------|-------------|
| BPB_BytsPerSec      | 512 字节    |
| BPB_SecPerClus      | 4 扇区      |
| BPB_RsvdSecCnt      | 1 扇区      |
| BPB_NumFATs         | 2           |
| BPB_FATSz           | 9 扇区      |

- **每簇大小**:  

```math
  \text{簇大小} = BPB\_BytsPerSec \times BPB\_SecPerClus = 512 \times 4 = 2048 \, \text{字节}
```


- **文件地址计算**:  
  如果文件在第 5 簇（LCN=5），其起始扇区号为：

```math
  \text{起始扇区号} = \text{BPB\_RsvdSecCnt} + (\text{簇号} - 2) \times BPB\_SecPerClus
```

```math
  = 1 + (5 - 2) \times 4 = 13 \, \text{扇区号}
```



---

### 📊 **FAT 文件系统选择 BPB_SecPerClus 的影响：**

| BPB_SecPerClus | 簇大小 | FAT 表大小 | 磁盘碎片风险 | 适用场景          |
|-----------------|--------|------------|--------------|-------------------|
| 小（如 1）      | 小      | 大          | 低            | 小文件、低容量磁盘 |
| 大（如 8）      | 大      | 小          | 高            | 大文件、大容量磁盘 |

---

### ✏️ **总结：**

- **`BPB_SecPerClus`** 定义了一个簇由多少个扇区组成，直接影响文件系统的性能和空间利用率。
- 在 FAT12 文件系统中，合理选择簇大小能够平衡 FAT 表大小和空间浪费。
