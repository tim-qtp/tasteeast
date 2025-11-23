# Collection Configuration Guide - 配置父类自动筛选子类产品

## 问题描述
点击父类（如 Wine）时，希望显示所有子类的产品（red-wine, white-wine, beer 等）

## 解决方案

### 方法：在Shopify后台配置Automated Collection

---

## 步骤1：配置Wine Collection

### 1.1 进入Shopify Admin
```
Products > Collections > 找到 "Wine" collection
```

### 1.2 编辑Collection设置

**Collection type:** Automated

**Conditions:** 设置为以下任意一种方案

#### 方案A：使用 "contains" 匹配（推荐）
```
Product tag contains "wine"
```

**效果：** 所有包含 "wine" 的标签都会被匹配
- ✅ red-wine → 匹配
- ✅ white-wine → 匹配
- ✅ wine → 匹配
- ❌ beer → 不匹配（需单独添加）

#### 方案B：使用多条件 "OR" 匹配
```
Product tag is equal to wine
OR Product tag is equal to red-wine
OR Product tag is equal to white-wine
OR Product tag is equal to beer
```

**效果：** 精确匹配指定的标签

---

## 步骤2：配置其他父类Collection

### Beverage Collection
```
Automated Conditions:
Product tag contains "beverage"
OR Product tag is equal to tea
OR Product tag is equal to coffee
OR Product tag is equal to juice
```

### Chinese Cookie Collection
```
Automated Conditions:
Product tag contains "cookie"
OR Product tag is equal to chinese-cookie
OR Product tag is equal to traditional-cookie
OR Product tag is equal to modern-cookie
OR Product tag is equal to cookie-gift-box
```

### Gift Collection
```
Automated Conditions:
Product tag contains "gift"
OR Product tag is equal to gift-sets
OR Product tag is equal to gift-baskets
OR Product tag is equal to custom-gifts
```

---

## 步骤3：确保产品标签正确

### 标签策略（重要！）

每个产品必须同时打上**子类标签**和**父类标签**：

#### 示例1：红酒产品
```
正确标签：red-wine, wine, beverage
```
- `red-wine` → 出现在 Red Wine collection
- `wine` → 出现在 Wine collection
- `beverage` → 出现在 Beverage collection

#### 示例2：茶产品
```
正确标签：tea, beverage, green-tea
```
- `tea` → 出现在 Tea collection
- `beverage` → 出现在 Beverage collection
- `green-tea` → 额外的筛选标签

#### 示例3：传统饼干产品
```
正确标签：traditional-cookie, chinese-cookie, mooncake
```
- `traditional-cookie` → 出现在 Traditional collection
- `chinese-cookie` → 出现在 Chinese Cookie collection
- `mooncake` → 额外的筛选标签

---

## 步骤4：验证配置

### 4.1 检查Collection产品数量

在Shopify Admin中：
```
Products > Collections > Wine
```

查看右上角产品数量，应该等于所有子类的总和。

**示例：**
- Red Wine: 10个产品
- White Wine: 8个产品
- Beer: 5个产品
- **Wine总计: 应该有 23个产品**

### 4.2 前台测试

1. 访问 `/collections/wine`
2. 确认显示所有wine相关产品
3. 使用左侧或顶部的Filter进行二次筛选

---

## 完整的产品标签配置清单

### Wine类产品
| 产品类型 | 标签配置 |
|---------|---------|
| 红酒 | `red-wine, wine, beverage` |
| 白酒 | `white-wine, wine, beverage` |
| 啤酒 | `beer, wine, beverage` |

### Beverage类产品
| 产品类型 | 标签配置 |
|---------|---------|
| 茶 | `tea, beverage` |
| 咖啡 | `coffee, beverage` |
| 果汁 | `juice, beverage` |

### Chinese Cookie类产品
| 产品类型 | 标签配置 |
|---------|---------|
| 传统饼干 | `traditional-cookie, chinese-cookie` |
| 现代饼干 | `modern-cookie, chinese-cookie` |
| 饼干礼盒 | `cookie-gift-box, chinese-cookie` |

### Gift类产品
| 产品类型 | 标签配置 |
|---------|---------|
| 礼品套装 | `gift-sets, gift` |
| 礼品篮 | `gift-baskets, gift` |
| 定制礼品 | `custom-gifts, gift` |

---

## 批量更新产品标签

### 方法1：CSV导出/导入

1. **导出现有产品**
   ```
   Products > Export products
   ```

2. **编辑CSV文件**
   
   在 `Tags` 列添加父类标签：
   ```csv
   Handle,Tags
   red-wine-1,"red-wine, wine, beverage"
   white-wine-1,"white-wine, wine, beverage"
   ```

3. **重新导入**
   ```
   Products > Import products
   选择 "Overwrite products with same handle"
   ```

### 方法2：手动批量编辑

1. 进入 `Products`
2. 勾选多个产品
3. 点击 `Bulk edit`
4. 选择 `Add tags`
5. 输入 `wine, beverage`
6. 保存

---

## 常见问题

### Q1: 为什么Wine Collection还是空的？
**A:** 检查产品是否有 `wine` 标签，或者Collection条件配置是否正确。

### Q2: Beer应该算Wine吗？
**A:** 根据业务需求决定。如果Beer应该在Wine分类下，给Beer产品打上 `beer, wine, beverage` 标签。如果不应该，只打 `beer, beverage` 标签。

### Q3: 如何让产品同时出现在多个父类？
**A:** 给产品打上所有相关的标签。例如：
```
红酒礼品套装：red-wine, wine, beverage, gift-sets, gift
```

### Q4: Collection条件太复杂了，有简化方法吗？
**A:** 使用 `contains` 条件最简单：
```
Product tag contains "wine"
```
但要确保产品标签规范，避免误匹配。

---

## 推荐的标签命名规范

### ✅ 好的做法
- 小写字母
- 使用连字符分隔
- 包含层级信息：`red-wine, wine`
- 保持一致性

### ❌ 避免的做法
- 大小写混用：`Red-Wine`
- 使用空格：`red wine`
- 不同层级标签不一致：`red-wine` 但父类用 `Wine-Collection`

---

## 测试清单

配置完成后，请测试：

- [ ] 点击 Wine → 显示所有红酒、白酒、啤酒
- [ ] 点击 Red Wine → 只显示红酒
- [ ] 点击 Beverage → 显示所有饮料（包括酒、茶、咖啡等）
- [ ] 点击 Tea → 只显示茶
- [ ] 使用Filter过滤功能正常
- [ ] 产品数量统计正确

---

**最后更新：** 2024-11-23

