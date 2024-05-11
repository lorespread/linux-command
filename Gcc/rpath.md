# 1 详解下 rpath

`rpath`（runtime library search path）和 `runpath` 是链接选项，用于在编译时指定程序运行时动态链接器搜索共享库的路径。这两个选项是通过链接器（如 GNU 的 `ld`）设置的，它们嵌入到可执行文件或动态库中，指定除了标准系统路径外，链接器还应该在哪些路径搜索所需的共享库。

### rpath vs. runpath

-   **rpath**:

    -   `rpath` 是一个旧的链接选项，用于指定共享库搜索路径。
    -   如果设置了 `rpath`，则在所有标准库路径和 `LD_LIBRARY_PATH` 环境变量之前被搜索。
    -   `rpath` 在程序运行时总是固定的，除非重新链接程序，否则无法更改。

-   **runpath**:
    -   `runpath` 是 `rpath` 的一个更新版本，具有类似的功能。
    -   与 `rpath` 不同的是，如果设置了 `LD_LIBRARY_PATH` 环境变量，`LD_LIBRARY_PATH` 会在 `runpath` 之前被搜索。
    -   这使得 `runpath` 在某些情况下更灵活，因为可以通过设置 `LD_LIBRARY_PATH` 覆盖 `runpath`。

### 使用示例

下面是如何在编译时为一个程序指定 `rpath` 或 `runpath` 的示例：

#### 设置 rpath

假设你有一个程序 `myapp`，依赖于位于 `/home/user/mylibs` 的共享库。你可以在编译时使用以下命令设置 `rpath`：

```bash
gcc -o myapp myapp.c -L/home/user/mylibs -Wl,-rpath,/home/user/mylibs -lmylib
```

这里：

-   `-L/home/user/mylibs` 指定编译时链接器的搜索路径。
-   `-Wl,-rpath,/home/user/mylibs` 指定运行时的库搜索路径。
-   `-lmylib` 指定要链接的库。

#### 设置 runpath

使用 `runpath` 而不是 `rpath` 可以这样做：

```bash
gcc -o myapp myapp.c -L/home/user/mylibs -Wl,-rpath=/home/user/mylibs -Wl,--enable-new-dtags -lmylib
```

这里的 `--enable-new-dtags` 选项告诉链接器使用 `runpath` 而不是 `rpath`。

### 注意事项

-   **安全性**：使用 `rpath` 或 `runpath` 可以使得应用程序的部署更加灵活和独立，但是如果库的位置包含不受信任的库，可能会有安全风险。
-   **维护性**：虽然这种方法可以减少对环境变量的依赖，但是当库位置变更时，可能需要重新编译应用。
-   **可移植性**：使用硬编码的库路径会影响应用的可移植性，应当谨慎使用这些路径，特别是在开发通用软件或发行包时。

`rpath` 和 `runpath` 提供了一种有效的方法来管理应用程序的依赖关系，但它们应当在了解其潜在影响的情况下使用。
