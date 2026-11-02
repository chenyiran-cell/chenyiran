---
name: auto-add-nav
description: 新建或修改 HTML 页面后，自动将该页面加入 pages/index.html 导航。Use when 新建页面、加导航、更新首页链接、把 X 加入导航、导航更新。
---

# 自动加入首页导航

每完成或新建一个 HTML 页面后，自动在 `pages/index.html` 对应章节追加导航超链接。

## 触发条件

以下情况**必须**执行本 Skill，不可遗漏：
1. 新建了一个 `.html` 页面并完成内容
2. 用户明确要求"把 XX 加入导航"、"更新首页"、"加导航链接"
3. 修改了已有页面需要同步导航（如重命名、补漏）

## 目标文件

- **导航文件**：`d:\my_web\pages\index.html`（唯一权威导航入口）
- **页面目录**：`d:\my_web\pages\` 下的所有 `.html` 文件

## 导航结构约定

`pages/index.html` 的导航结构固定如下：

```html
<div class="chapter">
    <h2>第X章（标题可选）</h2>
    <div class="links">
        <a href="文件名.html">显示名</a>
        <!-- 新链接追加在末尾 -->
    </div>
</div>
```

## 执行步骤

### 1. 读取导航文件

```
Read d:\my_web\pages\index.html
```

### 2. 判断目标章节

根据**文件名前缀**自动匹配章节：

| 文件名模式 | 目标章节 h2 |
|-----------|------------|
| `2-*.html` | `第二章` |
| `3-*.html` | `第三章` |
| `4-*.html` | `第四章` |
| `5-*.html` | `第五章` |
| ...以此类推 | ... |
| `16-*.html` | `第十六章（JavaScript 进阶）` |
| `17-*.html` | `第十七章（JSON & fetch & API）` |
| `四级项目.html` 等无数字前缀的独立项目 | 在最后一个 chapter **之后**、`</div>` 闭合标签**之前**，**新增**一个 chapter 块（如 `<h2>独立项目</h2>`） |

### 3. 生成链接 HTML

#### 3a. href 处理

- **纯英文/数字文件名**：直接写入，如 `href="17-6.html"`
- **含中文字符的文件名**：对 `.html` 之前的中文部分做 **URL 编码**（`encodeURIComponent`），但保留 `.html` 后缀不编码。示例：
  - `四级项目.html` → `href="%E5%9B%9B%E7%BA%A7%E9%A1%B9%E7%9B%AE.html"`
  - `5-1 层叠与继承.html` → `href="5-1%20%E5%B1%82%E5%8F%A0%E4%B8%8E%E7%BB%A7%E6%89%BF.html"`
- 注意：空格编码为 `%20`，中文每个字符各自编码。

#### 3b. 显示名（链接文本）

- 优先取**文件内容的 `<title>` 标签**或 `<h1>` 作为显示名
- 若无法读取或过长，则用**文件名去掉 `.html` 后缀**作为显示名
- 保持与同章节其他链接的命名风格一致（如 `17-6 Promise &amp; async 包饺子`）

### 4. 追加链接到导航

使用 **Edit 工具**，在目标章节的 `.links` div **末尾**（`</div>` 闭合标签之前）追加新的 `<a>` 标签：

```html
                <a href="URL编码后的文件名.html">显示名</a>
```

#### Edit 的 old_string 策略

找到目标章节 `.links` 的最后一个 `<a>` 标签行 + 紧接着的 `</div>` 行，作为 old_string，在二者之间插入新链接。

示例——第十七章末尾追加新链接时：

**old_string**（最后一个 a + 闭合 div）：
```
                <a href="17-6.html">17-6 Promise &amp; async 包饺子</a>
            </div>
```

**new_string**（插入新链接后）：
```
                <a href="17-6.html">17-6 Promise &amp; async 包饺子</a>
                <a href="新文件.html">新文件显示名</a>
            </div>
```

#### 独立项目（新章节）

当文件名不属于任何已有章节时，在 `</div><!-- wrap end -->` 或 `</div>\n</body>` **之前**新增一个完整 chapter：

```html
        <div class="chapter">
            <h2>独立项目</h2>
            <div class="links">
                <a href="URL编码.html">显示名</a>
            </div>
        </div>
```

### 5. 去重检查

追加前先扫描当前导航，确认 `href` 中**不存在**相同文件名。已存在则跳过，不重复添加。

### 6. 完成提示

导航更新完成后，向用户报告：
- 新增了哪条链接
- 归属哪个章节
- 如果是新章节，说明章节名称

## 与 github-auto-push Skill 的协作

本 Skill **只负责**修改 `pages/index.html`。修改完成后，正常进入 git 提交流程（调用 `github-auto-push` Skill 或手动执行），提交信息中应包含"导航更新"字样，例如：

```
git commit -m "新增 四级项目.html 并加入导航"
git commit -m "17-6 完成 + 导航更新"
```

## 反模式（禁止）

1. ❌ **覆盖式修改**：只改不追加，导致同章已有链接丢失
2. ❌ **href 不编码**：中文文件名直接写入，浏览器可能解析出错
3. ❌ **忘记闭合 tag**：append 导致 HTML 结构损坏
4. ❌ **同时改 origin remote**：本 Skill 只管 pages/index.html，不碰 git remote 配置
5. ❌ **跳过去重**：同文件被多次添加导致首页重复链接
