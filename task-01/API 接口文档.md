# API 接口文档

> 当前任务 API 文档。
>
> 如果 Apifox 可以访问，以 Apifox 为准。
>
> 如果 Apifox 无法访问，以本文档为准。

---

# 1. API 基础信息

## Base URL

```text
https://api.example.com
```

---

# 2. 用户列表

## 获取用户列表

### Request

```http
GET /api/users
```

### Query

| 参数 | 类型 | 必填 | 说明 |
|---|---|---:|---|
| page | number | 是 | 当前页 |
| pageSize | number | 是 | 每页数量 |
| keyword | string | 否 | 搜索关键词 |
| status | number | 否 | 用户状态 |

### Example

```http
GET /api/users?page=1&pageSize=20&keyword=test
```

---

## Response

```json
{
  "code": 0,
  "message": "success",
  "data": {
    "list": [
      {
        "id": "10001",
        "name": "Test User",
        "avatar": "",
        "status": 1
      }
    ],
    "page": 1,
    "pageSize": 20,
    "total": 100
  }
}
```

---

# 3. 用户详情

### Request

```http
GET /api/users/:id
```

### Path

| 参数 | 类型 | 必填 | 说明 |
|---|---|---:|---|
| id | string | 是 | 用户 ID |

### Response

```json
{
  "code": 0,
  "message": "success",
  "data": {
    "id": "10001",
    "name": "Test User",
    "avatar": "",
    "status": 1
  }
}
```

---

# 4. 修改用户状态

### Request

```http
POST /api/users/status
```

### Body

```json
{
  "id": "10001",
  "status": 1
}
```

### Response

```json
{
  "code": 0,
  "message": "success",
  "data": null
}
```

---

# 5. 错误码

| code | 说明 |
|---:|---|
| 0 | 成功 |
| 401 | 未登录 |
| 403 | 无权限 |
| 404 | 数据不存在 |
| 500 | 服务异常 |

---

# 6. 分页规则

如果接口存在分页：

```text
page 从 1 开始。

pageSize 默认 20。

当：

当前列表长度 >= total

则认为没有更多数据。
```

---

# 7. 特殊说明

填写当前业务特殊规则。

例如：

```text
status：

1 = 正常
2 = 禁用
3 = 删除
```

例如：

```text
金额字段统一使用字符串。
```

例如：

```text
时间统一返回 Unix timestamp。
```