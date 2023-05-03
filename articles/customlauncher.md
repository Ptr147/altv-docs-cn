# 介绍  

> [!CAUTION]
> 仅允许完全开放源代码的定制启动器连接到服务器,并且服务器不会列入主服务器列表。

作为定制启动器的替代方案,可以获得品牌启动器。  

品牌启动器可从Patreon"钻石"层或更高层获得,具有多个服务器特定显示,例如RSS提要、启动器和UI背景、徽标、主要颜色和服务器名称。\
为了激活此品牌,连接到服务器时会显示"应用服务器外观"复选框,默认情况下已启用。  

本文介绍所需的信息以及如何请求它。

# 示例  

您可以查看以下示例,了解一些可用定制项:  

- [启动器背景](~/altv-docs-assets/altv-docs-gta/images/customlauncher/launcher_splash.png)
- [界面背景和RSS提要](~/altv-docs-assets/altv-docs-gta/images/customlauncher/user_interface.png)  
- [加载屏幕](~/altv-docs-assets/altv-docs-gta/images/customlauncher/loading_screen.png)
- [激活外观](~/altv-docs-assets/altv-docs-gta/images/customlauncher/apply_skin.png)  

# 请求启动器  

要请求启动器,您必须有活动的Patreon会员资格,级别为"钻石"或更高级别。  

可选地可以包括以下数据来自定义界面:  
- **所有图像必须为PNG格式**  
- 启动器背景,分辨率为300x400 [[示例](~/altv-docs-assets/altv-docs-gta/images/customlauncher/launcher_splash.png)]  
- 徽标,分辨率为128x128 [[示例](~/altv-docs-assets/altv-docs-gta/images/customlauncher/user_interface.png)]  
- UI背景,分辨率为1920x1080或3840x2160 [[示例](~/altv-docs-assets/altv-docs-gta/images/customlauncher/loading_screen.png)]  
- 替换界面主要颜色的颜色 [[示例1](~/altv-docs-assets/altv-docs-gta/images/customlauncher/user_interface.png)] [[示例2](~/altv-docs-assets/altv-docs-gta/images/customlauncher/color_customization.png)]  
- RSS提要的链接。RSS提要的确切格式在后续主题中介绍。[[示例](~/altv-docs-assets/altv-docs-gta/images/customlauncher/user_interface.png)]  

准备好所有必要条件和必要媒体后,请加入我们的[Discord服务器](https://discord.altv.mp/)并联系公共关系团队的成员。

# RSS提要  

## 标题  

RSS提要的标题显示在提要上方。如果RSS提要的'title'属性是一个空字符串,则会使用用户语言的'最新消息'翻译代替。

## 项目属性  

启动器会处理RSS提要条目的以下属性:  

| 属性     | 必需 | 描述                                                                                     |
|-------------|----------|------------------------------------------------------------------------------------------------------------------------|
| title       | 否       | 帖子的标题,显示在帖子开头                                                              |  
| link        | 否       | 用户单击帖子时在浏览器中打开                                                                |
| pubDate     | 是      | 帖子发布日期。以用户语言特定的格式显示在末尾                       |
| description | 是      | 帖子内容。允许使用各种HTML元素。允许的元素在后续主题中介绍。 |
| dc:creator  | 否       | 帖子的创建者。允许使用HTML。           |

## HTML 使用

允许在RSS提要中使用以下HTML元素:\
address, article, aside, footer, header, h1, h2, h3, h4, h5, h6, hgroup, main, nav, section, blockquote, dd, div, dl, dt, figcaption, figure, hr, li, main, ol, p, pre, ul, a, abbr, b, bdi, bdo, br, cite, code, data, dfn, em, i, kbd, mark, q, rb, rp, rt, rtc, ruby, s, samp, small, span, strong, sub, sup, time, u, var, wbr, caption, col, colgroup, table, tbody, td, tfoot, th, thead, tr, img, del

## 特殊 elements

> [!TIP]
> 除非另有规定，否则下面列出的元素的显示样式将遵循 Discord 的显示样式。

### 隐藏剧透(Spoiler)

```html
<span data-spoiler>content</span>
```

显示可以通过用户点击才能显示的剧透。

### 本地化字符串

```html
<!-- 无参数的本地化字符串 -->
<span data-localized>LOCALIZATION_KEY</span>

<!-- 带参数的本地化字符串 -->
<span data-localized="localization argument">LOCALIZATION_KEY</span>
```

翻译后的字符串列表可以在[altv-locales](https://github.com/altmp/altv-locales/tree/master)存储库中找到。 

本地化字符串的用法仅限于 `description` 和 `dc:creator` 属性。

### 时间戳

```html
<span data-timestamp="F">1675455031</span>
```

时间戳的行为与Discord中的时间戳相似,并在用户的时区显示相应的时间。\  
span的内容是Unix时间戳,可能的[格式](https://discord.com/developers/docs/reference#message-formatting-timestamp-styles)基于Discord的语法。  

如果未指定其他格式,则默认使用 "F" 格式。

### 提及

```html
<span data-mention="255, 0, 0">@zziger</span>
```

显示提示。作为属性的值，可以指定用逗号分隔的RGB语法中的颜色\
如果未指定任何值，则使用Discord的默认提及颜色。

### 图片

#### Emoji

```html
<img data-emoji src="https://emoji/icon/url.png" alt="emojiname">
```

将图像显示为一个小的内联元素（1.375em高）\
在描述和 dc:creater 属性中允许。

#### Image row

```html
<span data-images>
    <img src="https://image/one/url.png" alt="image one">
    <img src="https://image/two/url.png" alt="image two">
    <img src="https://image/three/url.png" alt="image three">
</span>
```

在水平滚动列表中显示给定的图像。只有当宽度超过可能的显示时，才会显示滚动条。例如，建议用于画廊。

### 引用(Quote)

```html
<blockquote>quote content</blockquote>
```

将指定的内容显示为引用。

### 代码(Code)

#### Inline

```html
<code>Inline code</code>
```

将指定的文本显示为不带换行符的代码。

#### 代码块(Block)

```html
<pre>
    <code>
        Multi line
        Code block
    </code>
</pre>
```
