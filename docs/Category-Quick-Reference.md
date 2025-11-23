# Category Navigation - Quick Reference

## 🚀 快速开始

### 创建新分类（3步）

```bash
1. 创建Collection (Products > Collections > Create)
   - Title: "Sparkling Wine"
   - Handle: "sparkling-wine"
   - Type: Automated
   - Condition: Product tag = "sparkling-wine"

2. 在Theme Editor添加Block (Pages > collections)
   - Add Block > Child Category
   - Title: "Sparkling Wine"
   - Collection Handle: "sparkling-wine"
   - Parent ID: "parent_wine"

3. 给产品打标签
   - Tags: "sparkling-wine, wine, beverage"
```

---

## 📋 需要创建的Collections清单

### ✅ 已存在（9个）
- [x] wine
- [x] red-wine
- [x] white-wine
- [x] beer
- [x] beverage
- [x] chinese-cookie
- [x] gift
- [x] best-seller
- [x] new-arrivals

### ⚠️ 需创建（9个）

**Beverage子分类：**
- [ ] tea
- [ ] coffee
- [ ] juice

**Chinese Cookie子分类：**
- [ ] traditional-cookie
- [ ] modern-cookie
- [ ] cookie-gift-box

**Gift子分类：**
- [ ] gift-sets
- [ ] gift-baskets
- [ ] custom-gifts

---

## 🏷️ 标签规则速查

| 产品类型 | 必需标签 | 推荐标签 |
|---------|---------|---------|
| 红酒 | `red-wine, wine, beverage` | `bordeaux, premium, france` |
| 白酒 | `white-wine, wine, beverage` | `chardonnay, italy` |
| 茶 | `tea, beverage` | `green-tea, black-tea, china` |
| 咖啡 | `coffee, beverage` | `arabica, colombia` |
| 传统饼干 | `traditional-cookie, chinese-cookie` | `mooncake, sesame` |
| 礼品套装 | `gift-sets, gift` | `premium, festive` |

---

## 🎨 Hover颜色配置

```css
/* 主题绿色 */
--hover-color: #0a7084

/* 使用位置 */
✓ 分类菜单hover
✓ Featured卡片标题hover
✓ 导航栏hover
```

---

## 📐 布局尺寸速查

### 桌面端 (>990px)
```
总宽度: 1400px (居中)
左侧菜单: 320px (两列网格)
右侧内容: 自动填充
间隔: 60px
```

### 平板端 (750px-990px)
```
左侧菜单: 260px (单列)
间隔: 40px
```

### 移动端 (<750px)
```
单列布局
菜单两列网格
间隔: 40px
```

---

## 🔧 常用Git命令

```bash
# 提交更改
git add .
git commit -m "Update category configuration"
git push

# 查看状态
git status

# 同步远程
git pull
```

---

## 🐛 快速故障排除

| 问题 | 快速解决 |
|-----|---------|
| 点击404 | 检查collection handle是否正确 |
| 无产品 | 检查产品标签和collection条件 |
| 菜单不显示 | 检查Theme Editor配置 |
| 样式错乱 | 清除浏览器缓存 (Cmd+Shift+R) |
| 子分类错位 | 检查Parent ID是否正确 |

---

## 📱 访问链接

```
分类导航页: /pages/collections
所有产品: /collections/all
红酒分类: /collections/red-wine
茶分类: /collections/tea
```

---

## ⚙️ Collection配置模板

### Automated Collection (推荐)

```json
{
  "title": "Red Wine",
  "handle": "red-wine",
  "collection_type": "automated",
  "conditions": [
    {
      "column": "tag",
      "relation": "equals",
      "condition": "red-wine"
    }
  ]
}
```

### Manual Collection

```json
{
  "title": "Featured Products",
  "handle": "featured",
  "collection_type": "manual"
}
```

---

## 📦 产品CSV导入模板

```csv
Handle,Title,Tags,Published
red-wine-1,"Château Margaux 2015","red-wine, wine, beverage, bordeaux",TRUE
tea-green-1,"Longjing Green Tea","tea, beverage, green-tea, china",TRUE
```

**注意：** 
- 多个标签用逗号分隔
- 如有空格，整个Tags用引号包裹
- Handle必须唯一

---

## 🎯 测试清单

```bash
✓ 访问 /pages/collections
✓ 点击每个父分类，验证跳转
✓ 点击每个子分类，验证跳转
✓ 验证产品正确显示
✓ 测试过滤功能
✓ 测试排序功能
✓ 测试移动端布局
✓ 测试hover效果
```

---

## 🔑 关键文件

```
sections/collections-page.liquid       # 分类页面
assets/section-collections-page.css    # 样式
templates/page.collections.json        # 页面配置
templates/collection.json              # 集合模板
```

---

## 💡 专业提示

1. **标签命名**：始终使用小写+连字符（`red-wine`）
2. **父Collection**：使用宽泛条件包含所有子产品
3. **子Collection**：使用精确条件（`is equal to`）
4. **多层标签**：每个产品至少3个标签（子-父-祖）
5. **测试先行**：创建collection后立即测试

---

## 📞 紧急问题检查

```bash
问题：所有分类都404
→ 检查collection.json中product-grid是否启用

问题：标签不匹配
→ 对比产品标签和collection条件是否一致

问题：样式丢失
→ 检查section-collections-page.css是否存在

问题：配置丢失
→ 检查page.collections.json是否被覆盖
```

---

**版本：** 1.0.0  
**更新：** 2024-11-23

