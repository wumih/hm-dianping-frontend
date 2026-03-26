# 黑马点评前端 — 更新日志 (Frontend Changelog)

> 本仓库为"黑马点评"项目前端静态资源，由 Nginx 托管。
> 技术栈：Vue 2 (CDN) + Axios + Element UI (CDN)
> 对应后端仓库：[hm-dianping](https://github.com/wumih/hm-dianping)

---

## [v1.3.0] - 2026-03-27 | 个人主页完整交互闭环

### ✨ 新增功能

#### 1. 退出登录按钮 (`info.html`)
- **新增** 页面右上角"退出登录"按钮，绑定 `@click="logout"` 事件。
- **`logout()` 方法实现**：
  - 调用后端 `POST /user/logout` 接口，服务端删除 Redis 中的 Token 凭证。
  - 清理浏览器 `sessionStorage.removeItem("token")`，防止旧 Token 残留。
  - 成功后跳转至首页 `location.href = "/"`，实现完整的前后端双重注销闭环。

#### 2. Feed 流关注列表可点击跳转 (`info.html`)
- **修复**：在"关注"Tab 页的博客卡片中，补全了以下三处原版代码缺失的 `@click` 点击事件，解决了点击无响应的问题：
  - 博客配图：`@click="toBlogDetail(b)"` → 跳转至 `blog-detail.html?id=xxx`
  - 博客标题：`@click="toBlogDetail(b)"` → 跳转至博客详情
  - 用户头像/昵称：`@click="toOtherInfo(b)"` → 跳转至 `other-info.html?id=xxx`（自动过滤自己）
- **新增** `toOtherInfo(b)` 和 `toBlogDetail(b)` 两个完整方法实现，包含防自跳保护（`b.userId !== this.user.id`）。

---

## [v1.2.0] - 2026-03-10 | 双层拦截器架构前端适配

### 🔧 重构

#### Token 认证方式升级 (`common.js`)
- 配合后端从 Session 迁移至 **Redis Token** 认证方案，修改 Axios 全局请求拦截器。
- 登录成功后，将后端返回的 Token 存储至 `sessionStorage.setItem("token", data)`。
- 每次发起请求时，Axios 拦截器自动从 `sessionStorage` 取出 Token 并注入请求头 `Authorization`，实现无感鉴权。

---

## [v1.1.0] - 2026-03-09 | 登录页交互修复

### 🐛 修复

#### 用户协议勾选框无法点击 (`login.html`)
- **症状**：登录页底部的"我已阅读并同意"单选框点击后毫无反应，导致根本无法进入登录流程。
- **根因**：原版 HTML 中 `<input type="radio">` 标签缺少 `id="readed"` 属性，而对应的 `<label>` 标签使用了 `for="readed"` 进行关联。因 `id` 缺失，浏览器无法建立 `label` 与 `input` 的绑定关系，导致点击 `label` 区域不触发 `input` 的选中状态。
- **修复**：为 `<input>` 标签补全 `id="readed"` 属性，`label` 与 `input` 关联恢复正常，勾选框可正常点击。

---

## [v1.0.0] - 2026-03-06 | 初始版本

### ✨ 初始版本

- 完成项目前端基础框架搭建（Vue 2 CDN + Axios + Element UI CDN）
- 实现手机号验证码登录页面（`login.html`）
- 实现首页、商铺列表、商铺详情、博客列表等核心浏览页面
- 通过 Nginx `proxy_pass` 配置，将 `/api/` 请求反向代理至后端 `127.0.0.1:8081`
