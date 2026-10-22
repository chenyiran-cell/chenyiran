# 项目规则（Project Rules）

> 本项目（D:\my_web）下的所有开发工作必须遵守以下规则。

## 1. 命名规范

所有 **CSS 类名（class）**、**HTML id 名**、**JavaScript 变量名/函数名** 必须以 `cyr_` 开头。

示例：

```css
.cyr_card { ... }
.cyr_box-primary { ... }
```

```html
<div id="cyr_main-content"></div>
```

```javascript
const cyr_counter = 0;
let cyr_userName = "demo";
function cyr_initPage() { ... }
```

## 2. HTML 页面注释规范

每一个 HTML 页面（无论新建还是修改）都必须在 `<body>` 标签的**开头位置**包含以下格式的注释：

```html
<!-- 姓名：陈依然，日期：功能名称 -->
```

- **姓名** 固定为：`陈依然`
- **日期** 为页面功能或修改日期，格式 `yyyy-mm-dd`
- **功能名称** 为该页面的功能简述（如：首页、CSS 变量演示、14-1 示例页面）

示例：

```html
<body>
  <!-- 姓名：陈依然，日期：2026-10-22，功能：CSS 变量演示 -->
  <a href="index.html" class="cyr_back-home">← 返回首页</a>
  <!-- 页面主体内容 -->
</body>
```

## 3. 提示词记录规范

- 每次执行任务前，必须先将**用户的提示词**追加写入项目根目录下的 `chen_prompt.md` 文件。
- **必须以追加（append）方式写入，严禁覆盖已有内容。**
- 每条记录须包含以下三项信息：
  1. **时间**：格式 `yyyy-mm-dd-hh-mm`
  2. **使用的模型**
  3. **提示词内容**（用户原始请求）

记录格式示例：

```markdown
## 2026-10-12-15-56

- 模型：<模型名称>
- 提示词：<用户原始提示词内容>

---
```

## 4. 页面导航规范

- 每新建一个 HTML 文件，必须在该文件顶部（`<body>` 开头处）加入**返回首页的超链接**，指向 `index.html`。
- 链接文字示例：`← 返回首页`，样式需与页面整体风格协调，且不影响页面主体内容。
- 该导航链接须放在 HTML 页面注释之后（见规则 2）。

示例：

```html
<body>
  <!-- 姓名：陈依然，日期：2026-10-22，功能：XXX 页面 -->
  <a href="index.html" class="cyr_back-home">← 返回首页</a>
  <!-- 页面主体内容 -->
</body>
```

## 5. 技术栈规范

- **仅使用原生** HTML、CSS 和 JavaScript（ES6+），禁止使用任何第三方框架（如 Vue、React、jQuery、Bootstrap 等）。
- **禁止使用任何 CDN**（包括但不限于 CDN.js、unpkg、jsDelivr、cdnjs 等），所有资源必须本地化。
- **图标**统一使用 `font/` 目录下的 iconfont 图标资源，通过引入 `font/iconfont.css` 使用。禁止引入 Font Awesome、Lucide、Material Icons 等第三方图标库。
- **服务端数据获取**统一使用原生 `fetch()` API，禁止使用 axios、XMLHttpRequest 等其他库或方式。

iconfont 使用示例：

```html
<link rel="stylesheet" href="../font/iconfont.css">

<!-- 使用 iconfont 图标 -->
<i class="iconfont icon-home"></i>
```

fetch 使用示例：

```javascript
fetch('/api/data')
  .then(response => response.json())
  .then(data => {
    cyr_data = data;
  })
  .catch(error => console.error(error));
```
