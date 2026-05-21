# 前端面试八股 · 学习目录

> 完整版：`2026-前端面试八股文-超全完整版.md`  
> 分章带案例版：见下方 `chapters/` 目录

## 怎么用这份资料

1. **先背要点**：每章开头的「八股要点」是面试直答版。
2. **再看案例**：每章末尾或小节里的「案例讲解」用代码帮你理解「为什么」。
3. **自己敲一遍**：把案例在浏览器控制台或 CodeSandbox 跑一遍，比只看文字快很多。
4. **对照场景题**：第 12 章把前面知识串成真实项目问题。

## 章节目录

| 章节 | 文件 | 重点案例 |
| --- | --- | --- |
| 一 | [01-JavaScript核心.md](./chapters/01-JavaScript核心.md) | 值/引用、闭包、this、原型、Promise、事件循环、防抖节流 |
| 二 | [02-ES6+.md](./chapters/02-ES6+.md) | 解构、Set/Map、class、模块化、可选链 |
| 三 | [03-浏览器原理.md](./chapters/03-浏览器原理.md) | URL 加载、回流重绘、缓存、跨域、存储 |
| 四 | [04-HTTP与网络.md](./chapters/04-HTTP与网络.md) | 状态码、HTTPS、预检请求 |
| 五 | [05-CSS.md](./chapters/05-CSS.md) | 盒子模型、BFC、Flex 居中、权重 |
| 六 | [06-Vue2.md](./chapters/06-Vue2.md) | 响应式、通信、key、computed/watch、nextTick |
| 七 | [07-Vue3.md](./chapters/07-Vue3.md) | Proxy、Composition API、Pinia |
| 八 | [08-工程化.md](./chapters/08-工程化.md) | Webpack/Vite 区别、打包优化 |
| 九 | [09-性能优化.md](./chapters/09-性能优化.md) | 首屏、虚拟列表、打包瘦身 |
| 十 | [10-前端安全.md](./chapters/10-前端安全.md) | XSS、CSRF 攻防示例 |
| 十一 | [11-手写代码.md](./chapters/11-手写代码.md) | 防抖、深拷贝、Promise、call 等完整实现 |
| 十二 | [12-场景面试题.md](./chapters/12-场景面试题.md) | 大表格、白屏、跨域、防重复点击等 |

## 学习顺序建议

- **初级**：01 → 02 → 05 → 04  
- **中级**：01 → 03 → 06 → 09 → 11  
- **高级**：03 → 07 → 08 → 09 → 12  

---

## 挂到 GitHub 并用网页打开

### 方式一：GitHub Pages（推荐，像网站一样浏览）

本目录已带好 **Docsify** 配置（`index.html` + `_sidebar.md`），推上去后可在浏览器里看侧边栏、搜索、分章阅读。

**步骤：**

1. 在 [GitHub 新建仓库](https://github.com/new)（例如 `frontend-bagu`，选 **Public**）。
2. 在本目录打开终端，执行（把地址换成你的仓库）：

```powershell
cd D:\lpy\八股
git init
git add .
git commit -m "Add frontend interview notes"
git branch -M main
git remote add origin https://github.com/你的用户名/frontend-bagu.git
git push -u origin main
```

3. 打开仓库 **Settings → Pages**：
   - **Source**：Deploy from a branch  
   - **Branch**：`main`，文件夹选 **`/ (root)`**  
   - 保存后等 1～3 分钟。

4. 访问：`https://你的用户名.github.io/frontend-bagu/`  
   （仓库名不同则改 URL 最后一段。）

本地可先预览（需已安装 Node）：

```powershell
npx serve .
# 浏览器打开 http://localhost:3000
```

### 方式二：只在 GitHub 上看 Markdown

不配置 Pages 也可以：把仓库推上去后，在网页里点开任意 `.md` 文件，GitHub 会自动渲染（无侧边栏，适合偶尔查看）。

### 方式三：私有仓库

私有仓同样可用 Pages（GitHub 免费账号支持私有仓 Pages）。若不想公开，建仓库时选 **Private**，Pages 地址仅你知道（可再配合仓库可见权限）。
