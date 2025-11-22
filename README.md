# Shopify 主题本地开发指南

这是从 Shopify 商店 (vkwzu0-s1) 下载的主题源码。

## 📁 项目结构

```
tasteeast/
├── assets/          # 静态资源（CSS, JS, 图片, 图标）
├── config/          # 主题配置文件
├── layout/          # 页面布局模板
├── locales/         # 多语言翻译文件
├── sections/        # 页面区块（可在主题编辑器中配置）
├── snippets/        # 可复用的代码片段
└── templates/       # 页面模板
```

## 🚀 本地开发方法

### 方法 1: 使用 Shopify CLI 实时开发（推荐）

这是最佳的开发方式，可以实时预览修改效果。

```bash
# 启动开发服务器
shopify theme dev --store=vkwzu0-s1.myshopify.com
```

**会发生什么：**
1. 🌐 浏览器会自动打开授权页面
2. ✅ 使用您已登录的 Shopify 账号授权（**不需要邮箱验证**）
3. ⬆️  主题文件会上传到开发环境
4. 🔗 获得预览URL，例如：`https://vkwzu0-s1.myshopify.com?preview_theme_id=xxxxx`
5. 🔄 文件修改会自动同步并刷新浏览器

**优点：**
- ✓ 实时预览修改
- ✓ 自动同步文件
- ✓ 支持热重载
- ✓ 不影响线上主题

### 方法 2: 推送为新主题

如果您想在商店后台预览，可以将主题推送为未发布的主题。

```bash
# 推送为未发布主题
shopify theme push --unpublished --store=vkwzu0-s1.myshopify.com

# 或者推送并覆盖开发主题
shopify theme push --development --store=vkwzu0-s1.myshopify.com
```

然后在 Shopify 后台 → Themes 中查看和预览。

### 方法 3: 检查主题质量

```bash
# 检查主题是否符合 Shopify 标准
shopify theme check

# 查看主题信息
shopify theme info
```

## 🔧 开发工作流

### 日常开发

1. **启动开发服务器**
   ```bash
   shopify theme dev --store=vkwzu0-s1.myshopify.com
   ```

2. **编辑文件**
   - 修改 `.liquid`, `.css`, `.js` 文件
   - 保存后自动同步到开发环境

3. **在浏览器中预览**
   - 访问 CLI 提供的预览 URL
   - 修改会实时反映在浏览器中

### 拉取线上主题最新修改

如果团队成员在 Shopify 后台直接编辑了主题：

```bash
shopify theme pull --store=vkwzu0-s1.myshopify.com
```

### 推送到生产环境

```bash
# 推送并发布（谨慎使用！）
shopify theme push --store=vkwzu0-s1.myshopify.com --live
```

## 📝 常用命令

| 命令 | 说明 |
|------|------|
| `shopify theme dev` | 启动开发服务器 |
| `shopify theme push` | 推送主题到商店 |
| `shopify theme pull` | 从商店拉取主题 |
| `shopify theme check` | 检查主题质量 |
| `shopify theme list` | 列出商店中所有主题 |
| `shopify theme info` | 显示当前主题信息 |

## ⚠️ 注意事项

1. **不要直接推送到生产环境**
   - 使用 `--unpublished` 或 `--development` 标志
   - 在 Shopify 后台测试后再发布

2. **文件同步**
   - `shopify theme dev` 不会永久保存修改到商店
   - 需要使用 `shopify theme push` 推送修改

3. **版本控制**
   - 建议使用 Git 管理主题代码
   - 定期提交修改

## 🆘 问题排查

### CLI 授权失败
- 确保浏览器已登录 Shopify 后台
- 尝试运行 `shopify logout` 然后重新登录

### 文件同步失败
- 检查网络连接
- 确认商店URL正确：`vkwzu0-s1.myshopify.com`

### 主题检查失败
```bash
shopify theme check --list
```

## 📚 相关资源

- [Shopify CLI 文档](https://shopify.dev/docs/themes/tools/cli)
- [Liquid 模板语言](https://shopify.dev/docs/api/liquid)
- [主题架构](https://shopify.dev/docs/themes/architecture)

## 🎯 快速开始

```bash
# 1. 启动开发服务器
shopify theme dev --store=vkwzu0-s1.myshopify.com

# 2. 在浏览器中授权

# 3. 开始编辑文件，实时预览修改！
```

---

**主题信息**
- 商店: vkwzu0-s1.myshopify.com
- 原始主题ID: 149868118244
- 下载日期: 2025-11-22

