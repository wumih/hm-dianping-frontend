<!--
  黑马点评 — 前端 (hmdp)
  说明：此 README 面向前端开发与部署，包含运行、部署、以及与后端解耦和高并发优化建议。
-->
🔗 **相关项目**：
- [前端项目仓库](https://github.com/wumih/hm-dianping-frontend.git)
- [后端项目仓库](https://github.com/wumih/hm-dianping.git)
# 黑马点评 — 移动端点评平台（前端） 🚀

> 轻量化原生 HTML/JS/CSS 前端，基于 Vue 2.x 风格的开发模式，配合 Spring Boot 后端与 Redis 实战教程，面向高并发、高性能场景。

---

**项目简介**

- 本项目为移动端点评平台前端：一个高性能、高并发场景下的静态前端工程，适用于秒杀、Feed 推送、商户搜索与评价等业务。📱⚡
- 前端完全采用原生静态资源（HTML/CSS/JS），使用 Vue 2.x 的轻量化开发方式（通过 `vue.js` 引入）与 `axios` 进行 API 通信，部署在 Nginx 上并通过反向代理与后端对接，实现前后端解耦。

---

**核心功能展示** ✨

- 首页商户展示与搜索：快速渲染商户卡片，提供关键字与分类过滤。
- 商户详情与评价：查看商户信息、商品/菜品与用户评价；支持发表、编辑评论。
- 优惠券秒杀（抢购）：前端适配高并发抢购场景，页面展示倒计时与抢购结果反馈。
- 个人中心与登录/签到：支持账号登录（基于后端会话 / token）、签到领取积分或奖励。
- 关注与 Feed 流：用户关注商户或作者，首页/时间线推送关注相关的 Feed 内容。

每个功能都通过 RESTful API 与后端交互，前端仅负责表现与交互逻辑，业务与数据由后端提供。

---

**技术栈清单** 🧰

- Vue 2.x（通过 script 引入，不依赖构建工具）
- Axios（用于 API 请求、错误与授权拦截）
- 原生 HTML/CSS（响应式移动端布局）
- Nginx（用于静态资源托管和反向代理）

---

**项目结构（概要）**

- `index.html`, `shop-list.html`, `shop-detail.html`, `login.html` 等页面
- `css/`：样式文件
- `js/`：`vue.js`, `axios.min.js`, `common.js`, `element.js` 等
- `imgs/`：静态图片资源

（项目顶层目录即为 `hmdp` 文件夹）

---

**快速运行与部署** 🛠️

本项目为静态前端，只需将 `hmdp` 目录放入 Nginx 的静态根目录即可：

1. 将 `hmdp` 文件夹复制到 Nginx 的 `html` 目录下（例如 `d:\nginx\html\hmdp` 或你的 Nginx 安装路径下的 `html`）。

2. 修改 Nginx 配置以反向代理 API 到后端（Spring Boot）。示例配置：

```nginx
server {
	listen       80;
	server_name  your.domain.com;

	root /path/to/nginx/html; # 指向包含 hmdp 的 html 目录

	location / {
		# 默认返回静态页面
		try_files $uri $uri/ /hmdp/index.html;
	}

	# 将 /api 请求代理到后端 Spring Boot 服务
	location /api/ {
		proxy_pass http://127.0.0.1:8080/; # 修改为你的后端地址
		proxy_set_header Host $host;
		proxy_set_header X-Real-IP $remote_addr;
		proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
		proxy_connect_timeout 5s;
		proxy_read_timeout 30s;
	}
}
```

3. 前端 Axios 配置（建议）：将请求前缀指向 `/api`，以利用 Nginx 的反向代理：

```javascript
// js/axios-config.js
axios.defaults.baseURL = '/api';

// 全局请求拦截器（示例）
axios.interceptors.request.use(config => {
  // 可以在此注入 token
  const token = localStorage.getItem('token');
  if (token) config.headers.Authorization = `Bearer ${token}`;
  return config;
}, error => Promise.reject(error));

// 全局响应拦截器（错误处理示例）
axios.interceptors.response.use(res => res, err => {
  if (err.response && err.response.status === 401) {
	// 未授权，跳转登录或刷新 token
	window.location.href = '/hmdp/login.html';
  }
  return Promise.reject(err);
});
```

4. 重启或重载 Nginx：

```bash
# Linux / macOS
nginx -s reload

# Windows 示例（将 nginx.exe 路径替换为你的安装路径）
C:\path\to\nginx.exe -s reload
```

---

**前端与后端的解耦（重点说明）** 🔌

- 前端仅负责 UI 与交互：所有业务逻辑、数据操作、并发控制、限流、秒杀逻辑均在后端实现。
- 前端通过 `axios` 统一封装请求入口（`baseURL` + 拦截器），与后端保持契约化的 API（JSON 格式）。
- 优势：前后端完全解耦，前端可独立由 UI 团队维护或替换，后端可横向扩展（如使用 Redis 做缓存/限流/消息队列），从而提升整体并发处理能力。

示例：搜索与秒杀流程（简述）

- 搜索：前端发送查询参数 -> 后端返回分页化结果 -> 前端负责渲染并做本地缓存或防抖处理。
- 秒杀：前端发起抢购请求 -> 后端进行令牌桶/库存校验 -> 后端返回抢购结果；前端仅做状态展示与重试提示，不做核心并发控制。

---

**高并发与性能优化建议** ⚙️

- 静态资源：启用 gzip/ Brotli 压缩、合理设置 `Cache-Control` 与文件指纹（版本号）以提高命中率。
- CDN：将图片与静态大文件上 CDN，降低单机带宽压力。
- Nginx：开启连接复用、优化 `worker_connections` 与 `worker_processes`，并为 API 请求设置合理的超时与重试策略。
- 前端：对于频繁请求使用防抖/节流、分页加载和占位渲染（skeleton），减少首屏阻塞。
- 后端（建议）：使用 Redis 做缓存与分布式限流、异步下单和消息队列解耦秒杀压力。

---

**常见问题与排查** 🩺

- 页面 404：确认 Nginx 的 `root` 与 `try_files` 配置是否正确；对于 SPA 类页面确保回退到 `index.html`。
- 接口跨域/401：建议统一由 Nginx 反向代理到后端以避免 CORS；检查 Token 的存储与刷新逻辑。
- 静态资源未更新：清理浏览器缓存或采用版本化文件名（例如 `main.v1.2.css`）。

---

**贡献与联系** 🤝

- 如需我帮助：我可以进一步添加 `CONTRIBUTING.md`、示例 Nginx conf（含 https）、或把 Axios 封装成模块并拆分到 `js/` 中。
- 作者 / 维护：在项目根目录留下联系信息或 issue 指引，便于协作。

---

感谢使用「黑马点评」前端！


