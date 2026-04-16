# 48. VFX 后期协作与交付

## 这篇文档回答什么问题

VFX 在后期最容易失控的地方，不是单个镜头难，而是它天然横跨：

- 前期规划
- 现场拍摄纪律
- 后期版本变更
- 最终交付验收

本篇重点回答：

1. VFX 后期协作在传统电影项目里是怎么运作的。
2. 为什么 VFX 是最需要正式任务对象、状态流和交付标准的一条后期链。
3. 在导演智能体平台里，VFX post collaboration 与 delivery 应如何对象化和治理化。

---

## 一、VFX 后期不是“最后做特效”，而是跨阶段长链

现实里 VFX 往往从前期就开始定义，到后期才进入集中交付。

```mermaid
flowchart LR
    A["Previs / VFX Requirement"] --> B["On-set Capture Discipline"]
    B --> C["Post VFX Shot Work"]
    C --> D["Review / Iteration"]
    D --> E["Final Delivery to Conform / Grade"]
```

所以 VFX 在平台里不应该只出现于后期文档，而应当贯穿全流程；这里只聚焦 post 阶段的集中协作与交付。

---

## 二、传统 VFX 后期协作通常怎么走

```mermaid
flowchart TD
    A["Locked Cut / Conformed Shots"] --> B["VFX Shot Breakdown"]
    B --> C["Task Assignment / Vendor or Team Work"]
    C --> D["WIP Review"]
    D --> E["Notes / Revision"]
    E --> F["Final Shot Delivery"]
    F --> G["Back to Edit / Grade / Master"]
```

这说明 VFX 天生就是：

- shot-based
- note-driven
- delivery-driven

的系统。

---

## 三、为什么 VFX 特别容易失控

### 1. 与 cut version 高度耦合

镜头版本、时长、帧范围一变，VFX 就要重跟。

### 2. 与 on-set capture 强耦合

如果现场 reference、tracking、clean plate 不完整，后期难度会暴涨。

### 3. review 往往迭代次数多

而且反馈既可能来自导演，也可能来自制片、摄影或后期 supervisor。

```mermaid
flowchart LR
    A["Cut Change"] --> D["VFX Rework"]
    B["On-set Capture Gap"] --> D
    C["Multiple Review Sources"] --> D
```

---

## 四、VFX 后期真正需要管理的是什么

不是“有没有做”，而是：

- shot status
- dependency status
- review notes
- final delivery acceptance

```mermaid
flowchart TD
    A["VFX Shot"] --> B["Status"]
    A --> C["Dependencies"]
    A --> D["Notes History"]
    A --> E["Delivery Acceptance"]
```

---

## 五、在平台中的对象映射建议

建议至少建模：

- `VFXShot`
- `VFXTaskPackage`
- `VFXReviewRound`
- `VFXDependency`
- `VFXDelivery`

```mermaid
flowchart TD
    A["Cut / ShotPlan / On-set Data"] --> B["VFXShot"]
    B --> C["VFXTaskPackage"]
    C --> D["VFXReviewRound"]
    D --> E["VFXDelivery"]
    B --> F["VFXDependency"]
```

### 建议字段

#### `VFXShot`

- `shot_id`
- `cut_version_id`
- `vfx_type`
- `capture_readiness`
- `status`
- `delivery_due`

#### `VFXDelivery`

- `vfx_shot_id`
- `delivery_version`
- `delivery_status`
- `accepted_by`
- `integration_notes`

---

## 六、平台里的 VFX 工作流建议

```mermaid
sequenceDiagram
    participant C as Locked / Conformed Cut
    participant V as VFX Layer
    participant D as Director / Post Supervisor
    participant R as VFX Review
    participant G as Edit / Grade / Master Pipeline

    C->>V: 提供镜头基线与帧范围
    V-->>D: 输出 VFX shot work / WIP
    D->>R: 发起 VFX review
    R-->>V: 返回 notes
    V-->>G: 交付 final shot
    G-->>D: 集成到 edit / grade / master
```

---

## 七、为什么 VFX 需要显式状态机

VFX 最大的问题之一，就是镜头数量多、依赖复杂，如果没有清晰状态，团队很快就会失控。

```mermaid
stateDiagram-v2
    [*] --> not_started
    not_started --> in_progress
    in_progress --> in_review
    in_review --> notes_required
    notes_required --> in_progress
    in_review --> approved
    approved --> delivered
```

---

## 八、为什么 VFX 交付必须和后续环节联动

一个 VFX 镜头“交付了”不等于项目结束，它还要进入：

- 编辑复核
- 调色
- 最终 master

```mermaid
flowchart LR
    A["VFX Delivery"] --> B["Editorial Confirm"]
    B --> C["Color Pipeline"]
    C --> D["Final Master"]
```

所以 VFX delivery 是中间交付，不是终点。

---

## 九、对导演智能体平台和 Hermes 的启发

对平台而言，VFX 后期协作最值得优先补的是：

- `VFXShot` 状态流
- review note 与 dependency 管理
- final delivery acceptance
- 与 cut、grade、release 的联动

对 Hermes 来说，后续可补的能力包括：

- VFX shot registry
- note-driven review artifact
- 与 on-set capture、cut version、release package 的跨阶段连接

---

## 十、结论

VFX 后期协作与交付，在电影项目里本质上是一条跨阶段、跨版本、跨 review 的长链系统。

在导演智能体平台里，它应被理解成：

- shot-based 的正式对象群
- 高依赖 review、状态流和交付验收的治理链
- 从现场 capture 一直连接到 final master 的关键中间层

只有把 VFX 从“做特效任务”升级成正式状态和交付系统，后期制作才真正能在复杂镜头上保持可控。

---

## 相关文档

- [43-on-set-collaboration-camera-light-sound-vfx.md](./43-on-set-collaboration-camera-light-sound-vfx.md)
- [45-editing-workflow-and-versioning.md](./45-editing-workflow-and-versioning.md)
- [47-color-grading-and-visual-consistency.md](./47-color-grading-and-visual-consistency.md)
- [49-review-flow-versioning-and-release-package.md](./49-review-flow-versioning-and-release-package.md)
- [70-artifact-version-and-archive-system.md](./70-artifact-version-and-archive-system.md)
