`sestatus` 命令是一个用于查看当前 SELinux (Security-Enhanced Linux) 的状态和政策详细信息的实用工具。这个命令提供了一个简洁的方式来确定 SELinux 是否被启用、它的当前模式，以及正在使用的策略类型等信息。

### 命令格式

`sestatus` 没有复杂的参数或选项，通常只需要简单地在命令行中输入 `sestatus` 即可：

```bash
sestatus
```

### 输出解释

`sestatus` 的输出提供了几个关键信息，这些信息对于理解系统的 SELinux 配置非常重要：

-   **SELinux status**：显示 SELinux 是否启用。
-   **Current mode**：显示 SELinux 的当前模式，可能是 Enforcing、Permissive 或 Disabled。
    -   **Enforcing**：SELinux 策略被强制执行，任何违反策略的行为都将被阻止。
    -   **Permissive**：SELinux 策略不会被强制执行，违反策略的行为不会被阻止，但会被记录。
    -   **Disabled**：SELinux 完全禁用。
-   **Policy from config file**：显示从配置文件 `/etc/selinux/config` 中加载的策略类型。
-   **Policy version**：显示当前加载的 SELinux 策略的版本。
-   **Loaded policy name**：显示正在使用的 SELinux 策略的名称，如 targeted 或 mls。
-   **Policy booleans**：可选显示当前设置的布尔值及其状态。
-   **Policy MLS status**：显示多层安全（MLS）的启用状态，这是 SELinux 策略中的一个高级特性。

### 示例输出

运行 `sestatus` 命令的输出示例如下：

```plaintext
SELinux status:                 enabled
SELinuxfs mount:                /sys/fs/selinux
SELinux root directory:         /etc/selinux
Loaded policy name:             targeted
Current mode:                   enforcing
Mode from config file:          enforcing
Policy MLS status:              enabled
Policy deny_unknown status:     allowed
Memory protection checking:     actual (secure)
Max kernel policy version:      31
```

### 使用场景

`sestatus` 常用于以下场景：

-   **系统管理员**：在排查系统问题或进行安全审核时需要查看 SELinux 的状态和模式。
-   **脚本和自动化**：在自动化脚本中检查 SELinux 状态以适当调整操作。
-   **安全配置**：在进行系统安全配置或合规性检查时确认 SELinux 的配置和策略。

### 注意事项

-   确保你有适当的权限来运行 `sestatus`，尤其是在严格控制的环境中。
-   在调整 SELinux 配置或策略之前，建议了解当前的 SELinux 设置，以避免意外改变系统的安全行为。

通过 `sestatus`，你可以快速获得系统的 SELinux 配置和状态信息，这是维护和管理基于 SELinux 的系统安全的基础工具之一。
