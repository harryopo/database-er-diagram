# 数据库ER图示例文件

本目录包含完整的数据库ER图示例，展示如何使用本 Skill 生成符合 Oracle Academy 标准的 ER 图。

---

## 示例：宝贝在家宠物照管平台

### 文件
- [`宝贝在家-ER图.drawio`](./宝贝在家-ER图.drawio) - 完整的 9 实体 + 12 关系示例

### 项目背景
这是一个宠物照管平台的数据库概念模型，包含用户管理、宠托师管理、照料任务、订单、评价等核心业务流程。

### 实体清单（9个）

| 编号 | 实体 | 类型 | 颜色 | 属性数 |
|------|------|------|------|--------|
| 1 | USER（用户） |  核心实体 | 蓝色 | 8 |
| 2 | USER_ADDRESS（用户地址） | 🟢 次要实体 | 绿色 | 4 |
| 3 | CARE_PROVIDER（宠托师） | 🟡 角色实体 | 黄色 | 8 |
| 4 | PET（宠物） | 🟣 业务实体 | 紫色 | 8 |
| 5 | CARE_TASK（照料任务） | 🔵 核心实体 | 蓝色 | 10 |
| 6 | TASK_PET（任务-宠物关联） |  交集实体 | 橙色 | 2 |
| 7 | ORDER（订单） | 🔵 核心实体 | 蓝色 | 7 |
| 8 | REVIEW（评价） | 🔴 结果实体 | 红色 | 4 |
| 9 | EXCEPTION_EVENT（异常事件） | ⚫ 日志实体 | 灰色 | 5 |

### 关系清单（12条）

| # | 实体A | 关系名 | 实体B | A端 | B端 | NT |
|---|-------|--------|-------|-----|-----|----|
| R1 | USER | has/属于 | USER_ADDRESS | MANY+MAY | ONE+MUST | - |
| R2 | USER | becomes/属于 | CARE_PROVIDER | ONE+MAY | ONE+MUST | - |
| R3 | USER | owns/被拥有 | PET | MANY+MAY | ONE+MUST | ✓ |
| R4 | USER | publishes/由...发布 | CARE_TASK | MANY+MAY | ONE+MUST | ✓ |
| R5 | CARE_PROVIDER | recommends | CARE_PROVIDER | MANY+MAY | MANY+MAY | - |
| R6 | CARE_TASK | generates/收录 | ORDER | MANY+MAY | ONE+MUST | ✓ |
| R7 | CARE_PROVIDER | accepts/提供 | ORDER | MANY+MAY | ONE+MUST | ✓ |
| R8 | ORDER | produces/源自 | REVIEW | MANY+MAY | ONE+MUST | ✓ |
| R9 | ORDER | triggers/由...触发 | EXCEPTION_EVENT | MANY+MAY | ONE+MUST | - |
| R10 | CARE_TASK | involves/属于任务 | TASK_PET | - | - | - |
| R11 | PET | assigned to/涉及宠物 | TASK_PET | - | - | - |
| R12 | CARE_TASK | served at/serves | USER_ADDRESS | ONE+MUST | MANY+MAY | - |

> **说明：**
> - R3/R4/R6/R7/R8 是 NT（不可转移）关系，使用橙色空心菱形标记
> - R5 是递归关系（宠托师之间的推荐关系）
> - R10+R11 是交集实体 TASK_PET，解析 CARE_TASK 和 PET 的 M:N 关系

### 布局策略
采用**三列紧凑布局**，零交叉：
- **左列**：USER → USER_ADDRESS
- **中列**：CARE_PROVIDER → CARE_TASK → ORDER → EXCEPTION_EVENT
- **右列**：PET → TASK_PET → REVIEW

### 查看方式
1. 在 VS Code 中安装 `hediet.vscode-drawio` 插件
2. 双击打开 `.drawio` 文件
3. 可以预览、编辑、导出为 PNG/SVG/PDF

---

## 如何使用示例

### 学习参考
打开示例文件，查看：
- 实体容器的样式和属性标记（`#`/`*`/`o`）
- 关系连线的颜色规则（绿色=必须/粉色=可选）
- NT 菱形的 3 段 edge 实现
- 交集实体的连线方式
- 中点作为实体子元素的实现

### 快速开始
1. 复制示例文件作为新项目的基础
2. 修改实体名称和属性
3. 添加/删除关系连线
4. 调整布局

---

*本示例遵循 Oracle Academy Database Foundations 教材规范*
