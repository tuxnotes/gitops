# 1 workspace overview
workspace运行task声明一块文件系统，在task运行时给taskrun使用。这块文件系统有多种使用方式:
- 用作只读的secret或configmap
- 存在PVC在多个task间共享数据
- 从VolumeClaimTemplate创建PVC
- emptyDir

workspace与volume类，但是可以推迟到用户或taskrun运行时决定使用哪种storage class

workspace使用目的如下：
- 输入输出的存储
- task间共享数据
- 为Secret中存储的凭证提供挂载点
- 为ConfigMap中存储的数据提供挂载点
- 为共享的通用工具提供挂载点
- 作为artifact的构建缓存，加速任务

# 2 在Task和TaskRun中使用workspace
Task给其steps指定workspace在磁盘上的位置。在运行时，TaskRun提供挂载到workspace的volume的详细信息。

这种方式提供了更多的灵活性。如在孤立的task中，TaskRun使用emptyDir就可以了。但对于复杂的系统，TaskRun肯能就需要PVC了，并在提前填充Task需要处理的数据。对于这两种场景，workspace的声明方式是一样的，仅在TaskRun运行时发生改变。

Task还可以与其Sidecar共享workspace。但需要一点配置，设计添加需要的`volumeMount`。这允许长期运行的Sidecar与Task中执行的Step共享数据。
