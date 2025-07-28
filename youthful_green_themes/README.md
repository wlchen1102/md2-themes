# 青春绿-公众号主题样式

这是一个为md2card.com平台设计的青春绿风格公众号主题。

## 标题样式展示

### 一级标题 (h1)
大号字体(36px)，白色文字，绿色背景(#00B25A)，圆角设计，居中显示，背景两侧留有较宽空间，带阴影效果。

### 二级标题 (h2)
中号字体(30px)，白色文字，绿色背景(#00B25A)，圆角设计，居中显示，背景两侧留有较宽空间，带阴影效果。

### 三级标题 (h3)
小号字体(24px)，绿色文字(#00B25A)，左侧粗边框，浅绿色背景，右侧圆角设计。

### 四级标题 (h4)
更小字体(18px)，绿色文字(#00B25A)，左侧带有小圆点装饰。

## 主题特点

- **青春绿设计**：清新的绿色(#00B25A)渐变主色调，简洁的排版
- **层次分明的标题**：四级标题结构清晰，视觉层次丰富
- **美观的列表样式**：列表项使用背景框包裹，视觉效果更佳
- **Mac风格代码块**：代码块采用类似macOS终端的设计，包含红、黄、绿三个窗口按钮
- **精心设计的文本格式**：加粗、下划线、斜体、引用块等文本格式经过精心设计，引用块使用灰色边框避免与标题冲突

## 使用方法

1. 将`modern-theme.css`文件上传到md2card.com平台
2. 在编辑器中选择此主题
3. 开始创作您的内容

## 预览

可以通过打开`theme-preview.html`文件在浏览器中预览主题效果。

## 自定义

如需自定义主题，可以修改`modern-theme.css`文件中的样式。

## 开发注意事项

为了确保主题能在md2card.com平台正确生效，CSS的编写需要遵循特定的结构：

1.  **主题根类名**：您在平台提交的CSS类名（例如 `.theme-youthful-green`）用于定义卡片的**最外层容器**样式，如背景、边框、阴影等。

2.  **内容排版类名**：所有由Markdown转换而来的HTML元素（如标题`h1`, 段落`p`等）都会被包裹在一个固定的 `div` 中，这个 `div` 的类名是 `.card-content-inner`。

3.  **正确书写规则**：因此，所有针对内容排版的样式，都必须以 `.card-content-inner` 作为前缀，例如 `.card-content-inner h1 { ... }`。**请注意**，这部分规则不应再被主题的根类名（如`.theme-youthful-green`）所限定。

简单来说，CSS文件需要分为两部分：一部分是定义卡片外观的根类名样式，另一部分是定义卡片内部内容排版的 `.card-content-inner` 相关样式。

## 公众号模式兼容性问题

### 问题描述
当用户在md2card.com平台开启"公众号模式"时，平台会重构列表的HTML结构，导致原有的列表样式失效。

### 原始HTML结构
```html
<ul>
  <li>列表项内容</li>
</ul>

<ol>
  <li>有序列表项内容</li>
</ol>
```

### 公众号模式下的HTML结构
```html
<!-- 无序列表 -->
<section class="list unordered">
  <section class="list-item">
    <section class="prefix">•</section>
    <section class="content">列表项内容</section>
  </section>
</section>

<!-- 有序列表 -->
<section class="list ordered">
  <section class="list-item">
    <section class="prefix">1</section>
    <section class="content">有序列表项内容</section>
  </section>
</section>
```

### 解决方案
需要在CSS中添加针对公众号模式的兼容样式：

1. **无序列表兼容**：
   - 使用 `.card-content-inner .list.unordered` 选择器
   - 设置容器背景、圆角、内边距等样式
   - 使用 `display: flex` 布局对齐项目符号和内容
   - 为项目符号设置主题色

2. **有序列表兼容**：
   - 使用 `.card-content-inner .list.ordered` 选择器
   - 类似无序列表的样式设置
   - **重要**：需要使用 `::after` 伪元素为数字后添加点号，因为平台只提供数字（如"1"），需要手动添加点号变成"1."

3. **样式优先级**：
   - 所有兼容样式都需要使用 `!important` 声明来覆盖平台默认样式
   - 使用高特异性选择器确保样式生效

### 实现示例
```css
/* 公众号模式下的无序列表 */
.card-content-inner .list.unordered {
  background-color: #f8f9fa !important;
  border-radius: 8px !important;
  padding: 16px 16px 16px 20px !important;
  margin: 16px 0 !important;
}

.card-content-inner .list.unordered .list-item {
  display: flex !important;
  align-items: flex-start !important;
  margin-bottom: 12px !important;
}

.card-content-inner .list.unordered .list-item .prefix {
  color: #00B25A !important;
  font-size: 18px !important;
  font-weight: bold !important;
  margin-right: 8px !important;
}

/* 修复有序列表数字后缺少点号的问题 */
.card-content-inner .list.ordered .list-item .prefix::after {
  content: "." !important;
}
```

### 测试方法
1. 在md2card.com平台创建包含列表的内容
2. 分别在普通模式和公众号模式下预览
3. 确保两种模式下列表样式都正常显示
4. 特别检查有序列表的数字格式是否为"1. 2. 3."而不是"1 2 3"

### 注意事项
- 公众号模式的兼容样式应该放在CSS文件的最后部分
- 所有兼容样式都需要详细的注释说明
- 建议为每个兼容样式添加具体的选择器说明，方便后续维护
