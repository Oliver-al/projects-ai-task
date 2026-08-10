# AGENTS.md

# 项目级 AI Agent 开发规范

> 本文件是当前项目的 AI Coding 全局规范。
>
> 适用于 TRAE Agent、AI Coding Agent 以及其他自动化代码开发工具。
>
> 本文件定义“项目应该怎么开发”。
>
> 具体任务由：
>
> ```text
> docs/ai-tasks/{task}/TASK.md
> ```
>
> 定义。
>
> **优先级：项目现有代码 > AGENTS.md > TASK.md > API 文档 > 原型 > AI 自行推测**

---

# 1. AI 开发总原则

AI 在修改代码之前，必须先理解当前项目。

禁止：

```text
没有分析项目就直接编码
```

必须：

```text
分析项目
    ↓
寻找已有实现
    ↓
寻找可复用组件
    ↓
寻找类似页面
    ↓
寻找已有 API
    ↓
理解数据流
    ↓
制定实现方案
    ↓
开始编码
```

---

# 2. 核心开发原则

## 2.1 优先复用

项目中已经存在的代码，优先复用。

优先级：

```text
已有页面
    ↓
已有业务组件
    ↓
已有公共组件
    ↓
已有 Hooks
    ↓
已有 Utils
    ↓
已有 API
    ↓
已有 Store
    ↓
新增代码
```

---

# 2.2 不重复造轮子

如果项目已经存在类似功能：

```text
Button
Popup
Modal
Dialog
List
Table
Pagination
Tabs
Form
Input
Select
Loading
Empty
Toast
Message
```

必须优先使用。

禁止为了当前任务重新创建：

```text
NewButton
CustomButton
MyPopup
NewPopup
CustomList
NewRequest
```

除非现有实现确实无法满足需求。

---

# 2.3 最小修改原则

只修改完成当前需求所需要的代码。

禁止：

```text
顺手重构
顺手优化
顺手升级依赖
顺手修改公共组件
顺手修改其他页面
```

例如：

当前任务：

```text
新增用户列表页面
```

禁止同时：

```text
重构 UserStore
重构 Request
修改全局 Button
修改 ESLint
升级 React
升级依赖
```

除非任务明确要求。

---

# 3. 开发前项目分析

开始编码之前必须搜索：

```text
src/
components/
pages/
views/
hooks/
services/
api/
store/
utils/
assets/
```

重点寻找：

```text
相似页面
相似业务
相似组件
相似接口
相似数据结构
相似交互
```

---

# 4. 相似页面复用

如果已经存在相似页面：

```text
优先复制 / 扩展现有页面
```

推荐流程：

```text
参考页面
    ↓
复制结构
    ↓
替换业务数据
    ↓
替换 API
    ↓
调整 UI
    ↓
增加当前业务逻辑
```

禁止：

```text
完全重新设计
```

---

# 5. 组件开发规范

## 5.1 使用现有组件

开发前必须搜索现有组件。

例如：

```text
Button
Popup
Modal
Dialog
Form
Input
Select
Tabs
Table
List
Pagination
Loading
Empty
Toast
```

存在则复用。

---

## 5.2 新增组件

只有以下情况才允许新增：

```text
项目中不存在
+
当前页面确实需要
+
无法通过已有组件配置解决
```

新增组件必须：

```text
职责单一
命名清晰
目录合理
符合现有代码风格
```

---

# 6. 公共组件修改

公共组件属于高风险代码。

修改之前必须确认：

```text
1. 当前组件是否可以通过 Props 解决？
2. 是否可以增加配置？
3. 是否可以通过扩展解决？
4. 是否会影响其他页面？
```

优先级：

```text
Props
 ↓
Config
 ↓
扩展
 ↓
公共组件修改
```

如果修改公共组件：

最终报告必须说明：

```text
修改原因
影响范围
兼容性
```

---

# 7. API 开发规范

## 7.1 API 来源

优先级：

```text
Apifox
 ↓
api.md
 ↓
现有项目 API
 ↓
AI 推测
```

禁止直接猜 API。

---

# 7.2 API 请求封装

项目已经存在请求封装时：

```text
必须复用。
```

例如：

```ts
request()
http()
apiClient()
axiosInstance()
```

禁止重新创建：

```ts
axios.create()
```

或新的：

```text
request wrapper
```

---

# 7.3 API 类型

优先复用已有类型。

例如：

```ts
User
UserInfo
UserResponse
Pagination
ApiResponse
```

不要创建重复类型。

---

# 8. 数据流规范

必须遵循项目现有的数据流。

例如：

```text
API
 ↓
Service
 ↓
Store
 ↓
Hook
 ↓
Page
 ↓
Component
```

如果项目采用其他结构：

```text
按照现有结构。
```

不要为了当前任务引入新的架构。

---

# 9. 状态管理

如果项目已经使用：

```text
Redux
Zustand
Pinia
MobX
Context
Vuex
```

必须继续使用当前方案。

禁止因为一个页面而引入新的状态管理工具。

---

# 10. 页面状态

异步页面至少考虑：

```text
Initial
Loading
Success
Empty
Error
```

列表页面：

```text
Initial
Loading
Success
Empty
LoadingMore
NoMore
Error
```

---

# 11. 分页

如果 API 存在分页：

必须正确处理：

```text
page
pageSize
total
hasMore
loading
loadingMore
```

加载更多必须：

```text
追加数据
```

不能：

```text
覆盖旧数据
```

同时防止：

```text
重复请求
重复数据
并发请求
```

---

# 12. Loading

Loading 必须与请求生命周期绑定。

禁止：

```text
请求失败后 Loading 一直存在
```

或者：

```text
请求结束但 Loading 没有关闭
```

---

# 13. Empty

列表没有数据时：

必须根据项目已有 Empty 组件处理。

禁止：

```text
直接 return null
```

除非当前项目就是这样设计的。

---

# 14. Error

接口失败必须遵循项目已有错误处理方式。

例如：

```text
Toast
Message
Error Page
Retry
```

不要自行创建新的错误处理机制。

---

# 15. Popup / Modal

如果项目已经存在：

```text
PopupManager
ModalManager
DialogManager
```

必须复用。

不要重新创建生命周期。

---

# 16. 路由

使用项目已有：

```text
Router
Navigation
Navigator
Route
```

不要自行创建路由机制。

必须确认：

```text
进入
参数
返回
刷新
销毁
```

---

# 17. UI / 原型

原型负责：

```text
页面结构
布局
视觉
交互
```

项目现有组件负责：

```text
实现方式
```

所以：

```text
原型要求
+
现有组件体系
```

共同决定最终实现。

---

# 18. 资源

使用项目已有：

```text
assets
public
static
resources
images
icons
```

优先复用。

禁止：

```text
随意 CDN
随意网络图片
随意第三方 Icon
```

---

# 19. TypeScript

遵循项目现有 TypeScript 规范。

禁止：

```ts
any
```

除非确实必要。

优先：

```ts
interface
type
enum
泛型
类型守卫
```

复用已有类型。

---

# 20. 命名

遵循当前项目命名风格。

例如项目采用：

```text
PascalCase
camelCase
kebab-case
```

不要混用。

组件：

```text
UserList
UserDetail
UserPopup
```

Hooks：

```text
useUserList
useUserDetail
```

API：

```text
getUserList
getUserDetail
updateUser
```

---

# 21. CSS / 样式

必须遵循当前项目已有方案：

```text
CSS
SCSS
LESS
CSS Modules
Tailwind
Styled Components
```

不要引入新的 CSS 方案。

优先复用：

```text
变量
Mixins
主题
设计 Token
公共样式
```

---

# 22. 响应式

如果项目已有响应式方案：

```text
必须复用。
```

不要自行创造新的：

```text
breakpoint
media query
尺寸系统
```

---

# 23. 国际化

如果项目存在 i18n：

禁止直接写：

```tsx
<button>确定</button>
```

必须使用项目现有 i18n：

```ts
t('xxx')
```

新增文案必须：

```text
符合现有 key 命名规范
```

并根据项目实际支持语言补充翻译。

---

# 24. 日志

开发完成后：

禁止残留：

```ts
console.log()
console.debug()
console.warn()
```

除非项目本身明确允许。

---

# 25. 错误处理

不要吞掉错误。

禁止：

```ts
try {
  ...
} catch {
}
```

除非明确知道为什么可以忽略。

---

# 26. 性能

不要为了“优化”而过度设计。

只有发现真实问题时才考虑：

```text
memo
useMemo
useCallback
虚拟列表
懒加载
缓存
防抖
节流
```

优先保持代码简单。

---

# 27. 依赖

未经任务明确要求：

禁止：

```text
npm install
pnpm add
yarn add
```

禁止随意升级：

```text
React
Vue
TypeScript
Vite
Webpack
Cocos
Node
```

如果确实需要依赖：

必须在最终报告说明：

```text
新增依赖：
原因：
版本：
影响：
```

---

# 28. Git

代码修改必须保持清晰。

禁止：

```text
大规模无关格式化
```

禁止：

```text
修改无关文件
```

---

# 29. 测试

如果项目存在：

```text
Unit Test
E2E
Integration Test
```

必须遵循已有测试体系。

新增业务逻辑时：

优先增加必要测试。

---

# 30. Build

完成后尽可能执行项目已有检查：

```bash
pnpm lint
pnpm type-check
pnpm build
```

实际命令必须以项目 `package.json` 为准。

不要自行猜测命令。

---

# 31. 修改文件控制

完成任务后必须检查：

```text
git diff
```

确认：

```text
修改的文件是否都是任务相关文件
```

发现无关修改：

```text
恢复无关修改。
```

不要覆盖用户原本未提交的工作。

---

# 32. 保护用户已有修改

这是非常重要的规则。

执行：

```text
git status
```

之前先确认当前工作区状态。

如果存在用户已有修改：

```text
不要覆盖
不要删除
不要 reset
不要 checkout
```

除非用户明确要求。

---

# 33. 禁止 Git 破坏性操作

禁止自行执行：

```bash
git reset --hard
git clean -fd
git checkout .
git restore .
```

除非用户明确要求。

---

# 34. Agent 工作模式

AI 必须遵循：

```text
分析
 ↓
计划
 ↓
实现
 ↓
检查
 ↓
报告
```

而不是：

```text
看到需求
 ↓
直接疯狂修改代码
```

---

# 35. Task 文件

具体任务位于：

```text
docs/ai-tasks/
```

例如：

```text
docs/ai-tasks/task-001/TASK.md
```

执行任务时必须同时读取：

```text
AGENTS.md
TASK.md
api.md
prototype/*
CHECKLIST.md
```

---

# 36. TASK.md 优先级

`TASK.md` 描述当前具体任务。

例如：

```text
新增用户列表页面
```

如果 TASK 与项目长期规范冲突：

```text
先遵循 AGENTS.md
```

如果用户明确要求改变项目规范：

```text
按照用户当前明确要求执行。
```

---

# 37. 开发前输出

开始大量修改代码之前，应先形成简短方案：

```text
参考页面：
xxx

复用组件：
xxx

API：
xxx

状态：
xxx

新增文件：
xxx

修改文件：
xxx
```

---

# 38. 最终输出

任务完成后必须报告：

## 实现

```text
完成了什么
```

## 文件

```text
新增：
xxx

修改：
xxx

删除：
xxx
```

## API

```text
GET /xxx
POST /xxx
```

## 复用

```text
参考页面：
xxx

组件：
xxx

Hook：
xxx

Store：
xxx
```

## 检查

```text
TypeScript：通过 / 失败

Lint：通过 / 失败

Build：通过 / 失败
```

## TODO

```text
无
```

或者列出：

```text
1. xxx
2. xxx
```

---

# 39. 最终开发哲学

本项目 AI 开发必须遵循：

```text
不要猜
不要乱改
不要重构
不要重复造轮子
不要修改无关代码
```

应该：

```text
先搜索
先理解
先复用
再实现
最后检查
```

最终目标：

> **以最小代码修改完成需求，同时最大程度保持现有项目架构、代码风格和业务逻辑的一致性。**