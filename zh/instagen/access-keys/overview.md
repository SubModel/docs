# Access Key 概述

用于安全访问 InstaGen API 的认证 token。

## 核心特性

* **安全认证**:加密的 token
* **用量跟踪**:按 key 监控消耗
* **多个 key**:最多可创建至允许上限
* **key 管理**:启用/禁用、编辑、删除

## key 信息

每个 key 会显示:

* **名称(Name)**:用户自定义标识
* **token(Token)**:access key(只读)
* **状态(Status)**:活动或已禁用
* **描述(Description)**:可选备注
* **最近使用(Last Used)**:最近一次使用时间
* **使用次数(Usage Count)**:总请求数

## 安全最佳实践

* 妥善保管 key,注意保密
* 不要在公开仓库中共享
* 为不同应用使用不同的 key
* 定期轮换 key

## 后续步骤

* [创建你的第一个 key](create-key.md)
* [了解 key 管理](manage-keys.md)
