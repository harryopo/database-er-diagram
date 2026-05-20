***

name: "database-er-diagram"
description: "专用于生成数据库实体关系图（ER Diagram）的超强Skill，严格遵循Oracle Academy Database Foundations教材规范（Crow's Foot记法、#/\*标记体系、概念模型原则），支持NT不可转移菱形在线上放置、子元素NT菱形跟随实体移动、三列紧凑/两列模板零交叉布局、马卡龙色系（绿=实线=MUST/粉=虚线=MAY）、实虚线分段法、标准双向读法规则、子类型白色分隔线，适用于所有数据库ER图设计场景"
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

# 数据库ER图专用生成器（v12.0 · 完整模板版）

> **整合自：** Oracle Academy DFo 2-6/3-1/3-2 教材 + draw\.io Crow's Foot 专业实现 + 布局优化策略 + 颜色系统 + 项目实战经验，具体服务于深圳信息职业技术大学计算机应用技术专业数据库设计与实现课程
>
> **版本：** v12.0 · 完整模板版
>
> **适用场景：** 所有数据库ER图设计（概念模型），适用于教学、项目汇报、数据库课程设计

***

## 目录

- [零、输出原则](#零输出原则)
- [一、Oracle Academy 核心规范（约法7章）](#一oracle-academy-核心规范约法7章)
- [二、实体表示法（Draw.io 实现）](#二实体表示法drawio-实现)
- [三、关系连线规范](#三关系连线规范)
- [四、七种关系类型详解](#四七种关系类型详解)
- [五、不可转移关系（NT）菱形标记规范](#五不可转移关系nt菱形标记规范)
- [六、ER图布局优化策略](#六er图布局优化策略)
- [七、Draw.io 完整实现模板](#七drawio-完整实现模板)
- [八、反模式检查清单](#八反模式检查清单)
- [九、Draw.io 导出检查清单](#九drawio-导出检查清单)
- [十、完整绘制示例：宝贝在家平台](#十完整绘制示例宝贝在家平台)
- [十一、聊天侧输出模板](#十一聊天侧输出模板)
- [附录：教材参考与术语对照](#附录教材参考与术语对照)

***

## 零、输出原则

生成 ER 图时分为**两个通道**：

| 通道              | 位置         | 内容                                              |
| --------------- | ---------- | ----------------------------------------------- |
| **图纸（.drawio）** | VS Code 预览 | **仅包含：标题 + 实体 + 属性 + 关系连线**。不写任何说明面板、图例、脚注、来源引用 |
| **聊天（对话）**      | 终端文字       | 关系说明、术语对照、阅读示例、建模流程等所有解释性内容                     |

***

## 一、Oracle Academy 核心规范（约法7章）

### 第1章 实体（Entity）规范

```
实体 = 圆角矩形容器
实体名 = 全大写 · 单数名词（如 STUDENT、COURSE、DEPARTMENT）
属性区 = 实体名下方，逐行列出
```

- 实体用**圆角矩形**（`rounded=1;arcSize=10;`）
- 实体名称放在容器 `value` 中，全大写单数，括号注中文翻译
  - 例：`value="STUDENT（学生）"`
- 实体分六类（外加一类交集实体）：

| 类型   | 配色    | fillColor | strokeColor | 示例                      |
| ---- | ----- | --------- | ----------- | ----------------------- |
| 核心实体 | 🔵 蓝色 | #dae8fc   | #6c8ebf     | USER, ORDER, CARE\_TASK |
| 次要实体 | 🟢 绿色 | #d5e8d4   | #82b366     | USER\_ADDRESS           |
| 角色实体 | 🟡 黄色 | #fff2cc   | #d6b656     | CARE\_PROVIDER          |
| 业务实体 | 🟣 紫色 | #e1d5e7   | #9673a6     | PET                     |
| 结果实体 | 🔴 红色 | #f8cecc   | #b85450     | REVIEW                  |
| 日志实体 | ⚫ 灰色  | #f5f5f5   | #666666     | EXCEPTION\_EVENT        |
| 交集实体 | 🟠 橙色 | #ffe6cc   | #d79b00     | TASK\_PET（M:N解析）        |

### 第2章 属性（Attribute）标记体系（★★★★★）

**Oracle Academy 专用符号**（不是 fontStyle，而是前缀符号！）：

| 符号   | 含义                          | 示例                  |
| ---- | --------------------------- | ------------------- |
| `#`  | UID / 主键（Unique Identifier） | `# user_id（用户ID）`   |
| `*`  | 必填属性（Mandatory / Required）  | `* real_name（真实姓名）` |
| `o`  | 可选属性（Optional）              | `o wechat（微信号）`     |
| `#*` | 主键且必填（常见组合）                 | `#* order_id（订单ID）` |

- **绝不在"概念ER图"中画出外键字段！** 外键隐含在关系连线中
- 复合属性以缩进形式列在父属性下方
- 派生属性不包含在概念ERD中

### 第3章 关系（Relationship）命名与标注

- 关系名 = **动词**，小写
- 每条关系必须**双向可读**
  - 例：`USER ---publishes（发布）---→ CARE_TASK`
  - 反向：`CARE_TASK ---published by（由...发布）---→ USER`
- 连线上的标注格式：`动词（中文翻译）`
- 注解文字颜色必须与对应线条颜色一致
- 注解背景必须为纯白色 `labelBackgroundColor=#FFFFFF`
- 注解字体加粗 `fontStyle=1`

### 第4章 可选性与基数性（关键！）

关系连线包含 **两个独立维度**，分别用不同的视觉元素表达：

| 维度                    | 视觉元素              | 含义            | 取值                                  |
| --------------------- | ----------------- | ------------- | ----------------------------------- |
| **可选性 (Optionality)** | 线的**样式**（实线/虚线）   | 该实体是否"必须"参与关系 | 实线 = 必须(MUST BE) / 虚线 = 可以(MAY BE)  |
| **基数性 (Cardinality)** | 线的**末端符号**（单线/鸭脚） | 另一端实体的数量范围    | 单线 = 一个(ONE) / 鸭脚 = 多个(ONE OR MORE) |

**四种标准组合：**

```
┌─────────────┬──────────┬──────────┬─────────────────────────┐
│   组合       │ 可选性    │  基数性   │   阅读格式               │
├─────────────┼──────────┼──────────┼─────────────────────────┤
│ 实线 + 单线   │ MUST BE │ ONE      │ 必须是 恰恰一个          │
│ 虚线 + 单线   │ MAY BE  │ ONE      │ 可以是 零或一个          │
│ 实线 + 鸦脚   │ MUST BE │ ONE OR MORE │ 必须是 一个或多个     │
│ 虚线 + 鸦脚   │ MAY BE  │ ONE OR MORE │ 可以是 零个或多个    │
└─────────────┴──────────┴──────────┴─────────────────────────┘
```

**⚠️ 核心原则：避免"必须"+"零个"的矛盾**

- **实线（MUST BE）= 必须参与 = 至少1个**，不能搭配"零个"
- **虚线（MAY BE）= 可选参与 = 可以0个**，可以搭配"零个"

**错误示例：**

- ❌ "每个用户**必须**发布**零个**或多个任务" ← 矛盾！（实线却说可以零个）

**正确示例：**

- ✅ "每个用户**可能**发布**零个**或多个任务" ← 正确！（虚线+零个或多个）
- ✅ "每个用户**必须**发布**一个或**多个任务" ← 正确！（实线+一个或多个）

**Draw\.io 实现：**

| 可选性           | Draw\.io 实现                 | 视觉效果       | 颜色             |
| ------------- | --------------------------- | ---------- | -------------- |
| Mandatory（必须） | `dashed=0`                  | `━━━` 实线   | 马卡龙绿 `#7BC8A4` |
| Optional（可选）  | `dashed=1; dashPattern=8 4` | `- - -` 虚线 | 马卡龙粉 `#F4A8B0` |

| 基数性      | Draw\.io Arrow | 视觉效果    | 含义          |
| -------- | -------------- | ------- | ----------- |
| One（一个）  | `ERone`        | `│` 单竖线 | Exactly ONE |
| Many（多个） | `ERmany`       | `<` 鸦脚纹 | ONE OR MORE |

### 第5章 ERD 绘制流程（8步法）

```
步骤 1: 识别实体 → 列出系统中所有独立的"事物"
       ↓
步骤 2: 识别属性 → 为每个实体列出所有属性，确定UID
       ↓
步骤 3: 确定主键 → 用 # 标记每个实体的唯一标识符
       ↓
步骤 4: 识别关系 → 找出实体之间的业务联系（动词）
       ↓
步骤 5: 确定可选性与基数性 → 用实线/虚线+单线/鸭脚标注
       ↓
步骤 6: 处理 M:N → 创建关联实体或中间关系
       ↓
步骤 7: 布局设计 → 三列紧凑布局或星型分布，高频实体居中
       ↓
步骤 8: 验证 ERD → 双向阅读每条关系，检查线条交叉
```

### 第6章 命名规范

| 对象   | 规范          | 示例                             |
| ---- | ----------- | ------------------------------ |
| 实体名  | 全大写，单数名词    | `STUDENT`、`COURSE`             |
| 属性名  | 小写，下划线分隔    | `student_name`、`date_of_birth` |
| 关系名  | 小写，动词       | `enrolls`、`teaches`            |
| 关联实体 | 组合名或有意义的业务名 | `ENROLLMENT`、`ORDER_ITEM`      |

### 第7章 概念ERD禁止事项

1. ❌ **概念ERD中不显示外键字段**（FK隐含在关系中）
2. ❌ **不使用派生属性**（如 age 可由 birth\_date 推算）
3. ❌ **不标注数据类型**（在设计更靠后的阶段处理）
4. ❌ **不在概念模型中展示物理存储细节**
5. ❌ **不遗漏任何属性，也不添加无业务意义的属性**

***

## 二、实体表示法（Draw\.io 实现）

### 2.1 实体容器模板

```xml
<mxCell id="ent_user" value="USER（用户）"
        style="rounded=1;whiteSpace=wrap;html=1;
               fillColor=#dae8fc;strokeColor=#6c8ebf;
               strokeWidth=2;arcSize=10;
               container=1;collapsible=0;
               verticalAlign=top;align=center;
               fontStyle=1;fontSize=14;fontFamily=Consolas;
               fontColor=#000000;overflow=1;"
        vertex="1" parent="1">
  <mxGeometry x="X" y="Y" width="240" height="H" as="geometry"/>
</mxCell>
```

> **注意**：
>
> - 实体标题必须使用 `fontColor=#000000`（黑色字体），确保在浅色背景上清晰可读
> - **必须包含** **`overflow=1`**，允许子元素（中点、NT菱形）超出实体边界显示
> - 实体标题使用 `fontStyle=1`（加粗），字号 14px
> - 属性行字体颜色用 `#333333`（深灰），字号 12px，Consolas 等宽字体

### 2.2 属性行模板（作为实体子元素）

```xml
<!-- PK + 必填 -->
<mxCell id="uf1" value="#* user_id（用户ID）"
        style="text;strokeColor=none;fillColor=none;
               align=left;verticalAlign=middle;
               spacingLeft=8;fontSize=12;
               fontFamily=Consolas;fontColor=#333333;"
        vertex="1" parent="ent_user">
  <mxGeometry y="30" width="240" height="26" as="geometry"/>
</mxCell>

<!-- 必填属性 -->
<mxCell id="uf2" value="* real_name（真实姓名）"
        style="text;strokeColor=none;fillColor=none;
               align=left;verticalAlign=middle;
               spacingLeft=8;fontSize=12;
               fontFamily=Consolas;fontColor=#333333;"
        vertex="1" parent="ent_user">
  <mxGeometry y="56" width="240" height="26" as="geometry"/>
</mxCell>

<!-- 可选属性 -->
<mxCell id="uf3" value="o wechat（微信号）"
        style="text;strokeColor=none;fillColor=none;
               align=left;verticalAlign=middle;
               spacingLeft=8;fontSize=12;
               fontFamily=Consolas;fontColor=#333333;"
        vertex="1" parent="ent_user">
  <mxGeometry y="82" width="240" height="26" as="geometry"/>
</mxCell>
```

### 2.3 实体高度计算

```
H = 30（标题区）+ N属性 × 26
例：3个属性 = 30 + 3×26 = 108 → 取整 110
例：8个属性 = 30 + 8×26 = 238 → 取整 240
```

### 2.4 子类型父类型实体框（带internal分区）

```xml
<!-- 父类型实体框（高度增加以容纳子类型区段） -->
<mxCell id="ent_staff" value="STAFF（员工）" style="
  rounded=1;whiteSpace=wrap;html=1;container=1;collapsible=0;
  verticalAlign=top;align=center;fontStyle=1;fontSize=14;
  fillColor=#dae8fc;strokeColor=#6c8ebf;strokeWidth=2;overflow=1;"
  vertex="1" parent="1">
  <mxGeometry x="100" y="100" width="280" height="320" as="geometry"/>
</mxCell>

<!-- 子类型分隔线：白色横线（不是绿色！） -->
<mxCell id="sep1" value="" style="
  line;strokeWidth=2;fillColor=none;strokeColor=#FFFFFF;"
  vertex="1" parent="ent_staff">
  <mxGeometry y="120" width="280" height="1" as="geometry"/>
</mxCell>

<!-- 子类型区段1：MANAGER -->
<mxCell id="sub_manager"
        value="&lt;b&gt;MANAGER&lt;/b&gt;&lt;br&gt;* budget（预算）&lt;br&gt;* target_revenue（目标收入）"
        style="text;strokeColor=none;fillColor=none;html=1;
               align=left;verticalAlign=top;spacingLeft=8;
               fontSize=10;fontFamily=Consolas;fontColor=#333333;"
        vertex="1" parent="ent_staff">
  <mxGeometry y="125" width="280" height="60" as="geometry"/>
</mxCell>
```

> **子类型分隔线使用白色** **`strokeColor=#FFFFFF`**，宽度 `strokeWidth=2`，不能使用绿色或其他颜色。子类型文字使用 `<b>` 加粗标签。每个子类型区段之间都有分隔线，最后一个区段是 OTHER。分隔线位于子类型区段文字的上方。`y` 坐标：分隔线在前，子类型文字在后，两者 `y` 坐标相差约 5px。

***

## 三、关系连线规范

### 3.1 连线基本样式

```
edgeStyle=orthogonalEdgeStyle;
html=1;
strokeColor=#7BC8A4; 或 #F4A8B0;（马卡龙色系）
strokeWidth=2;
startFill=0; endFill=0;
fontSize=11; fontColor=#7BC8A4 或 #F4A8B0;
fontStyle=1;
labelBackgroundColor=#FFFFFF;
```

### 3.2 核心设计：中点分隔法（实虚线分段）

当一条关系的**两端可选性不同**时（一端MUST、一端MAY），需要在中间加入一个**不可见的中点**（midpoint），将一条线拆成两段：

```
实体A ──[段1: 实体A端设定]── ●(中点) ──[段2: 实体B端设定]── 实体B
       (必须关系)            (invisible)   (可能关系)
       绿色 #7BC8A4                       粉色 #F4A8B0
```

**每一段的设定规则：**

| 规则     | 说明                                                                  |
| ------ | ------------------------------------------------------------------- |
| **线型** | 当前段朝向的实体端的可选性 → 虚线 = MAY / 实线 = MUST                                |
| **箭头** | 靠近实体的那端标记基数（单竖线/鸦脚），**靠近中点的那端不标记**（endArrow=none / startArrow=none） |
| **颜色** | 实线段 = 马卡龙绿 `#7BC8A4`；虚线段 = 马卡龙粉 `#F4A8B0`                           |
| **注解** | 两段的注解位置应分布在线的两侧，避免重叠                                                |

**基数标注方向详解（关键理解）：**

**Oracle Academy 标准读法规则（从左到右、从右到左）：**

关系有两端，读法需要从两端分别读取。以 `BOOK ━━━(实线)──(鸦脚在BOOK端) - - - AUTHOR` 为例：

```
从左到右读（从实体A看实体B）：
BOOK ━━━(实线)──(鸦脚在BOOK端) - - - AUTHOR

1. 看靠近BOOK的线段样式 → 实线 = MUST BE（必须）
2. 看靠近AUTHOR的基数符号 → 单线 = ONE（一个）
3. 完整读法：EACH BOOK MUST BE WRITTEN BY ONE AUTHOR（每本BOOK必须由一位AUTHOR编写）

从右到左读（从实体B看实体A）：
BOOK ━━━(实线)──(鸦脚在BOOK端) - - - AUTHOR

1. 看靠近AUTHOR的线段样式 → 虚线 = MAY BE（可能）
2. 看靠近BOOK的基数符号 → 鸦脚 = ONE OR MORE（一个或多个）
3. 完整读法：EACH AUTHOR MAY BE THE AUTHOR OF ONE OR MORE BOOKS（每位AUTHOR可能是一位或多位BOOK的作者）
```

**核心规则（★★★★★）：**

- **靠近实体A的线段样式** = 实体A的**可选性**（实线=MUST/虚线=MAY）
- **靠近实体B的基数符号** = 每个实体A对应**多少个实体B**（ERone=ONE/ERmany=MANY）
- 关系必须**双向可读**，从左到右看一次，再从右到左看一次

**Draw\.io 实现规则：**

```
段1：实体A → 中点（startArrow标记实体B的基数）
段2：中点 → 实体B（endArrow标记实体A的基数）
```

- `startArrow` = 靠近**实体A端**的基数符号 = **"每个A对应多少个B"**
- `endArrow` = 靠近**实体B端**的基数符号 = **"每个B对应多少个A"**

```
段1：实体A ───[A端线型 + B端基数]─── 中点
段2：中点 ───[B端线型 + A端基数]─── 实体B
```

**颜色与线型对应关系（★★★★★）：**

- **马卡龙绿** **`#7BC8A4`** = 实线 `dashed=0` = **MUST BE（必须）** = 强连接
- **马卡龙粉** **`#F4A8B0`** = 虚线 `dashed=1` = **MAY BE（可能）** = 弱连接

**完整示例：USER ↔ PET（NT关系）**

```
USER ─ ─ ─(虚线/粉色)── ERmany ── PET
       (MAY)            (MANY)
```

- 从USER看PET：USER端虚线(MAY) + PET端ERmany → "每个USER可能拥有零只或多只PET"
- 从PET看USER：PET端实线(MUST) + USER端ERone → "每只PET必须属于恰恰一个USER"

### 3.3 中点放置规则

**两种中点放置方案：**

#### 方案A：子元素方案（实体紧密排列时推荐）

中点是**源实体的子元素**（`parent="ent_xxx"`，非 `parent="1"`），使用实体局部坐标。适用于三列紧凑布局等实体间距较小的情况。

**优势：**

- 中点在实体边界上，edge路径极短
- **跟随实体移动**，拖动实体时自动重新路由
- 局部坐标计算简单

```xml
<mxCell id="m1" value=""
        style="ellipse;fillColor=none;strokeColor=none;
               opacity=0.01;pointerEvents=1;"
        vertex="1" parent="ent_user">  <!-- parent=源实体ID -->
  <mxGeometry x="260" y="120" width="8" height="8" as="geometry"/>
</mxCell>
```

```
局部坐标 = 全局坐标 - 实体位置
例：实体在(550,50)，中点全局位置(810,170)
局部坐标 = (810-550, 170-50) = (260, 120)
```

#### 方案B：根级方案（两列模板布局时推荐）

中点作为**根级元素**（`parent="1"`），放置在**两列实体之间的间隙中**。适用于两列模板布局，避免注脚贴着实体。

```xml
<mxCell id="m1" value=""
        style="ellipse;fillColor=none;strokeColor=none;
               opacity=0.01;pointerEvents=1;"
        vertex="1" parent="1">  <!-- parent="1"（根级） -->
  <mxGeometry x="480" y="90" width="8" height="8" as="geometry"/>
</mxCell>
```

**计算规则：**

```
中点X = 左实体右边缘 + (右实体左边缘 - 左实体右边缘) / 2
例：左实体x=100,w=260→右边缘360，右实体x=600→左边缘600
中点X = 360 + (600-360)/2 = 480
```

> **选择建议：** 三列紧密布局（实体间距≤150px）用方案A；两列或松散布局（实体间距≥200px）用方案B。方案B的关键优势是**中点与实体边框之间有足够间距**，避免基数符号（鸦脚/单竖线）与实体边框贴在一起。

### 3.4 两种关系的Draw\.io实现

#### 3.4.1 普通关系（两端不同可选性，中点分隔）

**关键规则：**

- `startArrow`（靠近实体A端）= 标注的是**实体B的基数** = "每个A对应多少个B"
- `endArrow`（靠近实体B端）= 标注的是**实体A的基数** = "每个B对应多少个A"
- 段1的颜色/线型 = **实体A端**的可选性
- 段2的颜色/线型 = **实体B端**的可选性

```xml
<!-- 步骤1：创建中点（源实体子元素） -->
<mxCell id="mX" value=""
        style="ellipse;fillColor=none;strokeColor=none;
               opacity=0.01;pointerEvents=1;"
        vertex="1" parent="ent_user">
  <mxGeometry x="260" y="120" width="8" height="8" as="geometry"/>
</mxCell>

<!-- 步骤2：A到中点的线段（startArrow=实体B的基数） -->
<mxCell id="rXa" value="正向动词（翻译）" style="
  edgeStyle=orthogonalEdgeStyle;html=1;
  endArrow=none;
  startArrow=ERmany（或ERone）;startFill=0;  ← 标注的是B端基数："每个A对应多少个B"
  strokeColor=#7BC8A4（实线）或#F4A8B0（虚线）;  ← 颜色由A端可选性决定
  strokeWidth=2;dashed=0（实线）或1（虚线）;dashPattern=8 4;
  fontSize=11;fontColor=与线条同色;fontStyle=1;
  labelBackgroundColor=#FFFFFF;"
  edge="1" parent="1" source="ent_user" target="mX"/>

<!-- 步骤3：中点到B的线段（endArrow=实体A的基数） -->
<mxCell id="rXb" value="反向动词（翻译）" style="
  edgeStyle=orthogonalEdgeStyle;html=1;
  startArrow=none;
  endArrow=ERone（或ERmany）;endFill=0;  ← 标注的是A端基数："每个B对应多少个A"
  strokeColor=#7BC8A4（实线）或#F4A8B0（虚线）;  ← 颜色由B端可选性决定
  strokeWidth=2;dashed=0（实线）或1（虚线）;dashPattern=8 4;
  fontSize=11;fontColor=与线条同色;fontStyle=1;
  labelBackgroundColor=#FFFFFF;"
  edge="1" parent="1" source="mX" target="ent_B"/>
```

**实战示例：CARE\_PROVIDER ↔ ORDER（每个CP可能承接多个ORDER，每个ORDER必须被一个CP承接）**

```xml
<!-- CP端：虚线（MAY）+ startArrow=ERone（每个ORDER对应一个CP） -->
<mxCell id="r7a" value="accepts（承接）" style="
  edgeStyle=orthogonalEdgeStyle;html=1;endArrow=none;
  startArrow=ERone;startFill=0;
  strokeColor=#F4A8B0;strokeWidth=2;dashed=1;dashPattern=8 4;
  fontSize=11;fontColor=#F4A8B0;fontStyle=1;labelBackgroundColor=#FFFFFF;"
  parent="1" source="ent_cp" target="m7" edge="1">
</mxCell>

<!-- ORDER端：实线（MUST）+ endArrow=ERmany（每个CP对应多个ORDER） -->
<mxCell id="r7c" value="from provider（提供）" style="
  edgeStyle=orthogonalEdgeStyle;html=1;startArrow=none;
  endArrow=ERmany;endFill=0;
  strokeColor=#7BC8A4;strokeWidth=2;dashed=0;
  fontSize=11;fontColor=#7BC8A4;fontStyle=1;labelBackgroundColor=#FFFFFF;"
  parent="1" source="m7" target="ent_order" edge="1">
</mxCell>
```

#### 3.4.2 两端相同可选性（无需中点）

当关系两端可选性相同（两端都MUST或都MAY），可以使用**一条连续的实线或虚线**，不需要中点：

```xml
<mxCell id="rX" value="关系名（翻译）" style="
  edgeStyle=orthogonalEdgeStyle;html=1;
  startArrow=ERone（或ERmany）;startFill=0;
  endArrow=ERmany（或ERone）;endFill=0;
  strokeColor=#7BC8A4（实线）或#F4A8B0（虚线）;
  strokeWidth=2;dashed=0或1;dashPattern=8 4;
  fontSize=11;fontColor=与线条同色;fontStyle=1;
  labelBackgroundColor=#FFFFFF;"
  edge="1" parent="1" source="ent_A" target="ent_B"/>
```

***

## 四、七种关系类型详解

### 4.1 普通关系（Mandatory/Optional 1:N）

最常见的二元关系，使用中点分隔法实现。

**阅读格式：**

```
每个 实体A 【必须 | 可能】 动词 【一个 | 一个或多个】 实体B
```

**示例：USER → PET**

```
每个 USER 可能 拥有 零只或多只 PET    ← 虚线+鸦脚（USER端）
每只 PET 必须 属于 恰恰一个 USER     ← 实线+单竖线（PET端）
```

### 4.2 限定关系（Constraining Relationships）

**教材定义：** 实体的UID由多个相关实体的UID通过限定关系组成。

**视觉特征：** 在关系线上用小短横线（`-`）标记表示该关系参与了UID的构成。

**draw\.io画法：**

- 与普通FK关系画法相同，但在关系线上添加短横线标记
- 在垂直于关系线的方向添加一个小线段作为短横线标识

**示例：** CARE\_TASK → USER、ORDER → CARE\_TASK

### 4.3 交集实体（Intersection Entity）—— 解析M:M关系

**教材定义：** "如果属性描述关系，则必须解析关系"

**核心规则：**

- 交集实体中的关系**始终都是必需的**
- 交集实体到两个父实体：都使用**实线+鸦脚**（MUST BE + MANY）
- 父实体到交集实体：都使用**虚线+单竖线**（MAY BE + ONE）
- 交集实体的UDI由两个父实体的FK组合而成（复合主键）

**画法示意：**

```
      虚线+单竖线(MAY)        实线+鸦脚(MUST)
ORDER ────[段1]──── ● ────[段2]──── ORDER_ITEM
                                      ↑
                              UID = order_id(FK) + product_id(FK)

PRODUCT ──[段1]──── ● ────[段2]──── ORDER_ITEM
      虚线+单竖线(MAY)        实线+鸦脚(MUST)
```

### 4.4 不可转移关系（Non-transferable Relationships）

**教材定义：** "不可转移关系不能在其所连接的实体的实例之间移动"

**核心规则：**

- "不可转移关系在关系中用菱形表示"
- "不可转移关系只能是必需的"

**NT菱形放置：** 放在靠近"必须"端实体的线段上，重心精确落在关系线上。

详见 [第五章：不可转移关系（NT）菱形标记规范](#五不可转移关系nt菱形标记规范)。

### 4.5 父类型和子类型（Supertype/Subtype）

**教材定义：** "每个子类型都是父类型的具体化，因此必须放在实体内"

**子类型特征：**

- 继承父类型的所有属性
- 继承父类型的所有关系
- 通常有自己的属性或关系
- 在父类型中进行绘制
- 从不独立存在
- 可以有自己的子类型（嵌套）

**两个核心规则：**

| 规则                     | 含义                                   |
| ---------------------- | ------------------------------------ |
| **全面性** (Completeness) | 父类型的每个实例**必须**属于某个子类型，必须包含"OTHER"子类型 |
| **互斥性** (Disjointness) | 父类型的每个实例**只能**属于一个子类型                |

**画法：** 父类型实体框内用横线分隔，每个子类型一个区段，最后一个区段是OTHER。

```
┌──────────────────────────────────┐
│            STAFF                 │  ← 父类型名
│            #id                   │
│          * first name            │
│          * last name             │  ← 公共属性
│          * date of birth         │
├──────────────────────────────────┤  ← 分隔线1
│           MANAGER                │  ← 子类型A
│          * budget                │
│          * target revenue        │
├──────────────────────────────────┤  ← 分隔线2
│         ORDER_TAKER              │  ← 子类型B
│          * overtime rate         │
├──────────────────────────────────┤  ← 分隔线3
│            COOK                  │  ← 子类型C
│          * training              │
├──────────────────────────────────┤  ← 分隔线4
│           OTHER                  │  ← 必须包含 OTHER
└──────────────────────────────────┘
```

### 4.6 递归关系（Recursive Relationships）

**教材定义：** "递归关系是其中的一个实体实例与同一实体中的另一个实例相关的关系"
"递归关系始终使用循环进行建模"

**画法规范：**

- **一条线**：不需要中点，直接用一条edge做环路
- **source=target**：都指向同一个实体ID
- **两端不同基数**：`startArrow` 和 `endArrow` 可以不同
- **两个注解**：第二个注解需要单独创建一个文本标签

**示例：CARE\_PROVIDER 递归关系**

```
CARE_PROVIDER（宠托师）
  ↙                      ↘
recommends（推荐）      recommended by（被推荐）
虚线+鸦脚               虚线+单竖线
```

```xml
<mxCell id="rRec" value="recommends（推荐）" style="
  edgeStyle=orthogonalEdgeStyle;html=1;
  startArrow=ERmany;startFill=0;
  endArrow=ERone;endFill=0;
  dashed=1;dashPattern=8 4;
  strokeColor=#F4A8B0;strokeWidth=2;
  fontSize=11;fontColor=#F4A8B0;fontStyle=1;
  labelBackgroundColor=#FFFFFF;
  exitX=1;exitY=0.2;entryX=1;entryY=0.8;"
  edge="1" parent="1" source="ent_provider" target="ent_provider">
  <mxGeometry relative="1" as="geometry">
    <Array as="points">
      <mxPoint x="实体右X+60" y="实体上Y+60"/>
      <mxPoint x="实体右X+60" y="实体下Y-60"/>
    </Array>
  </mxGeometry>
</mxCell>

<!-- 反向标注单独文本 -->
<mxCell id="rRec_rev" value="recommended by（被推荐）" style="
  text;html=1;align=center;verticalAlign=middle;
  fontSize=11;fontColor=#F4A8B0;fontStyle=1;
  fillColor=#FFFFFF;"
  vertex="1" parent="1">
  <mxGeometry x="标注X" y="标注Y" width="120" height="20" as="geometry"/>
</mxCell>
```

### 4.7 弧关系（Arc Relationships）

**教材定义：** "弧是一个互斥关系组，旨在使实体的任何实例均只能存在一种关系"

**核心特征：**

- 弧中的关系通常具有相同的关系名称
- 弧中的关系要么全部为必需，要么全部为可选，且应具有相同的基数
- 弧属于单个实体，只能包含源自该实体的关系
- "弧中所包含的关系在关系弧线上有一个圆"

**draw\.io实现：**

1. 从父实体底部引出两条线，分别指向两个关联实体
2. 两条分支线在靠近父实体端略微交叉
3. 使用 `shape=arc` 绘制弧线横跨两条分支线
4. 在弧线与两条分支线的交叉点各放一个小圆形

```xml
<!-- 弧线形状 -->
<mxCell id="arc" value="" style="
  shape=arc;html=1;dx=20;dy=10;
  fillColor=none;strokeColor=#E67E22;strokeWidth=3;"
  vertex="1" parent="1">
  <mxGeometry x="arcX" y="arcY" width="80" height="30" as="geometry"/>
</mxCell>

<!-- 圆点 -->
<mxCell id="dot1" value="" style="
  ellipse;fillColor=#E67E22;strokeColor=none;"
  vertex="1" parent="1">
  <mxGeometry x="dot1X" y="dot1Y" width="6" height="6" as="geometry"/>
</mxCell>
```

***

## 五、不可转移关系（NT）菱形标记规范

### 5.1 NT概念

不可转移关系（Non-Transferable）：关系一旦建立，不能在实体实例之间转移。

例如：宠物一旦属于主人A，就不能随便改成属于主人B。

### 5.2 NT菱形放置规则

NT菱形作为**可见中点**，重心精确落在关系线段上。

```
实体E ─── 段1 ─── 不可见中点M ─── 段2 ─── NT菱形 ─── 段3 ─── 另一实体
                                        ↑
                                   重心落在线上
```

**坐标计算公式：**

```
设实体边缘点为 E，不可见中点为 M
NT菱形中心 = (E + M) / 2
NT菱形左上角 = (中心X - 12, 中心Y - 8)  // 菱形宽24高16
```

### 5.3 NT关系的3段edge结构

NT关系从原来的2段变为3段：

| 段  | source → target   | 基数箭头      | 线型     | 标签内容     |
| -- | ----------------- | --------- | ------ | -------- |
| 段1 | 实体A → 不可见中点(mX)   | A端基数箭头    | 按A端可选性 | 正向动词（中文） |
| 段2 | mX → NT菱形(nt\_rX) | 无箭头（none） | 实线（绿色） | 无标签      |
| 段3 | nt\_rX → 实体B      | B端基数箭头    | 按B端可选性 | 反向动词（翻译） |

**NT注解格式：** 与普通关系注解一致，格式为 `动词（中文翻译）`

- 例：`owned by（被拥有）`
- 例：`from task（收录）`
- **不要在注解中额外标注"NT"或"不可转移"文字**（NT菱形本身已表达此含义）

### 5.4 NT菱形XML模板（子元素方案）

NT菱形作为**目标实体的子元素**（`parent="ent_target"`），使用负x坐标放在实体左边界外。当拖动目标实体时，NT菱形自动跟随移动。

```xml
<!-- NT菱形（作为目标实体子元素，放在左边界外） -->
<mxCell id="nt_rX" value="NT" style="
  shape=rhombus;html=1;perimeter=rhombusPerimeter;
  fillColor=none;strokeColor=#E67E22;strokeWidth=1.5;
  fontSize=8;fontColor=#E67E22;fontStyle=1;
  align=center;verticalAlign=middle;
  pointerEvents=0;"
  vertex="1" parent="ent_target">  <!-- parent=目标实体ID -->
  <mxGeometry x="-30" y="50" width="24" height="16" as="geometry"/>
</mxCell>

<!-- 中间段：不可见中点 → NT菱形（无箭头无标签） -->
<mxCell id="rX_mid" value="" style="
  edgeStyle=orthogonalEdgeStyle;html=1;
  endArrow=none;startArrow=none;
  strokeColor=#7BC8A4;strokeWidth=2;dashed=0;"
  edge="1" parent="1" source="mX" target="nt_rX">
  <mxGeometry relative="1" as="geometry"/>
</mxCell>
```

### 5.5 常见NT关系示例

| 关系                     | NT端实体      | 原因       |
| ---------------------- | ---------- | -------- |
| PET → USER             | PET（宠物）    | 宠物不能换主人  |
| CARE\_TASK → USER      | TASK（任务）   | 任务不能换发布者 |
| ORDER → CARE\_TASK     | ORDER（订单）  | 订单不能换任务  |
| ORDER → CARE\_PROVIDER | ORDER（订单）  | 订单不能换宠托师 |
| REVIEW → ORDER         | REVIEW（评价） | 评价不能换订单  |

***

## 六、ER图布局优化策略

### 6.1 两种布局方案

根据项目规模和复杂度，推荐以下两种布局方案：

#### 方案A：三列紧凑布局（推荐 · 9实体以内）

**核心思想：** 按业务关系分三列，大量垂直/水平短边，零交叉。

**画布配置：** 1500×1500

```
         列1(x≈100)    列2(x≈550)     列3(x≈1000)
行1(y≈50):    USER          CP              PET
行2(y≈400):   ADDR          TASK            TP
行3(y≈800):                 ORDER
行4(y≈1200):                EXC           REVIEW
```

**实体坐标模板（9实体）：**

| 实体     | x    | y    | 宽   | 高   | 颜色   |
| ------ | ---- | ---- | --- | --- | ---- |
| USER   | 100  | 50   | 260 | 260 | 🔵 蓝 |
| ADDR   | 100  | 400  | 260 | 160 | 🟢 绿 |
| PET    | 1000 | 50   | 260 | 270 | 🟣 紫 |
| CP     | 550  | 50   | 260 | 260 | 🟡 黄 |
| TASK   | 550  | 400  | 260 | 320 | 🔵 蓝 |
| ORDER  | 550  | 800  | 260 | 250 | 🔵 蓝 |
| TP     | 1000 | 400  | 260 | 110 | 🟠 橙 |
| EXC    | 550  | 1200 | 260 | 185 | ⚫ 灰  |
| REVIEW | 1000 | 1200 | 260 | 160 | 🔴 红 |

**优势：**

- 同列实体用垂直边连接（极短）
- 跨列用水平/斜线连接
- 经参数方程验证零交叉

#### 方案B：星型分布布局（适合10+实体）

**核心思想：** 将最高频连接的实体居中，其他实体围绕分布。

**画布配置：** 2400×1800

```
                    CARE_PROVIDER
                    (左上 - 黄色)
                         │ R2
                         │
   USER_ADDRESS ──────── USER ────────── PET
   (右上 - 绿色)   R1    (居中 - 蓝)  R3  (右中 - 紫)
                         │
                         │ R4
                    CARE_TASK
                    (下中 - 蓝色)
                    ╱    │    ╲
             R6/R7  R8    R9  R5/R10
              ╱     │      ╲    ╲
           ORDER  REVIEW    EXCEPTION
           (左下) (底部)    (右下)
```

### 6.2 图层策略（避免线条交叉）

| 图层 | 关系路径                 | 走向   | 避免重叠方法             |
| -- | -------------------- | ---- | ------------------ |
| 1  | USER→ADDRESS         | 右上   | 单独通道，Y轴在USER上半部分   |
| 2  | USER→CARE\_PROVIDER  | 左上   | Y轴在USER上半部分，与图层1错开 |
| 3  | USER→PET             | 右偏下  | USER右下侧出线          |
| 4  | USER→CARE\_TASK      | 正下   | 居中偏左               |
| 5  | PET→CARE\_TASK       | 左下斜线 | 对角线连接              |
| 6  | CARE\_TASK→ORDER     | 左上斜线 | 图层6外围              |
| 7  | CARE\_PROVIDER→ORDER | 左下长线 | 外围大弧线              |
| 8  | ORDER→REVIEW         | 正下   | 短距离                |
| 9  | ORDER→EXCEPTION      | 右下斜线 | 通过m9中点             |
| 10 | CARE\_TASK→ADDRESS   | 右上斜线 | 与图层4错开             |

### 6.3 实体间距标准

#### 三列紧凑版：

| 方向      | 最小间距  | 推荐间距      |
| ------- | ----- | --------- |
| 水平（列间距） | 190px | 250-450px |
| 垂直（行间距） | 80px  | 100-300px |

#### 星型分布版：

| 方向 | 最小间距  | 推荐间距      |
| -- | ----- | --------- |
| 水平 | 600px | 700-850px |
| 垂直 | 400px | 500-600px |

### 6.4 边路由策略（显式exit/entry约束）

| 关系              | 路由类型 | exitX | exitY | entryX | entryY |
| --------------- | ---- | ----- | ----- | ------ | ------ |
| R1 CP→USER      | 水平   | 0     | 0.3   | 1      | 0.5    |
| R2 USER→PET     | 水平   | 1     | 0.3   | 0      | 0.5    |
| R6 TASK→ORDER   | 垂直   | 0.5   | 1     | 0.5    | 0      |
| R7 CP→ORDER     | 垂直   | 0.5   | 1     | 0.3    | 0      |
| R9 ORDER→EXC    | 垂直   | 0.5   | 1     | 0.5    | 0      |
| R11 PET→TP      | 垂直   | 0.5   | 1     | 0.5    | 0      |
| R3 USER→TASK    | 斜线   | 1     | 0.7   | 0      | 0.3    |
| R8 ORDER→REVIEW | 斜线   | 1     | 0.5   | 0      | 0.5    |
| R10 TASK→TP     | 斜线   | 1     | 0.3   | 0      | 0.5    |
| R12 TASK→ADDR   | 斜线   | 0     | 0.5   | 1      | 0.5    |

### 6.5 零交叉验证方法

用参数方程验证任意两条edge是否交叉：

```
设线段1: P1 + t*(P2-P1), t∈[0,1]
设线段2: P3 + s*(P4-P3), s∈[0,1]

解方程组:
  P1x + t*(P2x-P1x) = P3x + s*(P4x-P3x)
  P1y + t*(P2y-P1y) = P3y + s*(P4y-P3y)

若 t∈[0,1] 且 s∈[0,1]，则两线段交叉
否则不交叉
```

***

## 七、Draw\.io 完整实现模板

### 7.1 画布设置

```xml
<mxGraphModel dx="1200" dy="800" grid="1" gridSize="10"
              guides="1" tooltips="1" connect="1" arrows="1"
              fold="1" page="1" pageScale="1" pageWidth="1500"
              pageHeight="1500" background="none" math="0"
              shadow="0">
  <root>
    <mxCell id="0" />
    <mxCell id="1" parent="0" />
    <!-- 所有实体和关系放在此处 -->
  </root>
</mxGraphModel>
```

### 7.2 完整的关系连线实现

```xml
<!-- ============================================================== -->
<!-- 示例：USER → PET 关系（1:N，USER端=虚线+鸦脚，PET端=实线+单竖线） -->
<!-- ============================================================== -->

<!-- 步骤1：创建中点（作为源实体ent_user的子元素） -->
<mxCell id="m1" value=""
        style="ellipse;fillColor=none;strokeColor=none;
               opacity=0.01;pointerEvents=1;"
        vertex="1" parent="ent_user">
  <mxGeometry x="260" y="120" width="8" height="8" as="geometry"/>
</mxCell>

<!-- 步骤2：NT菱形（靠近PET端，作为ent_pet的子元素，放在左边界外） -->
<mxCell id="nt_r3" value="NT" style="
  shape=rhombus;html=1;perimeter=rhombusPerimeter;
  fillColor=none;strokeColor=#E67E22;strokeWidth=1.5;
  fontSize=8;fontColor=#E67E22;fontStyle=1;
  align=center;verticalAlign=middle;pointerEvents=0;"
  vertex="1" parent="ent_pet">
  <mxGeometry x="-30" y="100" width="24" height="16" as="geometry"/>
</mxCell>

<!-- 步骤3：段1：USER→中点（USER端=虚线+鸦脚：MAY + MANY） -->
<mxCell id="r3a" value="owns（拥有）" style="
  edgeStyle=orthogonalEdgeStyle;html=1;
  endArrow=none;
  startArrow=ERmany;startFill=0;
  strokeColor=#F4A8B0;strokeWidth=2;dashed=1;dashPattern=8 4;
  fontSize=11;fontColor=#F4A8B0;fontStyle=1;
  labelBackgroundColor=#FFFFFF;
  exitX=1;exitY=0.3;"
  edge="1" parent="1" source="ent_user" target="m1">
  <mxGeometry relative="1" as="geometry"/>
</mxCell>

<!-- 步骤4：段2：中点→NT菱形（无箭头，实线绿色） -->
<mxCell id="r3b" value="" style="
  edgeStyle=orthogonalEdgeStyle;html=1;
  endArrow=none;startArrow=none;
  strokeColor=#7BC8A4;strokeWidth=2;dashed=0;
  entryX=0;entryY=0.5;"
  edge="1" parent="1" source="m1" target="nt_r3">
  <mxGeometry relative="1" as="geometry"/>
</mxCell>

<!-- 步骤5：段3：NT菱形→PET（PET端=实线+单竖线） -->
<mxCell id="r3c" value="owned by（被拥有）" style="
  edgeStyle=orthogonalEdgeStyle;html=1;
  startArrow=none;
  endArrow=ERone;endFill=0;
  strokeColor=#7BC8A4;strokeWidth=2;dashed=0;
  fontSize=11;fontColor=#7BC8A4;fontStyle=1;
  labelBackgroundColor=#FFFFFF;
  entryX=0;entryY=0.5;"
  edge="1" parent="1" source="nt_r3" target="ent_pet">
  <mxGeometry relative="1" as="geometry"/>
</mxCell>
```

***

## 八、反模式检查清单

### 8.1 连线与符号错误

| 编号    | 错误做法                         | 正确做法                           |
| ----- | ---------------------------- | ------------------------------ |
| AP-01 | 使用普通箭头 `classic`             | 使用 Crow's Foot `ERone/ERmany`  |
| AP-02 | `startFill=1; endFill=1`（实心） | `startFill=0; endFill=0`（空心符号） |
| AP-03 | 属性行 parent 设为 "1"            | parent 设为对应实体容器 ID             |
| AP-04 | 忽略可选性的实线/虚线区分                | 必须用实线(MUST)和虚线(MAY)区分          |
| AP-05 | 在概念 ERD 中画外键字段               | FK 隐含在关系中，不单独列出                |
| AP-06 | 关系两端可选性不同但只用一种线型             | 使用中点分隔法，实虚线分段                  |
| AP-07 | "必须"+"零个"矛盾（实线+零个）           | 虚线（MAY）+零个，实线（MUST）+一个或多个      |
| AP-08 | 鸦脚方向理解错误                     | 靠近A端的记号表示"每个B对应多少A"            |

### 8.2 属性与实体错误

| 编号    | 错误做法                  | 正确做法                      |
| ----- | --------------------- | ------------------------- |
| AP-09 | 属性缺少 `#`/`*`/`o` 标记   | 每个属性必须有标记                 |
| AP-10 | 实体名使用中文或小写            | 全大写单数英文 + 中文括号注释          |
| AP-11 | 实体颜色随意使用              | 按实体类型选择配色方案               |
| AP-12 | 属性行没有 `spacingLeft=8` | 统一使用 `spacingLeft=8` 保持对齐 |

### 8.3 关系与注解错误

| 编号    | 错误做法              | 正确做法                                |
| ----- | ----------------- | ----------------------------------- |
| AP-13 | 关系不可双向阅读          | 每条关系必须能从两端分别朗读                      |
| AP-14 | 注解文字颜色与线条不一致      | 注解颜色必须匹配线条颜色                        |
| AP-15 | 注解背景透明或黑色         | 必须使用 `labelBackgroundColor=#FFFFFF` |
| AP-16 | 关系线使用灰色 `#666666` | 必须使用马卡龙色系（绿/粉）                      |

### 8.4 布局与交叉错误

| 编号    | 错误做法                           | 正确做法                                  |
| ----- | ------------------------------ | ------------------------------------- |
| AP-17 | 实体间距过小导致线条重叠                   | 三列紧凑版：列间距250-450px                    |
| AP-18 | 多条关系线重叠交叉                      | 使用图层策略，用参数方程验证                        |
| AP-19 | 画布过大/过小（>2000×1600或<1200×1200） | 小规模(≤9实体)用1500×1500，大规模用2400×1800     |
| AP-20 | 中点作为独立顶级vertex（parent="1"）     | 中点作为源实体子元素（parent="ent\_xxx"）         |
| AP-21 | 实体缺少overflow=1                 | 所有实体必须包含overflow=1                    |
| AP-22 | 中点opacity=0（不可选择）              | 中点opacity=0.01 + pointerEvents=1（可选择） |

### 8.5 NT菱形相关错误

| 编号    | 错误做法          | 正确做法                        |
| ----- | ------------- | --------------------------- |
| AP-23 | NT菱形放在不可见中点旁边 | NT菱形作为可见中点，重心落在线段上          |
| AP-24 | NT菱形有填充色      | `fillColor=none`（无填充，仅橙色边框） |
| AP-25 | NT菱形偏移线段      | NT菱形中心 = (实体边缘 + 中点) / 2    |
| AP-26 | NT标记用实心菱形     | 用空心菱形（无填充+橙色边框 `#E67E22`）   |
| AP-27 | NT关系只有2段edge  | NT关系必须3段：实体→中点→NT菱形→另一实体    |

***

## 九、Draw\.io 导出检查清单

### 9.1 结构检查

- [ ] 所有实体都有 `container=1`
- [ ] 所有属性行都是实体的子元素（`parent="<entity_id>"`）
- [ ] 关系边使用 `edgeStyle=orthogonalEdgeStyle;`
- [ ] 所有关系边使用 `ER*` 箭头类型（`ERone`/`ERmany`）
- [ ] 所有关系边有 `startFill=0; endFill=0;`

### 9.2 样式检查

- [ ] 实体配色正确（蓝/绿/黄/紫/红/灰/橙）
- [ ] 关系线颜色：实线=绿色`#7BC8A4`，虚线=粉色`#F4A8B0`
- [ ] 注解文字颜色与线条颜色一致
- [ ] 注解背景为纯白色 `#FFFFFF`
- [ ] 注解字体加粗 `fontStyle=1`
- [ ] 虚线模式 `dashed=1; dashPattern=8 4`

### 9.3 NT菱形检查

- [ ] NT菱形使用 `fillColor=none;strokeColor=#E67E22`
- [ ] NT菱形重心落在关系线段上（中心 = (E+M)/2）
- [ ] NT关系使用3段edge（实体→中点→NT菱形→另一实体）
- [ ] 中间段(mX→nt\_rX)无label无arrow
- [ ] NT菱形放在靠近"必须"端实体的线段上

### 9.4 布局检查

- [ ] 画布大小：小规模1500×1500，大规模2400×1800
- [ ] 实体间距：三列紧凑版列间距≥190px，星型版水平≥600px
- [ ] 高频实体居中
- [ ] 中点坐标错开，无重叠
- [ ] 关系线无明显交叉（经参数方程验证）

### 9.5 内容检查

- [ ] 每条关系可双向阅读
- [ ] 注解格式：`动词（中文翻译）`
- [ ] 属性格式：`#*/o 字段名（中文名）`
- [ ] 实体名：`全大写（中文翻译）`

***

## 十、完整绘制示例：宝贝在家平台

### 10.1 实体列表（9个）

| 实体ID        | 实体名                    | 颜色   | 属性数 | 高度  |
| ----------- | ---------------------- | ---- | --- | --- |
| ent\_user   | USER（用户）               | 🔵 蓝 | 8   | 240 |
| ent\_addr   | USER\_ADDRESS（用户地址）    | 🟢 绿 | 4   | 135 |
| ent\_cp     | CARE\_PROVIDER（宠托师）    | 🟡 黄 | 8   | 240 |
| ent\_pet    | PET（宠物）                | 🟣 紫 | 8   | 240 |
| ent\_task   | CARE\_TASK（照料任务）       | 🔵 蓝 | 10  | 290 |
| ent\_tp     | TASK\_PET（任务-宠物关联）     | 🟠 橙 | 2   | 85  |
| ent\_order  | ORDER（订单）              | 🔵 蓝 | 7   | 215 |
| ent\_review | REVIEW（评价）             | 🔴 红 | 4   | 135 |
| ent\_exc    | EXCEPTION\_EVENT（异常事件） | ⚫ 灰  | 5   | 160 |

### 10.2 关系清单（12条）

| #   | 实体A   | 关系名(A→B)        | 实体B    | A端基数 | A端可选 | B端基数 | B端可选 | NT     |
| --- | ----- | --------------- | ------ | ---- | ---- | ---- | ---- | ------ |
| R1  | USER  | has（拥有）         | ADDR   | MANY | MAY  | ONE  | MUST | -      |
| R2  | USER  | becomes（成为）     | CP     | ONE  | MAY  | ONE  | MUST | -      |
| R3  | USER  | owns（拥有）        | PET    | MANY | MAY  | ONE  | MUST | **NT** |
| R4  | USER  | publishes（发布）   | TASK   | MANY | MAY  | ONE  | MUST | **NT** |
| R5  | PET   | involved in（参与） | TASK   | MANY | MAY  | MANY | MUST | -      |
| R6  | TASK  | generates（产生）   | ORDER  | MANY | MAY  | ONE  | MUST | **NT** |
| R7  | CP    | accepts（承接）     | ORDER  | MANY | MAY  | ONE  | MUST | **NT** |
| R8  | ORDER | produces（产生）    | REVIEW | MANY | MAY  | ONE  | MUST | **NT** |
| R9  | ORDER | triggers（触发）    | EXC    | ONE  | MUST | MANY | MAY  | -      |
| R10 | TASK  | involves（涉及）    | TP     | -    | MUST | -    | MUST | -      |
| R11 | PET   | involved in（参与） | TP     | -    | MUST | -    | MUST | -      |
| R12 | TASK  | served at（服务地址） | ADDR   | ONE  | MUST | MANY | MAY  | -      |

> **说明：** R10+R11 是交集实体 TASK\_PET 解析 M:N 关系，两端都是 MUST。

### 10.3 关系详细解读

**R1：USER → USER\_ADDRESS**

- 段1（USER端→中点）：USER端=MANY+MAY → "每个用户可以拥有零个或多个地址"
- 段2（中点→ADDRESS端）：ADDRESS端=ONE+MUST → "每个地址必须属于恰恰一个用户"

**R2：USER → CARE\_PROVIDER（父子类型）**

- 段1（USER端→中点）：USER端=ONE+MAY → "每个用户可以成为零个或恰恰一个宠托师"
- 段2（中点→CP端）：CP端=ONE+MUST → "每个宠托师必须属于恰恰一个用户"

**R3：USER → PET（NT）**

- 段1（USER端→中点）：USER端=MANY+MAY → "每个用户可以拥有零只或多只宠物"
- 段2（中点→PET端）：PET端=ONE+MUST → "每只宠物必须属于恰恰一个用户（NT）"

**R4：USER → CARE\_TASK（NT）**

- 段1（USER端→中点）：USER端=MANY+MAY → "每个用户可以发布零个或多个照料任务"
- 段2（中点→TASK端）：TASK端=ONE+MUST → "每个照料任务必须由恰恰一个用户发布（NT）"

**R5：PET → CARE\_TASK（M:N，需解析）**

- 段1（PET端→中点）：PET端=MANY+MAY → "每只宠物可以参与零个或多个照料任务"
- 段2（中点→TASK端）：TASK端=MANY+MUST → "每个照料任务必须包含一只或多只宠物"

**R6：CARE\_TASK → ORDER（NT）**

- 段1（TASK端→中点）：TASK端=MANY+MAY → "每个照料任务可以产生零个或多个订单"
- 段2（中点→ORDER端）：ORDER端=ONE+MUST → "每个订单必须来自恰恰一个照料任务（NT）"

**R7：CARE\_PROVIDER → ORDER（NT）**

- 段1（CP端→中点）：CP端=MANY+MAY → "每个宠托师可以承接零个或多个订单"
- 段2（中点→ORDER端）：ORDER端=ONE+MUST → "每个订单必须被恰恰一个宠托师承接（NT）"

**R8：ORDER → REVIEW（NT）**

- 段1（ORDER端→中点）：ORDER端=MANY+MAY → "每个订单可以产生零个或多个评价"
- 段2（中点→REVIEW端）：REVIEW端=ONE+MUST → "每个评价必须属于恰恰一个订单（NT）"

**R9：ORDER → EXCEPTION\_EVENT**

- 段1（EXC端→中点）：EXC端=ONE+MUST → "每个异常事件必须属于恰恰一个订单"
- 段2（中点→ORDER端）：ORDER端=MANY+MAY → "每个订单可以触发零个或多个异常事件"
- **注意：** 此关系与常规1:N方向相反，读作"从EXC向ORDER看"

**R12：CARE\_TASK → USER\_ADDRESS**

- 段1（TASK端→中点）：TASK端=ONE+MUST → "每个照料任务必须在恰恰一个服务地址"
- 段2（中点→ADDR端）：ADDR端=MANY+MAY → "每个用户地址可以服务零个或多个照料任务"

### 10.4 阅读示例

```
R1 每个 USER 可以 拥有 零个或多个 ADDR      ← USER端：虚线+鸦脚
   每个 ADDR 必须 属于 恰恰一个 USER        ← ADDR端：实线+单竖线

R3 每个 USER 可以 拥有 零只或多只 PET        ← USER端：虚线+鸦脚
   每只 PET 必须 属于 恰恰一个 USER（NT）   ← PET端：实线+单竖线+NT

R9 每个 EXC 必须 属于 恰恰一个 ORDER        ← EXC端：实线+单竖线
   每个 ORDER 可以 触发 零个或多个 EXC      ← ORDER端：虚线+鸦脚
```

***

## 十一、聊天侧输出模板

每次生成 `.drawio` 文件后，在聊天中提供以下内容：

### 1. 关系阅读（ERD Reading）

```
每个 USER 可以 拥有 零个或多个 PET
每只 PET 必须 属于 恰恰一个 USER（NT）
```

### 2. 属性符号速查

| 符号   | 含义              |
| ---- | --------------- |
| `#`  | UID / 主键        |
| `*`  | 必填属性（Mandatory） |
| `o`  | 可选属性（Optional）  |
| `#*` | 主键且必填           |

### 3. 术语对照

| 英文               | 中文          |
| ---------------- | ----------- |
| Entity           | 实体          |
| UID              | 唯一标识符/主键    |
| Optionality      | 可选性（实线/虚线）  |
| Cardinality      | 基数性（单竖线/鸦脚） |
| Mandatory        | 必须参与（实线）    |
| Optional         | 可选参与（虚线）    |
| Crow's Foot      | 鸦脚记法        |
| Non-transferable | 不可转移关系      |

### 4. 建模步骤回顾

```
识别实体 → 识别属性 → 确定UID → 识别关系
→ 确定可选性和基数性 → 处理M:N → 布局设计 → 验证
```

### 5. 布局策略说明

- **画布：** 小规模（≤9实体）1500×1500，大规模 2400×1800
- **实体分布：** 三列紧凑布局 或 星型分布
- **关系线：** 正交折线 + 中点分隔法（实虚线分段），中点作为源实体子元素
- **颜色：** 🟢 马卡龙绿（必须 #7BC8A4）/ 🩷 马卡龙粉（可选 #F4A8B0）
- **NT标记：** 🟠 橙色菱形 `#E67E22`，放置在靠近"必须"端实体的线段上

***

***

## 十二、示例文件与模板

Skill 包中包含完整的示例文件和空白模板，方便快速上手。

### 12.1 文件结构

```
database-er-diagram/
├── SKILL.md                    # 主 Skill 文档（本文档）
├── README.md                   # 对外说明文档（快速开始+规范速查）
├── examples/                   # 示例文件目录
│   ├── README.md              # 示例说明文件（实体清单+关系清单）
│   └── 宝贝在家-ER图.drawio   # 完整项目示例（9实体+12关系）
└── templates/                  # 模板目录
    └── blank-template.drawio   # 空白模板，包含全部7种关系类型
```

### 12.2 如何使用

| 场景          | 操作                                                                                         |
| ----------- | ------------------------------------------------------------------------------------------ |
| **学习参考**    | 在 VS Code 中安装 `hediet.vscode-drawio` 插件，双击打开 `examples/宝贝在家-ER图.drawio` 查看完整的 9 实体 12 关系实现 |
| **快速开始新项目** | 复制 `templates/blank-template.drawio`，修改实体名称和属性，添加关系连线                                      |
| **参考关系实现**  | 打开完整示例，查看 NT 菱形（3段edge）、中点分隔法（实虚线分段）、交集实体等复杂关系的 XML 实现                                     |

### 12.3 示例亮点

- **三列紧凑布局**：左列(USER→ADDR)、中列(CP→TASK→ORDER→EXC)、右列(PET→TP→REVIEW)
- **5个NT关系**：R3/R4/R6/R7/R8，均使用3段edge+橙色空心菱形（菱形为实体子元素）
- **交集实体**：TASK\_PET 解析 PET↔TASK 的 M:N 关系
- **反向关系**：R9(ORDER↔EXC) 展示常规1:N的反向读法
- **子元素中点/NT菱形**：中点作为实体子元素，NT菱形作为目标实体子元素，拖动实体时自动跟随

***

## 附录：教材参考与术语对照

### A. 教材参考章节

| 章节                | 内容         | 教材文件                       |
| ----------------- | ---------- | -------------------------- |
| DFo\_3\_1 第4页     | 课程目标       | DFo\_3\_1\_关系阐述教学.pptx.pdf |
| DFo\_3\_1 第5-6页   | 限定关系       | DFo\_3\_1\_关系阐述教学.pptx.pdf |
| DFo\_3\_1 第7-14页  | M:M关系与交集实体 | DFo\_3\_1\_关系阐述教学.pptx.pdf |
| DFo\_3\_1 第15-18页 | 不可转移关系     | DFo\_3\_1\_关系阐述教学.pptx.pdf |
| DFo\_3\_1 第19-31页 | 父类型和子类型    | DFo\_3\_1\_关系阐述教学.pptx.pdf |
| DFo\_3\_1 第32页    | 分层关系       | DFo\_3\_1\_关系阐述教学.pptx.pdf |
| DFo\_3\_1 第33-37页 | 递归关系       | DFo\_3\_1\_关系阐述教学.pptx.pdf |
| DFo\_3\_1 第38-43页 | 弧关系        | DFo\_3\_1\_关系阐述教学.pptx.pdf |
| DFo\_3\_1 第44页    | 小结         | DFo\_3\_1\_关系阐述教学.pptx.pdf |
| DFo\_2\_6         | 实体、属性、关系   | DFo\_2\_6教学.pptx.pdf       |
| DFo\_3\_2         | 实体关系建模     | DFo\_3\_2\_ER建模.pptx.pdf   |

### B. 规范化理论速查

| 范式  | 要求                         | 示例                                  |
| --- | -------------------------- | ----------------------------------- |
| 1NF | 所有属性原子化，无重复列组              | ❌ 一个"电话号码"列存多个号码                    |
| 2NF | 满足1NF + 所有非主属性**完全依赖于**主键  | 适用于复合主键，消除部分依赖                      |
| 3NF | 满足2NF + 所有非主属性**不传递依赖于**主键 | ✅ ORDER表只保留provider\_id(FK)，不存宠托师姓名 |

### C. ERDish标准语法

根据Oracle Academy教材第51页，ER图关系的标准阅读语法为：

```
每个 实体1 【必须 | 可能】 关系名 【一个或多个 | 一个且仅一个】 实体2
```

**关键原则：**

- 每一条关系线必须能从两个方向分别阅读，两个方向的读法是**独立**的
- **实线（MUST BE）= 必须参与 = 至少1个**
- **虚线（MAY BE）= 可选参与 = 可以0个**

***

*本文档 v12.0 整合自 Oracle Academy DFo 2-6/3-1/3-2 教材标准、draw\.io Crow's Foot 专业实现、星型/三列紧凑/两列模板三种布局优化策略、马卡龙色系系统、子类型白色分隔线规范，以及宝贝在家项目实战经验。*具体服务于深圳信息职业技术大学计算机应用技术专业数据库设计与实现课程
