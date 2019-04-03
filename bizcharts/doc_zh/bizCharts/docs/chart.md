# 图表构成

## 组件构成
在 BizCharts 中，图表是由各个组件组合而成的。组件有两种类型，实体组件和抽象组件。
- 实体组件：在图表上有对应的图形、文本显示。
- 抽象组件：没有显示，是一种概念抽象组件。

常用图表组件：

| 名称                                                      | 组件类型 | 说明                                                                                         |
| :-------------------------------------------------------- | :------: | :------------------------------------------------------------------------------------------- |
| [`<Chart />`](../api/chart)                               | 实体组件 | 图表父组件，所有的其他组件都必须由 `<Chart>` 包裹。                                          |
| [`<Coord />`](../api/coord)                | 抽象组件 | 坐标系组件。`<Chart><Coord /></Chart>`, 用来描述 `<Chart/> <View />` 组件的坐标系，比如笛卡尔坐标系、极坐标系等。        |
| [`<Axis />`](../api/axis)                  | 实体组件 | 坐标轴组件, `<Chart><Axis /></Chart>`。                                                                                 |
| [`<Geom />`](../api/geom)                  | 实体组件 | 几何标记组件, `<Chart><Geom /></Chart>`。即我们所说的点、线、面这些几何图形。                                           |
| [`<Label />`](../api/label)   | 实体组件 | 几何标记的辅助文本组件, `<Chart><Geom><Label /></Geom></Chart>`。该组件必须作为`<Geom/>` 的子组件。                                   |
| [`<Tooltip />`](../api/tooltip)            | 实体组件 | 提示框组件, `<Chart><Tooltip /></Chart>`。                                                                                 |


非常用图表组件：

| 名称                                                      | 组件类型 | 说明                                                                                         |
| :-------------------------------------------------------- | :------: | :------------------------------------------------------------------------------------------- |
| [`<Guide />`](../api/guide)                | 实体组件 | 辅助标记组件, `<Chart><Guide /></Chart>`。                                                                               |
| [`<Line />`](../api/guide#line)     | 实体组件 | 辅助标记线组件, `<Chart><Guide><Line /></Guide></Chart>`。该组件处于 Guide 组件命名空间下，且必须作为 `<Guide />` 的子组件才会生效。   |
| [`<Image />`](../api/guide#image)   | 实体组件 | 辅助标记图片组件, `<Chart><Guide><Image /></Guide></Chart>`。该组件处于 Guide 组件命名空间下。且必须作为 `<Guide />` 的子组件才会生效   |
| [`<Text />`](../api/guide#text)     | 实体组件 | 辅助标记文本组件, `<Chart><Guide><Text /></Guide></Chart>`。该组件处于 Guide 组件命名空间下。且必须作为 `<Guide />` 的子组件才会生效   |
| [`<Region />`](../api/guide#region) | 实体组件 | 辅助标记举行组件, `<Chart><Guide><Region /></Guide></Chart>`。该组件处于 Guide 组件命名空间下。且必须作为 `<Guide />` 的子组件才会生效   |
| [`<Arc />`](../api/guide#arc)       | 实体组件 | 辅助标记弧形组件, `<Chart><Guide><Arc /></Guide></Chart>`。该组件处于 Guide 组件命名空间下。且必须作为 `<Guide />` 的子组件才会生效   |
| [`<Html />`](../api/guide#html)     | 实体组件 | 辅助标记 html 组件, `<Chart><Guide><Html /></Guide></Chart>`。该组件处于 Guide 组件命名空间下。且必须作为 `<Guide />` 的子组件才会生效 |
| [`<RegionFilter />`](#RegionFilter) | 实体组件 |辅助框组件，`<Chart><Guide><RegionFilter /></Guide></Chart>`，框选一段图区，设置背景、边框等。|
| [`<DataMarker />`](#DataMarker) | 实体组件 | 辅助 html，`<Chart><Guide><DataMarker /></Guide></Chart>`，指定位置添加自定义 html，显示自定义信息。|
| [`<DataRegion />`](#DataRegion) | 实体组件 |辅助弧线，`<Chart><Guide><DataRegion /></Guide></Chart>`。|
| [`<Facet />`](../api/facet)                | 抽象组件 | 分面组件，`<Chart><Facet /></Chart>`。                                                                                   |
| [`<View />`](../api/view)                  | 抽象组件 | 视图组件，`<Chart><View /><View /></Chart>`。                                                                                   |

图表插件有:

| 名称                                | 类型     | 说明                                |
| :---------------------------------- | :------- | :---------------------------------- |
| [`<Slider />`](../api/sliderPlugin) | 图标插件 | 使用前必须确定已经安装了 Bizcarts。 |

## 空间构成
下图所示为常用图表的各组件的空间构成。

![7df8bc11-09dc-4d8d-9832-54364f594501.png](https://img.alicdn.com/tfs/TB105z4efDH8KJjy1XcXXcpdXXa-2030-1480.png)
