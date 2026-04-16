# 66. Review / Approval / ReleasePackage 对象系统

## 这篇文档回答什么问题

对象系统进入治理层之后，最关键的一组对象就是 `ReviewRound`、`ApprovalRequest` 和 `ReleasePackage`。

如果没有这组三段式对象流，平台很快会陷入这些问题：

- 大家都在提意见，但不知道意见到底针对哪一版
- 有人说“可以了”，但没人知道是不是正式批准
- 交付物已经导出，但系统说不清它是不是正式出厂包

本篇重点回答：

1. 为什么治理层必须围绕 review、approval 和 release package 建模。
2. 这三类对象之间如何形成正式治理链。
3. Hermes Agent 应如何围绕它们支持审批、交付和归档。

---

## 一、为什么治理对象不能只是评论和文件夹

评论可以表达意见，文件夹可以装文件，但它们都无法天然表达正式治理状态。

```mermaid
flowchart LR
    A["Loose Comments"] --> C["No Formal Decision State"]
    B["Loose Export Files"] --> D["No Formal Delivery Boundary"]
    E["Governance Objects"] --> F["Reviewable / Approvable / Releasable"]
```

治理层真正要解决的是“状态合法性”。

---

## 二、三类对象的定位

### `ReviewRound`

回答“当前版本经历了什么评审，发现了什么问题”

### `ApprovalRequest`

回答“谁在申请批准什么对象进入下一状态”

### `ReleasePackage`

回答“哪些已经批准的对象共同构成正式交付包”

```mermaid
flowchart LR
    A["Versioned Object"] --> B["ReviewRound"]
    B --> C["ApprovalRequest"]
    C --> D["ReleasePackage"]
```

---

## 三、ReviewRound 应承载什么

`ReviewRound` 是治理链的第一道正式入口。

### 建议字段

- `review_id`
- `target_object_type`
- `target_object_id`
- `target_version_id`
- `reviewers`
- `findings`
- `recommendation`
- `status`

```mermaid
flowchart TD
    A["ReviewRound"] --> B["Target Object / Version"]
    A --> C["Reviewers"]
    A --> D["Findings"]
    A --> E["Recommendation"]
    A --> F["Status"]
```

---

## 四、ApprovalRequest 应承载什么

`ApprovalRequest` 不应该等同于“请看一下”，而是正式状态跃迁的申请对象。

### 建议字段

- `approval_id`
- `source_review_id`
- `target_object_type`
- `target_object_id`
- `requested_transition`
- `approvers`
- `decision`
- `decision_notes`
- `status`

```mermaid
flowchart LR
    A["ApprovalRequest"] --> B["Target Transition"]
    A --> C["Approvers"]
    A --> D["Decision / Notes"]
    A --> E["Status"]
```

---

## 五、ReleasePackage 应承载什么

`ReleasePackage` 是治理链的末端正式对象。

### 建议字段

- `package_id`
- `package_scope`
- `approved_object_refs`
- `delivery_files`
- `approval_history`
- `release_notes`
- `archive_pointer`
- `status`

```mermaid
flowchart LR
    A["ReleasePackage"] --> B["Approved Object Refs"]
    A --> C["Delivery Files"]
    A --> D["Approval History"]
    A --> E["Archive Pointer / Status"]
```

---

## 六、三者之间的正式关系

```mermaid
erDiagram
    ReviewRound ||--o{ ApprovalRequest : informs
    ApprovalRequest ||--o{ ReleasePackage : qualifies
    ReleasePackage }o--o{ VersionRecord : bundles
```

这里的关键是：

- review 负责提出结构化判断
- approval 负责状态跃迁
- release package 负责把被批准对象打包成正式交付体

---

## 七、为什么 approval 必须独立于 review

很多团队会把“review 通过了”直接等同于“批准了”，但这两者并不相同。

```mermaid
flowchart TD
    A["Review = Evaluate"] --> C["May Recommend Approval"]
    B["Approval = Authorize Transition"] --> D["Formal State Change"]
```

一个对象可以：

- review 通过，但仍待更高层批准
- review 没问题，但因外部约束暂不批准
- review 部分通过，只批准进入下一轮候选状态

---

## 八、治理对象的状态流

```mermaid
stateDiagram-v2
    [*] --> ReviewOpen
    ReviewOpen --> ReviewClosed
    ReviewClosed --> ApprovalPending
    ApprovalPending --> Approved
    ApprovalPending --> Rejected
    Approved --> Packaged
    Packaged --> Archived
```

这个状态机的价值在于：让系统知道“当前只是被讨论过”，还是“已经被正式授权出厂”。

---

## 九、典型协作时序

```mermaid
sequenceDiagram
    participant O as Target Object
    participant R as Review Layer
    participant A as Approval Layer
    participant P as Packaging Layer
    participant G as Archive Layer

    O->>R: 提交具体版本进入 review
    R-->>A: 输出 findings 与 recommendation
    A-->>O: 批准 / 驳回 / 退回修改
    A->>P: 批准进入 package
    P->>G: 生成 release package 并归档
```

---

## 十、在 Hermes Agent 中的映射建议

治理对象系统应成为 Hermes 电影化治理层的正式入口。

```mermaid
flowchart TD
    A["Versioned Artifacts / Objects"] --> B["ReviewRound"]
    B --> C["ApprovalRequest"]
    C --> D["ReleasePackage"]
    D --> E["Archive / Retrospective / Delivery"]
```

### 工程建议

- 所有 review 必须绑定具体对象和版本
- approval 必须记录状态跃迁意图
- release package 引用一组已批准对象，而不是裸文件
- archive 指针作为治理链末端标准字段

---

## 十一、MVP 设计建议

第一版先确保：

1. `ReviewRound` 可绑定对象版本
2. `ApprovalRequest` 可驱动正式状态变更
3. `ReleasePackage` 可引用已批准对象和文件
4. 审批链可追溯

```mermaid
flowchart LR
    A["Governance MVP"] --> B["ReviewRound"]
    A --> C["ApprovalRequest"]
    A --> D["ReleasePackage"]
    A --> E["Traceable State Changes"]
```

---

## 十二、结论

`ReviewRound`、`ApprovalRequest` 和 `ReleasePackage` 共同构成导演平台的治理对象主链。

它们分别回答：

- 评审发生了什么
- 谁批准了什么状态跃迁
- 哪些对象构成正式交付包

只有把这条对象链做实，平台才真正具备正式治理、正式交付和正式归档的能力。

---

## 相关文档

- [49-review-flow-versioning-and-release-package.md](./49-review-flow-versioning-and-release-package.md)
- [68-approval-and-escalation-flow-design.md](./68-approval-and-escalation-flow-design.md)
- [70-artifact-version-and-archive-system.md](./70-artifact-version-and-archive-system.md)
- [16-b-interfaces-and-data-contracts.md](./16-b-interfaces-and-data-contracts.md)
- [75-movie-tools-design.md](./75-movie-tools-design.md)
