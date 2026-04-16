# 04. 后期：剪辑、声音、调色、版本管理、宣发与项目复盘

## 1. 后期目标

后期阶段的导演智能体，要从“现场调度中枢”切换为“版本与品质中枢”。

主要目标：

- 快速形成可审看版本
- 控制剪辑、声音、音乐、调色、视效的协同节奏
- 保证版本管理清晰
- 为宣发和交付提供标准化资产
- 在项目结束后完成复盘与知识回收

```mermaid
timeline
    title 后期主线时间轴
    前期交接
      : 素材和元数据入库
      : 锁定风格包与补拍清单
    剪辑阶段
      : 粗剪
      : 精剪
      : 导演版审看
    声画完善
      : ADR/配乐/音效
      : 调色
      : VFX 终版
    交付发行
      : 审核版
      : 发行版
      : 宣发资产
    项目复盘
      : 经验沉淀
      : 方法库更新
```

## 2. 后期子智能体

- 剪辑智能体：粗剪、精剪建议、结构问题诊断、节奏建议
- ADR/配音智能体：补录台词点位、口型匹配、替换台词建议
- 配乐智能体：音乐情绪设计、段落功能、 temp track 管理
- 音效智能体：环境声、拟音、冲击点、空间层次建议
- 调色智能体：镜头一致性、场景色彩曲线、风格 LUT 策略
- 视效交付智能体：镜头状态追踪、版本审核、返工记录
- 版本管理智能体：版本号、审批链、发布包和归档
- 宣发智能体：预告片、海报 brief、短视频切条、平台适配

## 3. 后期核心工件

- `edit/`：粗剪、精剪、导演剪辑版、发行版说明
- `sound/`：ADR 清单、配乐方案、音效清单、混音意见
- `grade/`：调色参考、look bible、镜头一致性报告
- `vfx_delivery/`：VFX 镜头状态、版本记录、供应商反馈
- `release/`：送审版、发行版、平台版、字幕版、预告版
- `marketing/`：宣传语、预告结构、花絮、角色物料
- `retrospective/`：项目复盘、经验卡片、风险案例、最佳实践

## 4. 剪辑与导演协同

导演智能体在剪辑阶段应重点管理：

- 每场戏是否仍然服务主题
- 结构节奏是否拖沓
- 情绪转场是否足够平滑
- 演员最佳 take 是否被用对
- 是否存在补拍比重高于重剪的段落

建议系统输出三类剪辑建议：

- 结构级：删场、调序、并场
- 场次级：缩短、延长、换 take
- 镜头级：起止点、反应镜头、空镜、桥接镜头

```mermaid
flowchart TD
    A["粗剪版本"] --> B["剪辑智能体诊断结构问题"]
    B --> C["导演智能体判断主题和节奏"]
    C --> D{"问题类型"}
    D -- 结构级 --> E["删场/调序/并场"]
    D -- 场次级 --> F["缩短/延长/换 take"]
    D -- 镜头级 --> G["起止点/反应镜头/桥接镜头"]
    E --> H["形成新审看版"]
    F --> H
    G --> H
```

### 4.1 后期泳道图

```mermaid
flowchart LR
    subgraph Q1["导演总控"]
        A1["审看目标定义"]
        A2["节奏与主题判断"]
        A3["版本签核"]
        A4["复盘结论"]
    end

    subgraph Q2["剪辑部门"]
        B1["粗剪"]
        B2["精剪"]
        B3["版本比较"]
    end

    subgraph Q3["声音部门"]
        C1["ADR 清单"]
        C2["配乐设计"]
        C3["音效与混音"]
    end

    subgraph Q4["调色 / VFX"]
        D1["调色策略"]
        D2["VFX 终版跟踪"]
        D3["视觉一致性检查"]
    end

    subgraph Q5["版本 / 宣发 / 归档"]
        E1["审核版管理"]
        E2["发行版打包"]
        E3["宣发资产适配"]
        E4["项目复盘归档"]
    end

    A1 --> B1
    B1 --> B2
    B2 --> A2
    A2 --> B3
    B3 --> C1
    C1 --> C2
    C2 --> C3
    B3 --> D1
    D1 --> D2
    D2 --> D3
    C3 --> E1
    D3 --> E1
    E1 --> A3
    A3 --> E2
    A3 --> E3
    E2 --> E4
    E3 --> E4
    E4 --> A4
```

## 5. 配音、配乐、音效

声音系统不能只看单点，而要保持整体叙事体验：

- ADR 智能体负责识别台词不清、信息不足、表演不一致
- 配乐智能体负责段落情绪弧线与主题动机
- 音效智能体负责空间感、冲击力、环境真实感

导演智能体在这里要维护的是“声音风格规则集”：

- 哪些场景留白
- 哪些场景让音乐主导
- 哪些场景用环境声而不是台词解释

## 6. 调色与视觉统一

调色阶段建议把前期锁定的风格包重新激活为审片基线：

- 色温区间
- 对比度倾向
- 黑位控制
- 饱和度风格
- 日夜戏视觉统一原则
- 特效镜头与实拍镜头的视觉缝合标准

## 7. 版本管理

后期最怕版本混乱，因此需要明确对象模型：

- 项目版本
- 影片版本
- 场次版本
- 镜头版本
- VFX 版本
- 声音版本
- 字幕版本
- 宣发版本

每个版本都应记录：

- 来源
- 修改原因
- 审批人
- 与上一版差异
- 是否可发布

```mermaid
gitGraph
    commit id: "Assembly"
    commit id: "Rough Cut"
    branch director
    checkout director
    commit id: "Director Cut"
    checkout main
    merge director
    branch producer
    checkout producer
    commit id: "Producer Notes"
    checkout main
    merge producer
    commit id: "Approval Cut"
    branch release
    checkout release
    commit id: "Theatrical"
    commit id: "Streaming"
```

## 8. 审核与出片

“出片、审核”不应是一次性动作，而应形成流水线：

1. 内部工作版
2. 导演审看版
3. 制片审看版
4. 出品/平台/客户审看版
5. 送审版
6. 发行版

每次审核都输出：

- 问题清单
- 必改项
- 可选优化项
- 风险说明
- 截止时间

```mermaid
requirementDiagram
    requirement review_traceability {
      id: R1
      text: "每个审看版本都必须有问题清单与责任人"
      risk: high
      verifymethod: test
    }
    requirement release_gate {
      id: R2
      text: "发行版必须满足审批完成和版本可回溯"
      risk: high
      verifymethod: inspection
    }
    requirement promo_alignment {
      id: R3
      text: "宣发物料必须与最终发行风格一致"
      risk: medium
      verifymethod: analysis
    }
    element version_system {
      type: "module"
      docRef: "version-management"
    }
    element approval_flow {
      type: "module"
      docRef: "review-pipeline"
    }
    element marketing_pipeline {
      type: "module"
      docRef: "marketing-assets"
    }
    version_system - satisfies -> review_traceability
    approval_flow - satisfies -> release_gate
    marketing_pipeline - satisfies -> promo_alignment
```

## 9. 宣发协同

导演智能体在后期还可以把影片语言转为宣传语言：

- 预告片结构建议
- 海报和主视觉 brief
- 角色短视频拆解
- 社交平台文案与素材切条
- 幕后花絮主题设计

## 10. 项目复盘与总结

项目结束后，系统必须自动产出复盘：

- 哪些场次超时、超支
- 哪些创意决策最有效
- 哪些演员/场地/部门协作最稳定
- 哪些提示词和风格规则可复用
- 哪些流程需要在下一项目中前移

最终形成两层知识资产：

- 可跨项目复用的方法库
- 本项目独有的经验库和风险库

## 10.1 后期汇报总图

```mermaid
flowchart TD
    I0["后期输入<br/>已拍素材 / 锁定风格包 / 补拍清单 / 审核要求"] --> D0["导演总控智能体"]

    D0 --> E0["剪辑智能体<br/>粗剪 / 精剪 / 节奏诊断 / take 建议"]
    D0 --> S0["声音智能体<br/>ADR / 配乐 / 音效 / 混音建议"]
    D0 --> G0["调色智能体<br/>look bible / 一致性 / LUT 策略"]
    D0 --> V0["VFX 交付智能体<br/>镜头状态 / 返工 / 终版追踪"]
    D0 --> M0["版本与宣发智能体<br/>审核链 / 发行包 / 海报预告 brief"]

    E0 --> O1["画面版本流"]
    S0 --> O2["声音版本流"]
    G0 --> O3["调色版本流"]
    V0 --> O4["VFX 版本流"]
    M0 --> O5["发布与宣发流"]

    O1 --> C0["统一版本中枢<br/>差异说明 / 审批记录 / 可发布判断"]
    O2 --> C0
    O3 --> C0
    O4 --> C0
    O5 --> C0

    C0 --> R1["输出 1<br/>导演审看版 / 制片审看版 / 送审版 / 发行版"]
    C0 --> R2["输出 2<br/>预告 / 海报 / 短视频 / 平台物料"]
    C0 --> R3["输出 3<br/>复盘报告 / 方法库 / 风险库"]
```
