V免签 Fox
===========

这个 `main` 分支现在合并了两套后端实现：

- PHP / ThinkPHP 版本
- Go 版本

## 目录说明

- PHP 实现仍然保留在当前仓库原有结构中，例如 `app/`、`config/`、`route/`、`public/`
- Go 实现在 `cmd/`、`internal/`、`pkg/`，入口是 `cmd/server/main.go`

## 默认部署

当前仓库里的 `docker-compose.yml` 默认仍以 PHP 版本为主：

- `backend`: nginx 反向代理
- `vmqfox-backend`: 本地构建的 PHP-FPM 服务
- `mysql`
- `redis`

启动：

```bash
docker compose up -d --build
```

## Go 版本

Go 版本代码已经并入 `main`，主要文件包括：

- `go.mod`
- `go.sum`
- `config.example.yaml`
- `cmd/server/main.go`
- `internal/`
- `pkg/`

如果你要运行 Go 版本，需要根据 Go 实现自己的配置和启动方式单独部署。

## 当前 main 分支包含的额外修复

- 修复订单 `state=2` 被错误显示为未支付/未知状态
- 放宽异步通知 `success` 响应判断
- 保留新版/旧版通知兼容逻辑
- 默认前端地址支持回退到当前域名
- `docker-compose.yml` 改为本地构建 PHP 后端，避免本地代码修改不生效

## 监控原理

本项目仍然基于监控端监听手机通知栏收款消息：

1. 用户扫码付款
2. 手机收到微信/支付宝收款通知
3. 监控端将消息推送到服务端
4. 服务端按金额和支付方式匹配订单

## 说明

- iOS 无法等价实现安卓通知监听版监控端
- 如果要长期稳定商用，建议改为官方商户回调方案
