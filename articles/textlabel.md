# 文本标签 API

自 alt:V 更新 v15 起，有了用于创建客户端文本标签的类。这些客户端文本标签更易于创建和使用，因为它们会自动在客户端流式传输。
文本标签是在三维空间中可视化创建的文本，可用于各种用途。请务必将您要使用的字体导入您的 [资源](https://docs.altv.mp/js/articles/create-your-first-resource.html)。

## 用法

[TextLabel class in JS API reference](https://docs.altv.mp/js/api/alt-client.TextLabel.html)<br>
[TextLabel class in C# API reference](https://docs.altv.mp/cs/api/AltV.Net.Client.Elements.Entities.TextLabel.html)<br>

## 服务器结构

下面我们将介绍一般的服务器结构以及如何添加自定义字体。下面只是一个例子，你也可以使用相对路径在资源目录中导入字体。

```
server/
├── cache/
├── data/
├── modules/
├── resources/
    ├── resource1/
    ├── segoeuib.ttf
├── altv-server.exe
├── libnode.dll
├── server.toml
```

### 案例

# [JS Module](#tab/tab1-0)
```js
const labelPos = new alt.Vector3(-4.75, 2.497, 71.217);
const labelRot = new alt.Vector3(0, 0, -0.253);
const labelColor = new alt.RGBA(255, 255, 255, 255);
const labelOutlineColor = new alt.RGBA(0, 0, 0, 80);
const labelFontSize = 32;
const labelFontScale = 2;
const labelOutlineWidth = .25;

alt.once('resourceStart', () => {
  alt.Font.register('mycustomfont.ttf');

  new alt.TextLabel('Welcome to alt:V v16', 'Segoe UI', labelFontSize, labelFontScale, labelPos, labelRot, labelColor, labelOutlineWidth, labelOutlineColor);
});
```
# [C# Module](#tab/tab1-1)
```cs
var labelPos = new Position(-4.75f, 2.497f, 71.217f);
var labelRot = new Rotation(0f, 0f, -0.253f);
var labelColor = new Rgba(255, 255, 255, 255);
var labelOutlineColor = new Rgba(0, 0, 0, 80);
var labelFontSize = 32f;
var labelFontScale = 2f;
var labelOutlineWidth = 0.25f;
        
Alt.OnAnyResourceStart += _ =>
{
  Alt.RegisterFont("mycustomfont.ttf");

  Alt.CreateTextLabel("Welcome to alt:V v16", "Segoe UI", labelFontSize, labelFontScale, labelPos, labelRot, labelColor, labelOutlineWidth, labelOutlineColor, false, 0);
};
```
***

## 格式化

为实现多色或风格化文本，或包含图像，文本标签 API 支持简单的格式化，通过一组 XML 标记完成。

> [!WARNING]
> 无结束标记的标记应以 `/>`.<br>
> 例如: `<br />`, `<img src="..." />`

## 可用的格式化标记:

### `font`

为内部文本设置字体参数。

#### 属性:

| 名称         | 必须? | 描述               | 类型              | 案例    |
| ------------ | --------- | ------------------------- | ------------------- | ---------- |
| size         | no        | Font size (in pixels)     | float               | `16.0`     |
| color        | no        | Text color                | HEX color without # | `FF0000`   |
| outlineColor | no        | Outline color             | HEX color without # | `00FF00`   |
| outline      | no        | Outline width (in pixels) | float               | `5.0`      |
| face         | no        | Font name                 | string              | `Segoe UI` |
| weight       | no        | Font weight               | int                 | `500`      |

#### 案例:

```xml
Nice <font color="FF0000">red</font> text!
```

### `img`

允许在文本中插入图片。

#### 属性:

| 名称     | 必须? | 描述        | 类型 | 案例     |
| -------- | --------- | ------------------ | ------ | ----------- |
| src      | yes       | Image path[^1]     | string | `/test.png` |
| width    | no        | Image width        | float  | `60`        |
| height   | no        | Image height       | float  | `80`        |
| baseline | no        | Image baseline[^2] | float  | `0.1`       |

[^1]: 图像路径通过创建资源或资产包解析。例如
    `/test.png` - 创建资源根目录中的 test.png；
    `@demo/test.png` - 资产包资源 “demo ”根目录中的 test.png

[^2]: 基线决定图像上哪个位置（相对于高度，0.0 为上边缘，1.0 为下边缘）将位于文本基线（文本行底线）水平线上。
    该参数可用于使图像与周围的文本对齐。默认情况下，基线设置为 “0.5”。

#### 支持的图像格式:

- `png`
- `jpg`
- `bmp`
- `tga`

#### 案例:

```xml
alt:V Multiplayer <img src="@demo/logo.png" width="80" baseline="0.9" />
```

### `b`

Makes text **bold** (font weight 700). Has more priority than `font`.

#### 案例:

```xml
This text is <b>bold</b>
```

### `i`

Makes text display as _italic_.

#### 案例:

```xml
This text is <i>italic</i>
```

### `s`

Makes text display as crossed out (strikethrough: ~~example~~).

```xml
This text is <s>crossed out</s>
```

### `u`

Displays a <u>line under the text</u>

```xml
This text is <u>underlined</u>
```

### `br`

Inserts a newline character. Added for convinience, normal newline character is also supported.

```xml
Line 1<br />Line 2
Line 3
```
