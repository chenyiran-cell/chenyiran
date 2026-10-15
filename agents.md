# 项目规则（Project Rules）

> 本项目（D:\my_web）下的所有开发工作必须遵守以下规则。

## 1. 命名规范

- 所有 **CSS 类名（class）** 必须以 `chen_` 开头。
- 所有 **HTML id 名** 必须以 `chen_` 开头。
- 所有 **JavaScript 变量名** 必须以 `chen_` 开头。

示例：

```css
.chen_card { ... }
.chen_box-primary { ... }
```

```html
<div id="chen_main-content"></div>
```

```javascript
const chen_counter = 0;
let chen_userName = "demo";
function chen_initPage() { ... }
```

## 2. 提示词记录规范

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

## 3. 页面导航规范

- 每新建一个 HTML 文件，必须在该文件顶部（`<body>` 开头处）加入**返回首页的超链接**，指向 `index.html`。
- 链接文字示例：`← 返回首页`，样式需与页面整体风格协调，且不影响页面主体内容。

示例：

```html
<body>
  <a href="index.html" class="chen_back-home">← 返回首页</a>
  <!-- 页面主体内容 -->
</body>
```
