# 需求基线：并发任务测试

## 来源

| 项 | 值 |
|---|---|
| 原始来源 | 本轮用户请求（IssueTask 指令，业务任务「并发任务测试」） |
| 来源状态 | 本轮输入原文，无独立原始需求文件 |

本次任务的原始需求即用户请求正文：以「并发任务测试」为业务任务，指定连续阶段范围 requirements，并给出阶段执行、写范围与发布纪律。请求原文完整表达了验收框架，无需补充外部文档。

## 目标应用

| 项 | 值 |
|---|---|
| 应用名称 | Lingee Build 平台 |
| 应用编码 | lingee-build |
| 确认来源 | 本轮用户请求：平台级并发任务执行验证，未指定苍穹应用 |

本需求不是苍穹应用元数据交付：请求不包含表单、单据、字段等苍穹业务对象，也不含目标苍穹应用信息。被测对象为 Lingee Build 平台的任务并发执行机制，目标应用按被测对象如实记录。若后续确认需要绑定具体苍穹应用，应补充该应用的完整名称与应用编码，并沿共享 revise 命令修订本基线。

## 业务目标

并发任务测试：在多个任务（IssueTask）并发执行的场景下，验证每个任务按其授权的连续阶段范围独立完成，执行事实（进度、回执、产物）按任务隔离保存，并按发布纪律交付。

- 角色：任务发起人（用户）通过 IssueTask 派发任务；执行侧在各自独立 Session / 工作树中完成授权阶段。
- 情境：多个任务同时执行；每个任务拥有各自的 WorkItem、授权写范围与阶段授权，本任务为其中之一。
- 结果：本任务在 requirements 阶段内产出通过校验的需求基线，完成授权范围后停止。

## 范围与约束

1. 阶段范围：本任务仅执行授权的连续阶段范围 requirements；完成该阶段后停止，不执行范围之外的任何阶段。
2. 写范围隔离：源码与运行时产物仅写入本 WorkItem 的 docs/wi-t1000127 与 .expert-team/work-items/wi-t1000127；不修改其他 WorkItem、未声明的共享构建产物，不切换项目 Git HEAD、不使用共享 Git index 提交，不占用其他任务的端口或部署环境。
3. 发布纪律：阶段进度和回执只作为本地执行事实保存；只有全部阶段成功后，才发布一组最终产物和一个最终 WorkItem 快照。

## 验收条件

| AC | 前置 / 动作 | 可观察结果 |
|---|---|---|
| AC-01 | 已进入 requirements 阶段，撰写并提交需求基线 | 产出 docs/wi-t1000127/requirements.md，并通过产品基线校验（校验结果为通过） |
| AC-02 | requirements 阶段完成 | 不再发起或执行范围外阶段的操作，任务按授权范围停止 |
| AC-03 | 任务执行期间（含并发执行期间） | 本任务没有写操作落在授权范围之外：其他 WorkItem 与共享文件未被本任务修改 |
| AC-04 | 阶段执行与完成过程中 | 阶段回执仅作为本地执行事实记录；最终产物与最终 WorkItem 快照仅在全部阶段成功后发布 |

## 待决事项

无阻塞性待决事项。业务范围、执行约束与验收口径均来自用户请求原文。目标应用判定为 Lingee Build 平台（平台级验证）依据是请求原文未包含任何苍穹应用信息；如后续需要绑定具体苍穹应用，补充应用名称、编码与确认来源后修订本基线。

## 需求基线索引

```json
{
  "schemaVersion": "cosmic-requirements-baseline/v1",
  "revision": 1,
  "application": {
    "name": "Lingee Build 平台",
    "code": "lingee-build",
    "reference": "本轮用户请求：IssueTask 业务任务「并发任务测试」，平台级验证，未指定苍穹应用"
  },
  "items": [
    {
      "id": "REQ-01",
      "kind": "requirement",
      "description": "并发任务测试：多个任务并发执行时，各任务按授权的连续阶段范围独立完成；本任务完成 requirements 阶段并产出通过校验的需求基线。",
      "source": {
        "kind": "user-request",
        "reference": "用户请求：业务任务「并发任务测试」与阶段范围「requirements」"
      },
      "status": "confirmed",
      "acceptanceIds": ["AC-01"]
    },
    {
      "id": "CON-01",
      "kind": "constraint",
      "description": "仅执行授权的连续阶段范围 requirements；完成后停止，不执行范围外阶段。",
      "source": {
        "kind": "user-request",
        "reference": "用户请求：「按指定的连续阶段范围执行：requirements」「不得执行此范围之外的阶段；完成 requirements 后停止」"
      },
      "status": "confirmed",
      "acceptanceIds": ["AC-02"]
    },
    {
      "id": "CON-02",
      "kind": "constraint",
      "description": "写操作仅限本 WorkItem 的 docs/wi-t1000127 与 .expert-team/work-items/wi-t1000127，不修改其他 WorkItem 或共享产物，不切换 Git HEAD、不占用其他任务的端口或部署环境。",
      "source": {
        "kind": "user-request",
        "reference": "用户请求：源码写范围声明与「不得修改其他 WorkItem」等隔离要求"
      },
      "status": "confirmed",
      "acceptanceIds": ["AC-03"]
    },
    {
      "id": "CON-03",
      "kind": "constraint",
      "description": "阶段进度和回执只作为本地执行事实保存；只有全部阶段成功后，才发布一组最终产物和一个最终 WorkItem 快照。",
      "source": {
        "kind": "user-request",
        "reference": "用户请求：「阶段进度和回执只作为本地执行事实保存。只有全部阶段成功后，才发布一组最终产物和一个最终 WorkItem 快照。」"
      },
      "status": "confirmed",
      "acceptanceIds": ["AC-04"]
    }
  ]
}
```
