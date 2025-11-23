# Category Navigation Implementation Guide

## 📋 目录
- [功能概述](#功能概述)
- [技术架构](#技术架构)
- [实现的功能](#实现的功能)
- [配置指南](#配置指南)
- [使用方法](#使用方法)
- [故障排除](#故障排除)

---

## 功能概述

Category Navigation 是一个完整的产品分类浏览系统，允许用户通过层级化的分类菜单快速浏览和筛选产品。

### 核心特性
- ✅ 支持父子分类层级结构
- ✅ 点击分类自动跳转到对应的Collection页面
- ✅ 内置产品过滤和排序功能
- ✅ 基于Shopify标签的自动化产品分类
- ✅ 响应式设计，支持桌面端和移动端
- ✅ Hover交互效果

---

## 技术架构

### 文件结构

```
/sections/
  └── collections-page.liquid       # 分类页面主模板
/assets/
  └── section-collections-page.css  # 分类页面样式
/templates/
  ├── page.collections.json         # /pages/collections 页面配置
  └── collection.json               # /collections/* 集合页面配置
```

### 数据流

```
用户访问 /pages/collections
    ↓
显示分类菜单（collections-page.liquid）
    ↓
用户点击分类（如 "Red Wine"）
    ↓
跳转到 /collections/red-wine
    ↓
Shopify 使用 collection.json 模板渲染
    ↓
显示该分类下的所有产品
```

---

## 实现的功能

### 1. 分类菜单结构

#### 父分类（Parent Category）
- 可以有子分类
- 可以配置collection handle，使其可点击
- 如果不配置handle，仅作为分组标题

#### 子分类（Child Category）
- 必须关联到父分类
- 必须配置collection handle
- 点击跳转到对应的collection页面

#### 独立分类（Single Category）
- 不属于任何父分类
- 直接显示为一级菜单项

### 2. 分类层级示例

```
Wine (父分类，可点击)
├── Red Wine (子分类)
├── White Wine (子分类)
└── Beer (子分类)

Beverage (父分类，可点击)
├── Tea (子分类)
├── Coffee (子分类)
└── Juice (子分类)

Chinese Cookie (父分类，可点击)
├── Traditional (子分类)
├── Modern (子分类)
└── Gift Box (子分类)

Gift (父分类，可点击)
├── Gift Sets (子分类)
├── Gift Baskets (子分类)
└── Custom Gifts (子分类)
```

### 3. Collection页面功能

当用户点击任何分类后，会跳转到Shopify的Collection页面，该页面包含：

#### 启用的功能
- ✅ Collection Banner（集合横幅）
- ✅ Product Grid（产品网格，24个/页）
- ✅ Filtering（按标签、价格、可用性等过滤）
- ✅ Sorting（按价格、名称、最新等排序）
- ✅ Pagination（分页）

#### 禁用的装饰性Section
为了保持Collection页面简洁，以下section默认禁用：
- ❌ Hero Product Banner
- ❌ USP Tabbed Grid
- ❌ Split Hero
- ❌ Best Spirits Grid
- ❌ Limited Stock Banners
- ❌ Logistics Icons Row
- ❌ Taste Subscribe Block

### 4. 交互效果

#### Hover效果（色号：#0a7084）
- 父分类标题hover时变绿色
- 子分类链接hover时变绿色
- Featured Collection卡片hover时：
  - 阴影加深
  - 向上移动4px
  - 图片放大1.08倍
  - 标题文字变绿色

#### 布局适配
- **桌面端（>990px）**：
  - 左侧分类菜单：320px宽，两列网格
  - 右侧Featured Cards：两列
  - 总宽度：1400px（居中）

- **平板端（750px-990px）**：
  - 左侧分类菜单：260px宽，单列
  - 右侧Featured Cards：两列

- **移动端（<750px）**：
  - 分类菜单置顶，两列网格
  - Featured Cards：单列

---

## 配置指南

### 步骤1：在Shopify创建Collections

#### 1.1 进入Shopify Admin
```
Products > Collections > Create collection
```

#### 1.2 创建必需的Collections

**已存在的Collections（无需创建）：**
- wine
- red-wine
- white-wine
- beer
- beverage
- chinese-cookie
- gift
- best-seller
- new-arrivals

**需要创建的Collections（9个）：**

| Collection Title | Handle | Parent | Type |
|-----------------|---------|---------|------|
| Tea | tea | Beverage | Automated |
| Coffee | coffee | Beverage | Automated |
| Juice | juice | Beverage | Automated |
| Traditional | traditional-cookie | Chinese Cookie | Automated |
| Modern | modern-cookie | Chinese Cookie | Automated |
| Gift Box | cookie-gift-box | Chinese Cookie | Automated |
| Gift Sets | gift-sets | Gift | Automated |
| Gift Baskets | gift-baskets | Gift | Automated |
| Custom Gifts | custom-gifts | Gift | Automated |

#### 1.3 配置Collection为Automated（推荐）

**以 "Tea" Collection为例：**

1. **Collection type**: Automated
2. **Conditions**: 
   ```
   Product tag is equal to tea
   ```
3. **保存**

这样，所有带 `tea` 标签的产品会自动添加到此collection。

#### 1.4 配置父Collection包含所有子产品

**以 "Beverage" Collection为例：**

1. **Collection type**: Automated
2. **Conditions**:
   ```
   Product tag contains beverage
   OR
   Product tag is equal to tea
   OR
   Product tag is equal to coffee
   OR
   Product tag is equal to juice
   ```
3. **保存**

### 步骤2：配置产品标签

**重要：标签必须与Collection Handle完全一致！**

#### 2.1 单个产品添加标签

1. 进入产品编辑页面
2. 找到 "Tags" 字段
3. 输入标签，多个标签用**逗号**分隔：
   ```
   red-wine, wine, beverage
   ```

#### 2.2 批量导入标签（通过CSV）

在产品CSV中，`Tags` 列格式：
```csv
Handle,Tags
chateau-margaux-2015,"red-wine, wine, beverage"
longjing-green-tea,"tea, beverage"
```

**注意：** 标签之间用逗号和空格分隔，整体用引号包裹（如果有多个标签）。

### 步骤3：在Theme Editor中验证配置

#### 3.1 检查 Collections Page配置

1. 进入 **Online Store > Themes > Customize**
2. 导航到 **Pages > collections**
3. 检查左侧Section设置：
   - 确认所有Parent Category和Child Category都已配置
   - 确认每个category的 `collection_handle` 正确填写

#### 3.2 配置示例

**Parent Category: Wine**
- Title: `Wine`
- Collection Handle: `wine`

**Child Category: Red Wine**
- Parent Category ID: `parent_wine` （从Parent Category的Block ID复制）
- Title: `Red Wine`
- Collection Handle: `red-wine`

### 步骤4：同步GitHub主题（如果使用GitHub集成）

```bash
cd /path/to/your/theme
git pull
```

或在Shopify Admin中手动点击 "Sync from GitHub"。

---

## 使用方法

### 前台用户视角

#### 1. 访问分类页面
```
https://your-store.myshopify.com/pages/collections
```

#### 2. 浏览分类菜单
- 左侧显示所有产品分类
- 父分类以粗体显示
- 子分类缩进显示在父分类下方

#### 3. 点击分类
- 点击任何分类（父或子）
- 页面跳转到对应的collection页面
- 显示该分类下的所有产品

#### 4. 使用过滤和排序
- 使用页面顶部的过滤器按标签、价格等筛选
- 使用排序下拉菜单改变产品顺序
- 翻页查看更多产品

### 后台管理员视角

#### 添加新分类

**场景：添加 "Sparkling Wine" 到 Wine 分类下**

1. **创建Collection：**
   - 进入 Products > Collections > Create collection
   - Title: `Sparkling Wine`
   - Handle: `sparkling-wine`（自动生成）
   - Type: Automated
   - Conditions: `Product tag is equal to sparkling-wine`
   - 保存

2. **在Theme Editor中添加Block：**
   - 进入 Theme Customizer
   - 导航到 Pages > collections
   - 找到 "Collections Page" section
   - 点击 "Add block" > "Child Category"
   - 配置：
     - Parent Category ID: `parent_wine`
     - Title: `Sparkling Wine`
     - Collection Handle: `sparkling-wine`
   - 保存

3. **给产品打标签：**
   - 编辑产品
   - Tags: `sparkling-wine, wine, beverage`
   - 保存

4. **测试：**
   - 访问 /pages/collections
   - 验证 "Sparkling Wine" 显示在 Wine 下
   - 点击验证跳转正常
   - 验证产品正确显示

#### 修改分类顺序

在Theme Editor中，直接拖拽blocks即可改变顺序。

**注意：** Child Category必须在对应的Parent Category之后。

#### 修改分类名称

1. **前台显示名称：**
   - 在Theme Editor中修改block的 "Title" 字段

2. **Collection名称：**
   - 在Products > Collections中编辑collection的Title

**建议：** 保持两者一致，避免混淆。

---

## 故障排除

### 问题1：点击分类后显示404

**原因：** Collection不存在或Handle不匹配

**解决方法：**
1. 检查Theme Editor中配置的 `collection_handle` 
2. 在Products > Collections中验证该collection存在
3. 确认collection的handle与配置完全一致（大小写敏感）

### 问题2：Collection页面是空的

**原因：** Collection中没有产品

**解决方法：**
1. 检查collection配置（Manual或Automated）
2. 如果是Automated，检查条件是否正确
3. 检查产品是否有对应的标签
4. 手动将产品添加到collection（如果是Manual类型）

### 问题3：产品显示在错误的分类中

**原因：** 产品标签配置错误或Collection条件太宽泛

**解决方法：**
1. 检查产品标签是否正确
2. 检查Collection的Automated条件
3. 如果使用 "contains" 条件，改为 "is equal to" 更精确

### 问题4：分类菜单不显示

**原因：** Theme Editor配置缺失或页面模板错误

**解决方法：**
1. 在Theme Editor中检查 Pages > collections 是否使用了正确的模板
2. 确认 `page.collections.json` 引用了 `collections-page` section
3. 检查是否有blocks配置

### 问题5：子分类没有正确嵌套

**原因：** Parent ID配置错误

**解决方法：**
1. 找到父分类的Block ID（在Theme Editor中选中block时，URL中可以看到）
2. 将这个ID复制到子分类的 "Parent Category ID" 字段
3. 确保子分类block在父分类block之后

### 问题6：Featured Collection卡片不显示

**原因：** 图片未上传或链接配置错误

**解决方法：**
1. 在Theme Editor中检查 Featured Collection Card block
2. 上传图片
3. 配置正确的链接（通常是 `/collections/collection-handle`）

### 问题7：样式不正确或布局错乱

**原因：** CSS文件缺失或缓存问题

**解决方法：**
1. 确认 `assets/section-collections-page.css` 存在
2. 在浏览器中清除缓存（Ctrl+Shift+R 或 Cmd+Shift+R）
3. 检查浏览器控制台是否有CSS加载错误

### 问题8：过滤和排序功能不可用

**原因：** Product Grid section被禁用

**解决方法：**
1. 编辑 `templates/collection.json`
2. 找到 `"product-grid"` section
3. 确保 `"disabled"` 字段不存在或为 `false`
4. 确认以下设置：
   ```json
   "enable_filtering": true,
   "enable_sorting": true
   ```

---

## 最佳实践

### 1. 标签命名规范

- ✅ 使用小写字母
- ✅ 多个单词用连字符连接（如 `red-wine`）
- ✅ 保持标签与collection handle一致
- ❌ 避免使用空格
- ❌ 避免使用特殊字符

### 2. Collection配置建议

- **首选Automated类型**：减少手动维护工作
- **使用精确条件**：`is equal to` 优于 `contains`
- **父Collection使用宽泛条件**：包含所有子分类的产品
- **子Collection使用精确条件**：只包含特定子类产品

### 3. 产品标签策略

**为每个产品添加多层标签：**

```
Example: Château Margaux 2015
Tags: red-wine, wine, beverage, bordeaux, premium, france
```

这样产品可以出现在：
- Red Wine collection（red-wine）
- Wine collection（wine）
- Beverage collection（beverage）
- 还可以通过其他标签进行过滤（bordeaux, premium, france）

### 4. 性能优化

- 限制每个collection的产品数量（建议<500）
- 使用产品图片压缩
- 配置合理的 `products_per_page`（建议24-48）
- 启用Shopify CDN缓存

### 5. 测试清单

在上线前，请测试：
- [ ] 所有分类链接是否正常工作
- [ ] 每个collection是否显示正确的产品
- [ ] 过滤和排序功能是否正常
- [ ] 移动端布局是否正常
- [ ] Hover效果是否正确
- [ ] 图片加载是否正常
- [ ] 分页功能是否正常

---

## 技术参考

### Collection URL结构

```
/pages/collections          → 分类导航页面（collections-page.liquid）
/collections/wine           → Wine collection页面（collection.json）
/collections/red-wine       → Red Wine collection页面（collection.json）
/collections/all            → 所有产品
```

### Liquid变量

```liquid
{{ collection.title }}           # Collection标题
{{ collection.handle }}          # Collection handle
{{ collection.description }}     # Collection描述
{{ collection.products_count }}  # 产品数量
{{ collection.url }}             # Collection URL
```

### CSS类名参考

```css
.collections-page                    # 页面容器
.collections-page__container         # 主网格容器
.collections-page__sidebar           # 左侧分类菜单
.collections-nav__list               # 分类列表
.collections-nav__item--parent       # 父分类项
.collections-nav__item--child        # 子分类项
.collections-nav__parent-title       # 父分类标题
.collections-nav__link               # 分类链接
.featured-collections                # Featured卡片容器
.featured-collection__card           # Featured卡片
```

### Shopify Settings Schema

```json
{
  "type": "parent_category",
  "name": "Parent Category",
  "settings": [
    {
      "type": "text",
      "id": "title",
      "label": "Category Title"
    },
    {
      "type": "text",
      "id": "collection_handle",
      "label": "Collection Handle"
    }
  ]
}
```

---

## 版本历史

### v1.0.0 (2024-11-23)
- ✅ 初始实现
- ✅ 支持父子分类层级
- ✅ 启用collection产品网格
- ✅ 响应式设计
- ✅ Hover交互效果

---

## 支持与联系

如有问题，请检查：
1. 本文档的"故障排除"部分
2. Shopify官方文档：https://help.shopify.com/
3. Liquid文档：https://shopify.dev/docs/themes/liquid

---

**文档版本：** 1.0.0  
**最后更新：** 2024-11-23  
**维护者：** TasteEast Development Team

