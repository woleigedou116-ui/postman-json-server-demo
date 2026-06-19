# postman-json-server-demo

一个用于练习 Postman 接口测试的本地 API 项目。

## 项目内容

- 使用 `json-server` 根据 `db.json` 启动本地 REST API
- 使用 Postman Collection 测试文章接口
- 使用 Environment 管理 `base_url` 和测试数据
- 使用 Newman 在命令行运行接口测试
- 使用 HTML 报告保存测试结果

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

常用接口：

```text
GET    /posts
POST   /posts
GET    /posts/:id
PATCH  /posts/:id
DELETE /posts/:id
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
```

注意：导出 Environment 给 Newman 使用时，静态变量需要填写到 Postman 的 `Shared Value`，否则导出的环境文件可能没有值。

## 命令行运行测试

服务启动后，另开一个 PowerShell 窗口，执行：

```powershell
npm run test:api
```

成功时应看到：

```text
requests:   6 failed: 0
assertions: 20 failed: 0
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

## 测试流程

Collection Runner 和 Newman 会按顺序执行：

```text
1. 新增文章
2. 查询文章
3. 修改文章
4. 删除文章
5. 确认删除文章
6. 查询不存在文章
```

前 5 个请求是完整的正向链路，第 6 个请求是负向测试。
