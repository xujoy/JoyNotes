` ExternalizedCheckpointCleanup`

- **ExternalizedCheckpointCleanup.RETAIN_ON_CANCELLATION: 当作业被取消时，保留外部的checkpoint。注意，在此情况下，您必须手动清理checkpoint状态。
- **ExternalizedCheckpointCleanup.DELETE_ON_CANCELLATION**: 当作业被取消时，删除外部化的checkpoint。只有当作业失败时，检查点状态才可用。



