在 FAT12 文件系统中，**FAT 表（File Allocation Table）** 和 **根目录区（Root Directory）** 是两个不同的区域，它们在文件系统中扮演不同的角色。

---

## 🔹 **1. FAT 表（File Allocation Table, FAT）**

---

### 📘 **作用：**
FAT 表用于记录磁盘簇（Cluster）分配的信息，即每个簇与其他簇之间的链接关系。

---

### 🔍 **主要功能：**
- 每个簇都由一个表项来描述。
- 在 FAT12 中，每个表项占用 **12 位**。
- 每个表项指向：
  - 另一个簇的位置。
  - 标识簇是否为空、损坏、结束等状态。
- 通过这些表项，可以实现对非连续磁盘空间的链接，从而达到文件存储分散的目的。

---

### 📊 **位置：**
- FAT 表一般存储在磁盘的 **保留区域（Reserved Area）**。
- 通常占用若干个簇，例如：常见的 **9 个簇**。

### 📖 **示意：**
- 如果一个文件跨越多个簇，那么 FAT 表会存储每个簇与下一个簇的连接信息。
- 示例：  
  ```
  Cluster 1 -> Cluster 2 -> Cluster 5 -> End of File
  ```

---

## 🔹 **2. 根目录区（Root Directory）**

---

### 📘 **作用：**
根目录区存储磁盘根目录下的所有文件和目录信息。

---

### 🔍 **主要功能：**
- 根目录存储的是文件名、属性、时间戳、起始簇号等信息。
- 在 FAT12 文件系统中，**根目录是固定大小的**。
  - 根目录区最多可以容纳 **224 个条目**。
- 每个条目包含：
  - 文件名、扩展名。
  - 文件属性（只读、隐藏、系统等）。
  - 文件起始簇号（指向实际数据存储位置）。

---

### 📊 **位置：**
- 根目录通常位于 **FAT 表之后**。
- 根目录并不存储实际数据，仅存储文件信息。
- 在小容量磁盘（如软盘）上，根目录固定在磁盘结构中。

---

## 📍 **FAT 表与根目录区的区别总结：**

| 特性                  | **FAT 表 (File Allocation Table)**                          | **根目录区 (Root Directory)**                          |
|----------------------|----------------------------------------------------------|--------------------------------------------------------|
| **作用**             | 记录磁盘簇之间的分配信息                                  | 存储根目录下所有文件和目录的元数据                     |
| **数据存储位置**     | 存放在磁盘的保留区域（Reserved Area）                    | 紧接在保留区域后面，包含文件信息                         |
| **表项大小**         | 在 FAT12 中，每条表项占 **12 位（1.5 字节）**          | 每个条目占固定大小，如 **32 字节**                       |
| **可存储文件数限制** | 依赖于簇数量和分配信息                                  | 根目录区通常最多可以存 **224 个文件/目录条目**        |
| **跨簇链接信息**     | 每个簇都由 FAT 表中的条目链接指向下一个簇的位置         | 仅存储指向起始簇的地址，不存储跨簇链接信息             |

---

## 🎯 **实际使用示例：**
假设你有一个 FAT12 文件系统：
1. 文件 `file.txt` 跨越多个簇。
2. 在 **FAT 表中**：
   - 保存每个簇的位置和链接信息。
3. 在 **根目录区中**：
   - 包含文件名、扩展名、起始簇号、时间戳、属性等。

这样，系统可以通过根目录区中的起始簇号快速定位到实际数据所在的簇位置，而 FAT 表则提供了簇之间的链接顺序。
