# postman-json-server-demo

一个基于 `json-server`、Postman 和 Newman 的接口测试练习项目，用于模拟本地 REST API，并完成接口设计、环境变量管理、接口串联、负向测试、数据驱动测试和命令行自动化执行。

## 项目目标

这个项目用于练习接口测试的完整基础流程：

- 搭建本地 REST API 服务
- 使用 Postman 编写接口请求和断言
- 使用环境变量管理测试数据
- 使用动态变量串联接口
- 使用 Runner 批量执行测试
- 使用 Newman 在命令行运行测试
- 使用 JSON 数据文件进行数据驱动测试
- 生成 HTML 测试报告
- 使用 Git 和 GitHub 管理项目版本

## 技术栈

```text
Node.js
npm
json-server
Postman
Newman
newman-reporter-htmlextra
Git / GitHub
PowerShell
```

## 项目结构

```text
postman-json-server-demo/
├─ data/
│  └─ posts.json
├─ export/
│  ├─ collection/
│  │  └─ json-server-postman-tests.postman_collection.json
│  └─ environment/
│     └─ json-server-local.postman_environment.json
├─ db.json
├─ package.json
├─ package-lock.json
└─ README.md
```

说明：

- `db.json`：`json-server` 使用的本地数据源。
- `export/collection`：Postman Collection 导出文件。
- `export/environment`：Postman Environment 导出文件。
- `data/posts.json`：数据驱动测试使用的数据文件。
- `reports/`：HTML 测试报告输出目录，已通过 `.gitignore` 忽略。
- `node_modules/`：依赖目录，已通过 `.gitignore` 忽略。

## 安装依赖

第一次拿到项目后执行：

```powershell
npm install
```

## 启动接口服务

打开一个 PowerShell 窗口，执行：

```powershell
npm run start:api
```

服务启动后不要关闭这个窗口。

接口地址：

```text
http://localhost:3000
```

当前本地 API 包含两个资源：

```text
/posts
/users
```

常用接口：

```text
GET    /posts
POST   /posts
GET    /posts/:id
PATCH  /posts/:id
DELETE /posts/:id

GET    /users
POST   /users
GET    /users/:id
PATCH  /users/:id
DELETE /users/:id
```

## Postman 文件

导出的 Postman 文件在：

```text
export/collection/json-server-postman-tests.postman_collection.json
export/environment/json-server-local.postman_environment.json
```

导入 Postman 后，选择环境：

```text
json-server-local
```

## 环境变量

当前环境使用这些变量：

```text
base_url             http://localhost:3000

post_id              运行时动态生成
new_post_title       postman chain test
new_post_body        created for api chaining
updated_post_title   postman chain test updated

user_id              运行时动态生成
new_user_name        Charlie
new_user_email       charlie@example.com
updated_user_name    Charlie Updated
```

注意：导出 Environment 给 Newman 使用时，静态变量需要填写到 Postman 的 `Shared Value`，否则导出的环境文件可能没有值。`post_id` 和 `user_id` 是运行时动态生成的，可以为空。

## 测试覆盖范围

### posts 文章接口

```text
POST   /posts
GET    /posts/{{post_id}}
PATCH  /posts/{{post_id}}
DELETE /posts/{{post_id}}
GET    /posts/{{post_id}}
GET    /posts/not-exist-id
DELETE /posts/not-exist-id
```

覆盖内容：

- 新增文章
- 查询刚新增的文章
- 修改刚新增的文章
- 删除刚新增的文章
- 删除后再次查询，确认返回 `404`
- 查询不存在文章，确认返回 `404`
- 删除不存在文章，确认返回 `404`

### users 用户接口

```text
POST   /users
GET    /users/{{user_id}}
PATCH  /users/{{user_id}}
DELETE /users/{{user_id}}
GET    /users/{{user_id}}
GET    /users/not-exist-id
DELETE /users/not-exist-id
```

覆盖内容：

- 新增用户
- 查询刚新增的用户
- 修改刚新增的用户
- 删除刚新增的用户
- 删除后再次查询，确认返回 `404`
- 查询不存在用户，确认返回 `404`
- 删除不存在用户，确认返回 `404`

### 数据驱动测试

数据文件：

```text
data/posts.json
```

数据驱动请求：

```text
POST /posts
```

覆盖内容：

- 使用 JSON 文件中的多组 `title` 和 `body` 创建文章
- 使用 `pm.iterationData.get()` 断言响应内容来自当前迭代数据

## 命令行运行测试

服务启动后，另开一个 PowerShell 窗口，执行：

```powershell
npm run test:api
```

当前主流程测试结果：

```text
requests:   14 failed: 0
assertions: 44 failed: 0
```

## 运行数据驱动测试

执行：

```powershell
npm run test:api:data
```

当前数据驱动测试结果：

```text
iterations: 2 failed: 0
requests:   2 failed: 0
assertions: 6 failed: 0
```

## 生成 HTML 报告

执行：

```powershell
npm run test:api:report
```

报告会生成到：

```text
reports/api-test-report.html
```

打开报告：

```powershell
Invoke-Item .\reports\api-test-report.html
```

## 项目亮点

- 使用 Postman Environment 管理 `base_url` 和测试数据。
- 使用 `post_id` 和 `user_id` 实现接口串联。
- 覆盖 `posts` 和 `users` 两个资源模块。
- 覆盖正向 CRUD 流程和资源不存在的负向场景。
- 使用 Newman 实现命令行接口测试。
- 使用 `newman-reporter-htmlextra` 生成 HTML 测试报告。
- 使用 JSON 数据文件实现数据驱动测试。
- 使用 Git 和 GitHub 管理项目版本。

## 学习成果

通过这个项目，练习了：

- Postman 请求创建
- Postman Scripts / Post-response 断言
- 状态码断言和响应字段断言
- Environment 变量管理
- 接口串联和动态变量传递
- 正向测试和负向测试
- Collection Runner 批量执行
- Newman 命令行运行
- HTML 测试报告生成
- JSON 数据驱动测试
- Git 和 GitHub 基础发布流程
