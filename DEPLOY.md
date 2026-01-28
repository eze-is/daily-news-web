# Daily News Web 部署指南

## GitHub 仓库

- **URL**: https://github.com/eze-is/daily-news-web
- **状态**: 已推送，包含构建脚本和初始内容

## Cloudflare Pages 配置步骤

### 1. 登录 Cloudflare Dashboard

访问 https://dash.cloudflare.com 并登录

### 2. 创建 Pages 项目

1. 点击左侧菜单 **"Pages"**
2. 点击 **"Create a project"**
3. 选择 **"Connect to Git"**

### 3. 连接 GitHub 仓库

1. 授权 Cloudflare 访问你的 GitHub 账户
2. 选择仓库: `eze-is/daily-news-web`
3. 点击 **"Begin setup"**

### 4. 构建设置

填写以下配置:

| 设置项 | 值 |
|--------|-----|
| **Project name** | `daily-news` (或你喜欢的名称) |
| **Production branch** | `main` |
| **Build command** | `python3 build.py` |
| **Build output directory** | `dist` |

### 5. 环境变量（可选）

如有需要，可添加环境变量。

### 6. 保存并部署

点击 **"Save and Deploy"**

Cloudflare 会自动:
- 克隆仓库
- 运行 `python3 build.py`
- 部署 `dist/` 目录内容

### 7. 自定义域名（可选）

1. 在 Pages 项目设置中，点击 **"Custom domains"**
2. 点击 **"Set up a custom domain"**
3. 输入你的域名（如 `news.yourdomain.com`）
4. 按照提示添加 DNS 记录

### 8. 自动部署

每次推送代码到 `main` 分支，Cloudflare 会自动重新构建和部署。

## 本地预览

```bash
cd ~/Documents/1-Projects/每日资讯日报/website
python3 build.py
cd dist
python3 -m http.server 8000
```

访问 http://localhost:8000 预览

## 添加新日报

当 `daily-news` skill 生成新的日报后:

1. 新日报会自动保存到 `output/YYYY-MM-DD.md`
2. 运行构建脚本更新网站:
   ```bash
   cd ~/Documents/1-Projects/每日资讯日报/website
   python3 build.py
   git add -A
   git commit -m "Add daily report for YYYY-MM-DD"
   git push origin main
   ```
3. Cloudflare 会自动部署更新

## 网站结构

```
website/
├── build.py          # 构建脚本
├── dist/             # 生成的静态网站
│   ├── index.html    # 首页（最新日报）
│   └── YYYY-MM-DD.html
├── src/              # 源代码（可选）
└── wrangler.toml     # Cloudflare 配置
```

## 故障排除

**构建失败？**
- 检查 `build.py` 是否有语法错误
- 确认 `output/` 目录有 `.md` 文件

**页面 404？**
- 确认 Build output directory 设置为 `dist`
- 检查 `dist/index.html` 是否存在

**样式不生效？**
- 清除浏览器缓存
- 检查浏览器开发者工具中的网络请求
