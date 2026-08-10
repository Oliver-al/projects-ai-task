# AI Coding Task

> 本文件是当前开发任务的唯一主任务说明。
>
> 请严格按照本文档执行。
>
> **核心原则：优先复用现有项目代码，禁止无意义重构，禁止重复造轮子。**

---

# 1. 当前任务

## 任务名称

【填写页面/功能名称】

例如：

```text
用户列表页面
```

## 任务类型

```text
新增页面 / 修改页面 / 新增功能 / Bug 修复
```

## 任务描述

请根据：

```text
原型截图
+
现有项目代码
+
API 接口文档
```

完成当前页面/功能开发。

---

# 2. 任务资料

## 2.1 原型

原型文件位于：

```text
./prototype/
```

请读取该目录下所有相关图片。

重点分析：

- 页面布局
- UI 结构
- 组件层级
- 页面状态
- 用户操作
- 点击行为
- 弹窗
- 列表
- Tab
- 表单
- Loading
- Empty
- Error
- 分页
- 加载更多
- 页面跳转

---

# 2.2 API

优先级：

```text
Apifox
    ↓
api.md
    ↓
现有项目 API 实现
```

### Apifox

```text
【填写 Apifox URL】
```

例如：

```text
https://app.apifox.com/project/8605431
```

### API 本地文档

```text
./api.md
```

如果 Apifox 可以访问：

> 优先以 Apifox 为准。

如果 Apifox 无法访问：

> 使用 api.md。

如果两者存在冲突：

> 优先 Apifox，并在最终报告中说明冲突。

---

# 3. 开发前必须分析现有项目

## ⚠️ 禁止直接开始编码

在开始修改代码之前，必须先分析当前项目。

必须搜索：

```text
页面
组件
Hooks
Utils
API
Service
Store
状态管理
路由
样式
资源
```

重点寻找：

```text
与当前页面最接近的已有页面
```

---

# 4. 页面复用原则

如果项目中已经存在相似页面：

```text
不要从零开发。
```

优先采用：

```text
已有页面
   ↓
复制 / 扩展
   ↓
替换业务数据
   ↓
替换 API
   ↓
调整 UI
   ↓
增加当前业务逻辑
```

而不是：

```text
重新创建页面
重新创建组件
重新创建 API
重新创建 Hook
重新创建 Store
```

---

# 5. 组件复用原则

开发前必须搜索现有公共组件。

例如：

```text
Button
Popup
Modal
Dialog
List
Table
Pagination
Tabs
Input
Select
DatePicker
Loading
Empty
Toast
Message
Form
```

如果项目中已经存在：

```text
必须优先复用。
```

禁止创建功能重复的新组件。

例如已经存在：

```ts
BasePopup
```

禁止再创建：

```ts
UserPopup
CommonPopup
CustomPopup
NewPopup
```

除非业务确实存在明显不同的职责。

---

# 6. API 复用原则

必须使用项目现有 API 请求封装。

例如项目已经存在：

```ts
request()
http()
axios()
fetch()
apiClient()
```

则继续使用。

禁止为了当前页面重新创建：

```ts
axios.create()
fetch wrapper
request wrapper
```

---

# 7. 数据流原则

优先遵循项目已有架构。

例如：

```text
API
 ↓
Service
 ↓
Store / Hook
 ↓
Page
 ↓
Component
```

或者项目本身采用：

```text
Page
 ↓
API
 ↓
State
 ↓
UI
```

则继续使用现有方式。

**不要因为当前页面而引入新的状态管理方案。**

---

# 8. TypeScript 原则

优先使用项目已有类型。

例如已经存在：

```ts
User
UserInfo
UserList
Pagination
ApiResponse
```

必须复用。

不要重新创建：

```ts
UserData
UserResponse2
NewUser
UserListData2
```

除非确实是不同的数据结构。

---

# 9. 页面状态

如果当前页面存在异步请求，必须考虑：

```text
Initial
Loading
Success
Empty
Error
```

如果存在分页：

```text
Initial
Loading
Success
Empty
LoadingMore
NoMore
Error
```

必须正确处理：

```text
loading
loadingMore
page
pageSize
hasMore
error
```

禁止出现：

```text
重复请求
重复加载
分页数据覆盖
分页数据重复
Loading 无法关闭
```

---

# 10. 列表开发规则

如果 API 返回分页数据：

例如：

```json
{
  "page": 1,
  "pageSize": 20,
  "total": 100,
  "list": []
}
```

则实现：

```text
首次进入
    ↓
page = 1
    ↓
请求 API
    ↓
展示数据
    ↓
加载更多
    ↓
page++
    ↓
追加数据
```

注意：

> 加载更多不能覆盖之前的数据。

---

# 11. Popup / Modal 规则

如果当前页面存在 Popup：

优先搜索：

```text
项目现有 Popup
```

如果已有 PopupManager：

```text
必须复用 PopupManager。
```

如果已有 Popup 基础类：

```text
必须继承 / 扩展已有基础类。
```

不要重新设计 Popup 生命周期。

---

# 12. 路由规则

如果当前页面需要跳转：

优先使用现有：

```text
Router
Navigation
Route
Navigator
```

不要创建新的路由机制。

必须确认：

```text
进入页面
返回页面
页面参数
页面刷新
页面销毁
```

是否符合现有项目逻辑。

---

# 13. 原型实现规则

原型截图用于确定：

```text
布局
尺寸
间距
视觉
交互
```

但代码实现必须优先遵循：

```text
现有项目组件体系
+
现有设计规范
+
原型要求
```

如果原型和现有公共组件存在差异：

> 优先通过配置 / Props / 参数解决。

不要轻易修改公共组件。

---

# 14. 图片和资源

开发前必须搜索：

```text
assets/
public/
static/
resources/
images/
icons/
```

如果项目中已经存在对应资源：

```text
直接复用。
```

禁止：

```text
随意使用网络图片
随意引入 CDN
随意增加第三方图标库
```

---

# 15. 文件修改原则

## 只修改必要文件

优先：

```text
当前业务目录
```

不要随意修改：

```text
公共组件
公共工具
构建配置
Webpack/Vite
ESLint
TSConfig
package.json
其他业务页面
```

除非当前任务确实需要。

---

# 16. 公共代码修改规则

如果发现公共组件无法满足当前需求：

按照以下顺序处理：

```text
方案 1：
通过 Props / Config 解决

↓

方案 2：
增加兼容能力

↓

方案 3：
扩展公共组件

↓

方案 4：
最后才修改公共逻辑
```

如果修改公共代码：

必须在最终报告中说明：

```text
修改文件：
修改原因：
影响范围：
是否兼容原有页面：
```

---

# 17. 开发执行流程

必须按照以下流程执行。

## Step 1：扫描项目

搜索：

```text
项目目录
页面
组件
API
Hooks
Utils
Store
```

---

## Step 2：寻找参考页面

找到最接近的已有页面。

输出：

```text
参考页面：
xxx

页面路径：
xxx

复用组件：
xxx
xxx

复用逻辑：
xxx
xxx
```

---

## Step 3：分析原型

输出：

```text
页面结构：
xxx

主要组件：
xxx

交互：
xxx

特殊状态：
xxx
```

---

## Step 4：分析 API

输出：

```text
API：
GET /xxx

参数：
xxx

返回：
xxx

分页：
xxx
```

---

## Step 5：制定实现方案

开始编码前先确定：

```text
页面：
xxx

参考页面：
xxx

复用组件：
xxx

复用 API：
xxx

新增 API：
xxx

新增组件：
xxx

状态：
xxx

交互：
xxx
```

---

## Step 6：执行开发

按照实现方案修改代码。

遵循：

```text
最小修改
最大复用
保持现有架构
保持现有代码风格
```

---

## Step 7：执行检查

开发完成后：

```text
检查 TypeScript
检查 ESLint
检查 API
检查页面状态
检查交互
检查分页
检查 Popup
检查路由
```

如果项目提供：

```text
pnpm lint
pnpm type-check
pnpm build
```

则优先执行对应检查。

---

# 18. 禁止 AI 自行推测

以下内容禁止随意猜测：

```text
API URL
API 参数
Response 字段
枚举值
业务状态
权限
用户角色
金额
时间格式
分页规则
错误码
```

如果无法确定：

优先：

```text
搜索项目现有实现
```

仍然无法确定：

```text
保留 TODO
```

并在最终报告说明。

---

# 19. 禁止行为

禁止：

```text
❌ 没搜索项目就开始写代码

❌ 重复创建已有组件

❌ 重复创建 API 请求封装

❌ 重复创建 Store

❌ 重复创建 Hook

❌ 虚构 API

❌ 虚构字段

❌ 随意修改公共组件

❌ 随意升级依赖

❌ 修改无关页面

❌ 重构整个项目

❌ 为了代码“更优雅”改变现有架构
```

---

# 20. 代码风格

必须遵循当前项目现有代码风格：

包括：

```text
命名
目录结构
组件写法
Hooks
状态管理
API
TypeScript
CSS
SCSS
LESS
错误处理
```

不要引入项目中不存在的新编码风格。

---

# 21. 完成后的最终报告

开发完成后必须输出：

## 1. 实现结果

```text
已完成：
xxx
xxx
xxx
```

## 2. 修改文件

```text
新增：

xxx
xxx

修改：

xxx
xxx

删除：

xxx
```

## 3. API

```text
GET /xxx
POST /xxx
```

## 4. 复用内容

```text
参考页面：
xxx

复用组件：
xxx
xxx

复用 Hook：
xxx

复用 API：
xxx
```

## 5. 检查结果

```text
TypeScript：通过 / 未通过

ESLint：通过 / 未通过

Build：通过 / 未通过

功能检查：通过 / 未通过
```

## 6. TODO

如果存在问题：

```text
TODO：

1. xxx
2. xxx
3. xxx
```

如果没有：

```text
TODO：无
```

---

# 22. 最终原则

请始终遵循：

```text
现有代码
    ↓
现有页面
    ↓
现有组件
    ↓
现有 API
    ↓
现有工具
    ↓
原型
    ↓
当前需求
    ↓
AI 自己实现
```

**能复用，不新增。**

**能扩展，不重写。**

**能配置，不复制。**

**能搜索，不猜测。**

**非必要，不修改公共代码。**

**非必要，不重构项目。**

---

# 23. 开始任务

现在开始执行：

```text
1. 分析项目
2. 寻找参考页面
3. 分析原型
4. 分析 API
5. 输出实现方案
6. 开始编码
7. 自检
8. 输出最终报告
```

**在完成项目分析之前，不要直接开始大规模修改代码。**