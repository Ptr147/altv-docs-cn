# 名字标签示例

本文介绍名称标签作为RmlUi示例。
此示例使用的所有文件可以在以下[仓库](https://github.com/altmp/altv-example-resources/tree/master/rml-nametags)中找到。

## 要求

- 一些代码编辑器(例如Visual Studio Code)
- 基本的HTML和CSS知识
- 字体文件(.ttf)
- 空资源与`Client`子文件夹和`Client/main.js`作为入口点

## 创建rml文档 

> [!TIP]
> 在此示例中使用`arialbd.ttf`作为字体,可以在上面提到的仓库中找到。

创建Rml文档相对简单,除了前导`<rml>`标签和XHTML1标准之外,与创建.html文件没有什么不同。\
我们在`Client`子文件夹中创建`index.tml`文件,并在其中添加如下内容:

```html
<!-- 对RML UI文档来说,我们的HTML必须以<rml> 标签开始 --> 
<rml> 
     <!-- 其他的全是基于XHTML 1标准的HTML --> 
    <head>
        <title>演示</title>
        <style>
            body, #nametag-container {
                <!-- 字体需要包含我们后面要加载的字体文件 -->     
                font-family: arial;
                font-weight: bold;
                font-style: normal;
                font-weight: bold;

                width: 100%;
                height: 100%;
            }

            .nametag {
                position: absolute;
                font-effect: outline(1px black);
                color: #FFFFFFFF;
                width: auto;
                height: auto;
                text-align: center;
                font-size: 20dp;
            }

            .hide {
                display: none;
            }

        </style>
    </head>
    <body>
    <!-- 这是我们后面客户端脚本中将使用的容器 --> 
    <div id="nametag-container"></div>
    </body>
</rml>
```

## 创建客户端脚本 

> [!TIP]
> 尽管这个示例中使用了JavaScript,它也可以与C#和其他模块一起使用。 
> 可以在各个模块的文档中找到更多信息。

创建我们的`index.rml`后,我们在同一文件夹中创建名为`main.js`的脚本文件。 
在这个文件中,我们添加以下内容,稍后将根据注释对其进行详细说明:

```js
import * as alt from "alt-client";
import * as native from "natives";

// ---------------- 配置 ---------------- 
// showVehicleIds:`true` 如果为true,车辆的姓名标签也将包含其ID 
// showPlayerIds:`true` 如果为true,玩家的姓名标签将包含其ID 
// showPlayerNames:`true` 如果为true,玩家的姓名标签将包含其名称 
// checkLoS:`true` 如果为true,需要视线才能绘制姓名标签 
// dynamicSize:`true` 如果为true,大小会根据实体的距离增加或减小 
// controlKey:`79` 激活或停用RML控件的切换键。79是O键 
// 
// 注意:对于玩家,要么启用ID,要么启用名称,要么两者都启用,否则不会绘制姓名标签

const showVehicleIds = true;
const showPlayerIds = true;
const showPlayerNames = true;
const checkLoS = true;
const dynamicSize = true;
const controlKey = 79;

// --------------- Prototype --------------
// We use a custom prototype on the RmlElement to quickly check if its currently drawn or not

alt.RmlElement.prototype.shown = false;

/ ---------------- 脚本 ----------------
// 所有的代码。每行都会包含更详细的说明 
// 对于RML,您必须加载需要的字体,以便可以使用它 

alt.loadRmlFont("/Client/arialbd.ttf", "arial", false, true);
// 创建一个新的RML文档。这类似于创建一个新的Web视图
const document = new alt.RmlDocument("/Client/index.rml");
// 我们存储nametag-container以供进一步使用,例如添加和删除姓名标签
const container = document.getElementByID("nametag-container");
// 我们使用map简化实体和RmlElement之间的映射
const nameTags = new Map();
// 这个变量将存储我们每次滴答的ID,以便在没有姓名标签要绘制时立即禁用它
let tickHandle = undefined;

// 我们将使用gameEntityCreate事件向容器添加新姓名标签
alt.on("gameEntityCreate", (entity) => {
    // 创建一个<button> HTML元素并将其存储在rmlElement变量中
    const rmlElement = document.createElement("button");
    // 设置HTML元素的id属性
    rmlElement.id = entity.id.toString();
    // 添加我们的类以应用样式
    rmlElement.addClass("nametag");
    rmlElement.addClass("hide");

    / 根据顶部的配置,我们会根据配置绘制不同的姓名标签 
     // 如果没有配置适用于实体类型,我们将销毁RmlElement
    if (entity instanceof alt.Player) {
        if (showPlayerIds && !showPlayerNames)
            rmlElement.innerRML = `ID: ${entity.id}`;
        else if (showPlayerIds && showPlayerNames)
            rmlElement.innerRML = `ID: ${entity.id} | Name: ${entity.name}`;
        else if (!showPlayerIds && showPlayerNames)
            rmlElement.innerRML = `Name: ${entity.name}`;
        else {
            rmlElement.destroy();
            return;
        }
    } else if (entity instanceof alt.Vehicle && showVehicleIds)
        rmlElement.innerRML = `ID: ${entity.id}`;
    else {
        rmlElement.destroy();
        return;
    }

    // 将RmlElement和Entity添加到我们的map中 
    nameTags.set(entity, rmlElement);
    // 将RmlElement添加到我们的RmlDocument。一旦删除了“hide”类,它就会被绘制 
    container.appendChild(rmlElement);
    // 我们订阅单击事件,以便在需要时打印出实体坐标 
    rmlElement.on("click", printCoordinates);

    // 如果已经激活了everyTick,我们可以直接返回,否则我们将启动我们的everyTick 
    if (tickHandle !== undefined) return;
    tickHandle = alt.everyTick(drawMarkers);
});

// 与gameEntityCreate事件相同,但在这里我们将删除姓名标签 
alt.on("gameEntityDestroy", (entity) => {
    // 我们从地图中获取实体的rmlElement 
    const rmlElement = nameTags.get(entity);
    // 如果没有找到,例如因为创建条件不匹配,我们可以在这里直接返回 
    if (rmlElement === undefined) return;
    // 从容器中删除RmlElement...... 
    container.removeChild(rmlElement);
     // ...并结束其存在。
    rmlElement.destroy();
     // 从姓名标签映射中删除 
    nameTags.delete(entity);

    // 如果没有更多的姓名标签剩下,我们停止每次滴答以节省性能 
    if (tickHandle === undefined || nameTags.size > 0) return;
    alt.clearEveryTick(tickHandle);
    tickHandle = undefined;
});

// 我们使用keyup事件启用或禁用控件 
alt.on("keyup", (key) => {
    // 如果键不是我们的控制键,我们不需要任何进一步处理 
    if (key !== controlKey) return;

    // 获取当前的控制状态 
    const currentState = alt.rmlControlsEnabled();
    if (currentState) {
       // 如果已经启用,则禁用控件...... 
        alt.toggleGameControls(true);
        alt.showCursor(false);
        alt.toggleRmlControls(false);
    } else {
        // 或者如果已禁用,则启用
        alt.toggleGameControls(false);
        alt.showCursor(true);
        alt.toggleRmlControls(true);
    }
});

// 此功能来自单击事件 
// 事件参数可以在rmlui文档中找到:https://mikke89.github.io/RmlUiDoc/pages/rml/events.html 
function printCoordinates(rmlElement, eventArgs) {
    // 在这里我们访问先前设置给rmlElement的id 
    const entity = alt.Entity.getByID(parseInt(rmlElement.id));
    alt.log("Entity Position", "X", entity.pos.x, "Y", entity.pos.y, "Z", entity.pos.z);
}

// 此功能来自我们的everyTick 
function drawMarkers() {
    // 我们循环遍历地图中的所有实体
    nameTags.forEach((rmlElement, entity) => {
        // 获取它们的位置 
        const {x, y, z} = entity.pos;

        // 检查它们是否在屏幕上,并且如果启用了视线检查,则在我们之间没有障碍, 
        if (!native.isSphereVisible(x, y, z, 0.0099999998) || (checkLoS && !native.hasEntityClearLosToEntity(alt.Player.local, entity, 17))) {
            // 如果我们看不到它,并且RmlElement已经隐藏,我们可以直接返回 
            if (!rmlElement.shown) return;

            // 否则我们要通过添加css类来隐藏它......
            rmlElement.addClass("hide");
             // ...并设置我们的自定义属性
            rmlElement.shown = false;
        } else {
            // 实体可见,所以我们检查RmlElement是否仍然隐藏 
            if (!rmlElement.shown) {
                 // 如果是,我们删除隐藏类...... 
                rmlElement.removeClass("hide");
                // ...并将shown属性设置为true 
                rmlElement.shown = true;
            }

            // 现在我们获取实体的屏幕坐标
            const {x: screenX, y: screenY} = alt.worldToScreen(x, y, z + 2);
            // 并将其应用于我们的RmlElement
            rmlElement.style["left"] = `${screenX}px`;
            rmlElement.style["top"] = `${screenY}px`;

            // 如果动态大小被禁用,我们可以简单地在这里返回...... 
            if (!dynamicSize) return;
            // ...否则我们检查距离,在这个例子中,我们将使用最小尺寸1dp和最大尺寸50dp 
            // 这将根据从1到100米的距离应用。低于或高于的所有内容将使用
            // 1或50作为字体大小。
            const fontSizeModificator = Math.min(entity.pos.distanceTo(alt.Player.local.pos) / 100, 1);
            const fontSize = (1 - fontSizeModificator) * 50;
            // 计算后,我们修改样式属性以使用新的字体大小
            rmlElement.style["font-size"] = `${fontSize}dp`;
        }
    });
}
```

## 总结

在我们同时创建了`index.rml`和`main.js`以及添加了字体文件之后,RmlUi 资源已经准备就绪。

尤其值得注意的是,HTML元素可以直接通过客户端代码控制,因此可以在没有事件的情况下进行更新。  
这就是RmlUi可以达到更高性能的原因之一。