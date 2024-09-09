# Runtime 配置

该配置项用于 `CPU Core` 与 `用户线程`、`后台线程`进行分离与绑定，以保障系统在高负载时前台线程与后台线程不会争抢 CPU 资源。在系统负载较高的场景建议配置该项。
```tips
后台线程包括不限于：compact、flush、gc、compact_active、compact_inactive

前台线程：除后台线程之外的其他线程
```

## 默认运行时配置

`runtime.default` 配置控制系统主要操作的 CPU 使用情况。

- **`cpu_cores`**:  
  定义分配给默认运行时的 CPU 核心数量。此设置允许灵活管理 CPU 资源。该值可以是绝对数量，或者为总 CPU 核心的百分比，或者为 `0` 表示不隔离该运行时的 CPU。  
  - `>= 1`：指定分配的绝对 CPU 核心数量。  
  - `0`：不为此运行时隔离 CPU，这意味着没有特定的核心专用于它。
  - `> 0 and < 1`：指定分配的总 CPU 核心百分比。例如，`0.2` 表示分配 20% 的所有 CPU 核心。
  - **默认值**: `0.0` （无 CPU 隔离）。

## 后台运行时配置

`runtime.background` 配置管理与主要操作同时运行的后台任务的 CPU 资源分配。

- **`cpu_cores`**:  
  控制分配给后台任务的 CPU 核心数量。该值与 `runtime.default` 设置遵循相同的规则：  
  - `>= 1`：后台任务的绝对 CPU 核心数量。  
  - `0`：后台任务不隔离 CPU。  
  - `> 0 and < 1`：分配给后台任务的总 CPU 核心百分比。  
  - **默认值**: `0.0` （无 CPU 隔离）。

```tips
默认配置，前后台线程共享所有 CPU Core.

所有运行配置中的 `cpu_cores` 累加不能超过系统总的 CPU 核心数量。

如果一项运行配置中设置 `cpu_cores` 不等于 0，表示该项运行配置要绑定部分 CPU，此时另外的运行配置 `cpu_cores` 等于 0 表示会绑定到剩余的 CPU 上。
```