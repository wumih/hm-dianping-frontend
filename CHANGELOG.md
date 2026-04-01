# 黑马点评前端 — 更新日志 (Frontend Changelog)

> 本仓库为"黑马点评"项目前端静态资源，由 Nginx 托管。
> 技术栈：Vue 2 (CDN) + Axios + Element UI (CDN)
> 对应后端仓库：[hm-dianping](https://github.com/wumih/hm-dianping)

---

## [v2.10.0] - 2026-04-02 | 前端视觉重构与质感升级体验版

### ✨ 新增功能与 UI 重构

#### 1. 全局 UI “莫兰迪绿” 主题重塑 (`main.css`)
- **新增** 摒弃默认橙红色基调，引入高级文艺的“莫兰迪主题（#609981）”作为主品牌色。
- **设计细节**：采用纯 CSS 拦截覆写技巧，创造性地使用 `hue-rotate` 滤镜技术将无法改色的前端硬编码 PNG 按钮平滑变色；并运用属性选择器拦截核心 SVG 点赞图标色温，实现全站零死角的品牌色深度渗透。
- **修改文件**：`main.css`

#### 2. “拟物悬浮大圆角”现代化排版体系 (`main.css`)
- **新增** 全局彻底推翻原本的扁平直角设计，更迭为极度现代的悬浮质感架构。
- **设计细节**：各大功能板块与标签统一使用胶囊修角与 16px 无边线设计，施以浅绿弥散阴影 `0 4px 16px rgba(96, 153, 129, 0.08)`。并在外围 Body 底层铺设高级的高级拟物宣纸渐变（`radial-gradient` 网格噪点），实现页面空灵清新的顶级产品视觉表现。
- **修改文件**：`main.css`

#### 3. “一键防流失”沉浸导航胶囊 (`shop-list.html`、`shop-detail.html`、`blog-detail.html`)
- **新增** 于所有易流失用户的深层交互页面底部加入 “返回全局首页” 的全局悬浮导航球（FAB）。
- **设计细节**：巧妙置于防误触的安全黄金三角区域（`bottom: 80px`），采用 95% 白色磨砂感遮挡防止被滚动图层冲叠。点击反馈精准挂载 `location.replace`，清空深潜历史栈，根治了深层退出的心智痛点。
- **修改文件**：`shop-list.html`、`shop-detail.html`、`blog-detail.html`


---

## [v2.9.0] - 2026-04-02 | 前端大重构 - 订单列表与博客评论区动态化集成

### ✨ 新增功能

#### 1. 订单列表视觉与逻辑重组 (`order-list.html`)
- **新增** 基于后端 `VoucherOrderVO` 的订单卡片渲染结构，支持展示优惠券标题、副标题、现价与原价。
- **新增** 对接 `util.formatPrice` 的金额格式化展示，统一前端金额呈现口径。
- **新增** 订单卡片底部业务态展示与“去使用”交互按钮，替换原基础占位样式。

#### 2. 探店笔记多级评论区集成 (`blog-detail.html`)
- **新增** 基于真实评论数据的 `v-for` 动态渲染，替换静态占位评论。
- **新增** 作者标识高亮（作者徽章）与“回复 @xxx”溯源展示，提升楼中楼可读性。
- **新增** 底部 `el-dialog` 评论弹窗交互，打通前端发布评论到后端保存接口的闭环。

### 🐛 修复

#### 1. 订单列表接口响应解析修复 (`order-list.html`)
- **修复** Axios 回调中响应对象读取错误（`res` 误用）导致的数据解析风险。
- **修复后** 统一按 `({data})` 读取并处理 `data.success` / `data.data`，避免“有数据却空列表”或异常空态显示问题。

---

## [v1.5.1] - 2026-04-02 | 订单详情展示增强与评论区交互完善

### ✨ 新增功能

#### 1. 我的订单卡片信息增强 (`order-list.html`)
- **新增** 订单卡片中的优惠券标题、副标题、支付金额与原价金额展示，替代仅展示 `voucherId` 的基础形态。
- **新增** 订单底部“秒杀抢购成功”状态提示与“去使用”动作按钮，提升订单页业务信息密度。

#### 2. 博客详情评论区动态化 (`blog-detail.html`)
- **新增** 评论列表接口渲染逻辑，支持按真实评论数据展示头像、昵称、评论内容与时间。
- **新增** 评论弹窗发布能力，支持直接评论与对指定评论进行回复（携带 `parentId` / `answerId`）。
- **新增** 评论为空时的空状态提示，避免静态占位内容误导用户。

### 🐛 修复

#### 1. 订单接口响应读取错误修复 (`order-list.html`)
- **修复** Axios 响应解构使用错误导致的列表解析异常风险（`res` 误用）。
- **修复后** 统一按 `({data})` 读取并处理 `data.success` 与 `data.data`，保障与现有后端返回结构一致。

---

## [v1.5.0] - 2026-04-01 | 订单链路打通与交互体验优化

### ✨ 新增功能

#### 1. 我的订单页面 (`order-list.html`)
- **新增** 独立的“我的订单”页面 `order-list.html`，对接后端 `GET /voucher-order/list`。
- **实现** 订单卡片展示：订单号、订单状态、优惠券 ID、下单时间、支付方式。
- **实现** 空状态兜底：无订单时展示“您还未下过单~”。
- **实现** 状态与支付方式映射翻译（数字状态 → 中文状态），提升可读性。

#### 2. 秒杀券发布页面 (`add-voucher.html`)
- **新增** `add-voucher.html` 发布页，包含完整的 Vue 表单校验与提交逻辑。
- **实现** 金额“元 → 分”自动转换（`payValue` / `actualValue`），并固定 `type=1`。
- **实现** 生效/失效时间提交前格式化为 `yyyy-MM-ddTHH:mm:ss`。

#### 3. 个人页入口打通 (`info.html`)
- **新增** “全部订单”入口，点击跳转 `/order-list.html`，完成 C 端订单查询链路闭环。
- **新增** “【内部通道】模拟商家控制台”入口，点击跳转 `/add-voucher.html`，用于演示环境快速发券联调。

### 🐛 修复

#### 1. 日期组件二次选择报错修复 (`add-voucher.html`)
- **修复** Element UI 日期时间选择器在二次选择时的类型冲突问题（`getHours is not a function`）。
- **修复方案**：移除 `value-format` 让组件内部维持 `Date` 类型；提交前手动格式化为后端所需字符串。

#### 2. 店铺列表触底加载失效修复 (`shop-list.html`, `css/shop-list.css`)
- **修复** PC 端联调时滚动容器高度不稳定导致触底逻辑偶发失效的问题。
- **修复** 连续触底重复请求、空页继续请求的问题，新增 `isLoading` 与 `noMore` 防重与终止逻辑。
- **修复** 排序后分页状态污染问题，排序时重置 `current`、`shops`、`noMore`。

#### 3. 秒杀登录态判断缺失修复 (`shop-detail.html`)
- **修复** `seckill(v)` 中 `token` 未定义导致的登录态判断异常，补全 `sessionStorage` 取值。

### 🔧 重构

#### 1. 个人资料页可编辑化改造 (`info-edit.html`, `info.html`)
- **重构** 资料编辑页为可直接输入与选择的表单形态（昵称、简介、性别、城市、生日）。
- **新增** 固定底部“保存”按钮，统一触发资料保存流程。
- **实现** 并行更新用户基础信息与详情信息（`/user/me` + `/user/info`），成功后回退并刷新本地缓存。
- **优化** 个人主页信息展示：补全简介、城市、性别、生日展示字段。

#### 2. 可点击手型统一优化（多页面 CSS）
- **优化** 全局轻量兜底：`a, button, [role="button"]` 统一 `cursor: pointer`。
- **优化** 高频交互区域手型反馈（`index.css`、`info.css`、`shop-list.css`、`shop-detail.css`），修复 PC 端“可点击元素显示工字光标”的体验问题。

---

## [v1.4.0] - 2026-03-28 | 登录流程优化与密码设置功能

### ✨ 新增功能

#### 1. 设置密码功能 (`info.html`)
- **新增** 页面右上角"设置密码"按钮（蓝色），绑定 `@click="setPassword"` 事件。
- **`setPassword()` 方法实现**：
  - 优先从 `this.user.phone` 获取手机号，兜底读取 `sessionStorage.getItem("loginPhone")`。
  - 弹出 Element UI `$prompt` 对话框，输入类型为密码，支持 6 位以上校验。
  - 调用后端 `POST /user/password?phone=xxx&newPassword=xxx` 接口完成密码设置。
  - 若未获取到手机号，先弹出对话框让用户手动输入。

#### 2. 独立密码登录页面 (`login-password.html`)
- **新增** 独立的密码登录页面 `login-password.html`，支持手机号 + 密码登录。
- 手机号登录页 (`login.html`) 底部链接更新指向 `login-password.html`。

### 🔧 重构

#### Token 动态读取 (`js/common.js`)
- **修复**：将 Axios 请求拦截器中的 Token 获取方式从全局变量改为动态读取。
- **原因**：原写法在页面加载时一次性读取 `sessionStorage.getItem("token")`，若用户登录状态在页面存活期间变化，拦截器无法感知。
- **改动**：`const token = sessionStorage.getItem("token")` 移至拦截器内部，每次请求时实时读取。

#### 登录后手机号缓存 (`login.html`)
- **优化**：登录成功后，将用户输入的手机号缓存至 `sessionStorage.setItem("loginPhone", phone)`，供后续"设置密码"等功能使用。

#### 错误拦截器空值保护 (`js/common.js`)
- **修复**：在 401 未授权跳转逻辑前增加 `error.response` 空值检查，防止后端响应超时导致 JS 报错。

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
