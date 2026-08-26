<div align="center">
  <h1>DigitalPlat 免费域名自动续期</h1>
  <p>每周自动检查 DigitalPlat 域名，到期前自动免费续期</p>
  <p>简体中文 | <a href="README.en.md">English</a></p>
  <p>
    <img alt="Python" src="https://img.shields.io/badge/python-3.10%2B-3776AB">
    <img alt="Platform" src="https://img.shields.io/badge/platform-GitHub%20Actions-2088FF">
    <img alt="License" src="https://img.shields.io/badge/license-MIT-111827">
    <img alt="Schedule" src="https://img.shields.io/badge/schedule-Weekly-22c55e">
  </p>
</div>

> 只需 3 分钟部署，之后每周自动检查并续期你的 DigitalPlat 免费域名。

## 3 分钟部署

### 第 0 步：创建 DigitalPlat API Token

打开：

* <https://dash.domain.digitalplat.org/dashboard/api/keys>

创建一个 API Key，通常是 `dp_live_...` 开头。文档页位于：

* <https://dash.domain.digitalplat.org/dashboard/api/docs>

本项目使用 Bearer Token 调用 DigitalPlat API。

### 第 1 步：Fork 本仓库

1. 登录 GitHub。
2. 打开：<https://github.com/xz0609/digitalplat-auto-renew/fork>
3. 点击 `Create fork`，通常几秒到几十秒完成。

### 第 2 步：配置 GitHub Secret 和 Variable

进入：

* `Settings -> Secrets and variables -> Actions`

添加 Secret：

* `DIGITALPLAT_API_TOKEN`
* `DIGITALPLAT_DOMAINS`

`DIGITALPLAT_DOMAINS` 一行一个域名：

```text
example.dpdns.org
example.qzz.io
```

> `DIGITALPLAT_DOMAINS` 故意放在 Secret 而不是 Variable，是为了避免域名以明文出现在 Actions 运行日志里；脚本运行时也会对每个域名调用 `::add-mask::` 进一步脱敏。

可选 Variable：

* `DIGITALPLAT_RENEW_BEFORE_DAYS`：默认 `120`

### 第 3 步：手动运行一次

打开 GitHub 的 `Actions`，手动运行 `DigitalPlat Auto Renew`。

## 续期规则

默认规则：

* 免费续期窗口：到期前 `120` 天

* 每周检查一次

* 只有进入窗口后才会请求续期

* 默认调用 `renewal_type=free`、`years=1`

如果域名还没进入窗口，脚本只记录检查状态，不会调用续期接口。

## 文件说明

* `scripts/digitalplat_auto_renew.py`：续期脚本

* `.github/workflows/digitalplat-auto-renew.yml`：每周 GitHub Actions 工作流

## API 说明

当前实现使用 DigitalPlat Domain API：

* `GET /domains`

* `GET /domains/{domain}`

* `POST /domains/{domain}/renew`

默认 API Base：

* `https://domain-api.digitalplat.org/api/v1`

如果官方文档后续调整 API Base，可在 GitHub Variables 里添加 `DIGITALPLAT_API_BASE` 并同步修改 workflow 环境变量。

## 许可证

本项目使用 MIT License。
