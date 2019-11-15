# Guide

用于绘制图表的辅助元素。

<img src="https://gw.alipayobjects.com/zos/rmsportal/ekrHtCkdfMttjnAXqApH.png" width="500px">

## 使用说明
### 父组件
[`<Chart />`](chart) [`<View />`](view)

### 子组件

| 组件                                 | 说明                                                   |
| :----------------------------------- | :----------------------------------------------------- |
| [`<Guide><Line /></Guide>`](#line)     | 辅助线（可带文本），例如表示平均值或者预期分布的直线。 |
| [`<Guide><Image /></Guide>`](#image)   | 辅助图片，在图表上添加辅助图片。                       |
| [`<Guide><Text /></Guide>`](#text)     | 辅助文本，指定位置添加文本说明。                       |
| [`<Guide><Region /></Guide>`](#region) | 辅助框，框选一段图区，设置背景、边框等。               |
| [`<Guide><Html /></Guide>`](#html)     | 辅助 html，指定位置添加自定义 html，显示自定义信息。   |
| [`<Guide><Arc /></Guide>`](#arc)       | 辅助弧线。                                             |
| [`<Guide><RegionFilter /></Guide>`](#RegionFilter) | 辅助框，框选一段图区，设置背景、边框等。|
| [`<Guide><DataMarker /></Guide>`](#DataMarker) | 辅助 html，指定位置添加自定义 html，显示自定义信息。|
| [`<Guide><DataRegion /></Guide>`](#DataRegion) | 辅助弧线。|

### 使用注意
- Line是Guide的API, [详见demo](https://bizcharts.net/products/bizCharts/demo/detail?id=line-series)

```js
import { Chart, Geom, Axis, Tooltip, Guide } from 'bizcharts';
const { Line } = Guide;

<Chart height={400} data={data} scale={cols} forceFit>
  <Geom type="line" position="month*revenue" size={2} color={'city'} />
  <Guide>
    <Line
      top // {boolean} 指定 guide 是否绘制在 canvas 最上层，默认为 false, 即绘制在最下层
      start={{ month: 'Aug', revenue: 26.5 }} // {object} | {function} | {array} 辅助线结束位置，值为原始数据值，支持 callback
      end={{ month: 'Dec', revenue: 29 }} // 同 start
      lineStyle={{
	stroke: '#999', // 线的颜色
	lineDash: [0, 2, 2], // 虚线的设置
	lineWidth: 3, // 线的宽度
      }} // 图形样式配置 https://bizcharts.net/products/bizCharts/api/graphic
      text={{
	position: 'start', // 'start' | 'center' | 'end' | '39%' | 0.5 文本的显示位置
	autoRotate: true, // {boolean} 是否沿线的角度排布，默认为 true
	style: { fill: 'red' }, // {object}文本图形样式配置
	// content: {string}, // {string} 文本的内容
	offsetX: 20, // {number} x 方向的偏移量
	offsetY: -10, // {number} y 方向的偏移量
	content: '预期收益趋势线',
      }}
    />
  </Guide>
</Chart>
```



### 属性说明
- 不同辅助标记组件所支持的配置属性也不一样，主要差异为坐标位置属性差异:
  - Text, Html 中使用 position
  - Line, Region, Image, Arc 中使用 start、end 字段

**位置值说明**
- Object 使用图表 x,y 对应的原始数据例如： {time: ‘2010-01-01’, value: 200}
- Array 数组来配置位置 [x, y]，根据数组中的值的存在以下几种形式：
	- x，y 都是原始数据 [‘2010-01-01’]
	- x，y 可以使用原始数据的替代字符串 ‘min’, ‘max’, ‘median’ , 例如：[‘median’, 200]
	- x, y 都是用百分比的形式，在绘图区域定位，字符串中存在 ‘%’, 例如[‘50%’, ‘50%’] 使得辅助元素居中
- Function 回调函数，可以动态的确定辅助元素的位置

```js
// 应用于数据动态更新，辅助元素的位置根据数据变化
<Guide>
  <Text
    content='最大值'
    position={(xScale, yScale)=>{
      return []; //位置信息
    }}
  />
</Guide>
```

## 使用示例
![e051f3e7-35ab-4895-8aa6-89fba3045da9.png](https://img.alicdn.com/tfs/TB16XZ8bOqAXuNjy1XdXXaYcVXa-1186-510.png)

```js
<Chart width={600} height={400} data={data}>
  <Guide>
    <Region start={[-1, 0]} end={[1, ranges[0]]} style={{fill: '#e96e33',
    fillOpacity: 0.5}}/>
    <Region start={[-1, ranges[0]]} end={[1, ranges[1]]} style={{fill: '#f9ca47',
    fillOpacity: 0.5}}/>
    <Region start={[-1, ranges[1]]} end={[1, ranges[2]]} style={{fill: '#88bb34',
    fillOpacity: 0.5}}/>
  </Guide>
</Chart>
```
[demo链接](../demo/detail?id=other-bullet-graph&selectedKey=%E5%85%B6%E4%BB%96%E5%9B%BE%E8%A1%A8)

## API

### Guide
`<Guide>` 本身并没有属性需要配置，该组件作为一个管理标签，用来管理其子组件的渲染与更新。任意Guide更新会导致这这组内的其他Guide重绘。

<span id = "line"></span>
### Line

```js
<Guide>
  <Line
    top={boolean} // 指定 guide 是否绘制在 canvas 最上层，默认为 false, 即绘制在最下层
    start= {{object} | {function} | {array}} // 辅助线起始位置，值为原始数据值，支持 callback
    end={{object} | {function} | {array}} // 辅助线结束位置，值为原始数据值，支持 callback
    lineStyle= {{
      stroke: '#999', // 线的颜色
      lineDash: [ 0, 2, 2 ], // 虚线的设置
      lineWidth: 3 // 线的宽度
    }} // 图形样式配置
    text={{
      position: 'start' | 'center' | 'end' | '39%' | 0.5, // 文本的显示位置
      autoRotate: {boolean}, // 是否沿线的角度排布，默认为 true
      style: {object}, // 文本图形样式配置
      content: {string}, // 文本的内容
      offsetX: {number}, // x 方向的偏移量
      offsetY: {number} // y 方向的偏移量
      content: "xxxx"
    }}
  />
</Guide>
```
![image](https://cdn.nlark.com/yuque/0/2018/png/100996/1539840373775-8ae52cb7-b1b8-4ef3-9053-006721abf61c.png)
<br/>
[详见 Guide Line demo](https://bizcharts.net/products/bizCharts/demo/detail?id=line-series)


#### `top`
* 类型：Boolean
* 描述：指定 guide 是否绘制在 canvas 最上层，默认为`false`, 即绘制在最下层。

#### `start`
* 类型： Object | Function | Array
* 描述：指定辅助线的起始位置，该值的类型如下：
  - object: 使用图表 x,y 对应的原始数据例如： { time: '2010-01-01', value: 200 }
  - array: 数组来配置位置 [ x, y ]，根据数组中的值的存在以下几种形式：
    - x，y 都是原始数据 [ '2010-01-01', 200 ];
    - x，y 可以使用原始数据的替代字符串 'min', 'max', 'median' , 例如：[ 'median', 200 ]
    - x, y 都是用百分比的形式，在绘图区域定位，字符串中存在 '%', 例如 [ '50%', '50%' ] 使得辅助元素居中
  - function: 回调函数，可以动态的确定辅助元素的位置，应用于数据动态更新，辅助元素的位置根据数据变化的场景

  ```js
    <Guide>
      <Line start={(xScale, yScale) => {
        return []; //位置信息
      }} />
    </Guide>
  ```

#### `end`
* 类型： Object | Function | Array
* 描述：指定辅助线的结束位置，使用同 start。

#### `lineStyle`
* 类型： Object
* 描述：辅助线样式配置。[更详细配置](./graphic)

#### `text`
* 类型： Object
* 描述：辅助线可以带文本，该属性使用如下：

```js
<Guide>
  <Line
    text={{
      position: 'start' | 'center' | 'end' | '39%' | 0.5, // 文本的显示位置，值除了指定的常量外，还可以是百分比或者小数
      autoRotate: {boolean}, // 指定文本是否沿着线的方向排布，默认为 true，即沿着线排布
      // 设置文本的显示样式
      style: {
        fill: 'red'
      },
      content: {string}, // 文本的内容
      offsetX: {number}, // x 方向的偏移量
      offsetY: {number} // y 方向的偏移量
    }}
  />
</Guide>
```

<span id = "text"></span>
### Text

```js
<Guide>
  <Text
    top= {boolean} // 指定 guide 是否绘制在 canvas 最上层，默认为 false, 即绘制在最下层
    position= {object} | {function} | {array} // 文本的起始位置，值为原始数据值，支持 callback
    content= {string} // 显示的文本内容
    style= {{
      fill: '#666', // 文本颜色
      fontSize: '12', // 文本大小
      fontWeight: 'bold' // 文本粗细
      rotate: 30 // 旋转角度
    }} // 文本的图形样式属性
    offsetX= {number} // x 方向的偏移量
    offsetY= {number} // y 方向偏移量
  />
</Guide>
```
![image](https://cdn.nlark.com/yuque/0/2018/png/100996/1539840429277-53a8e36c-8a7e-41cf-93fd-43ca7c2ab444.png)

#### `top`
* 类型： Boolean
* 描述：指定 giude 是否绘制在 canvas 最上层。
默认值:false, 即绘制在最下层。

#### `position`
* 类型：  Object | Function | Array
* 描述：指定辅助文本的显示位置，该值的类型如下：
    - object: 使用图表 x,y 对应的原始数据例如： { time: '2010-01-01', value: 200 }
    - array: 数组来配置位置 [ x, y ]，根据数组中的值的存在以下几种形式：
    - x，y 都是原始数据 [ '2010-01-01', 200 ];
    - x，y 可以使用原始数据的替代字符串 'min', 'max', 'median' , 例如：[ 'median', 200 ]
    - x, y 都是用百分比的形式，在绘图区域定位，字符串中存在 '%', 例如 [ '50%', '50%'] 使得辅助元素居中
    - function: 回调函数，可以动态的确定辅助元素的位置，应用于数据动态更新，辅助元素的位置根据数据变化的场景

    ```js
    <Guide>
      <Text position={(xScale, yScale) => [x, y]} > // 返回位置信息
    </Guide>
    ```

#### `content`
* 类型：  String
* 描述：辅助文本的显示内容。

#### `style`
* 类型： Object
* 描述：用于设置辅助文本的显示样式。[更详细配置](./graphic)

#### `offsetX`
* 类型：  Number
* 描述：x 方向的偏移量。

#### `offsetY`
* 类型：  Number
* 描述：y 方向的偏移量。

<span id = "image"></span>
### Image

```js
<Guide>
  // 辅助图片 image，只是指定了 start，则该点表示图片左上角坐标
  <Image
    top = {boolean} // 指定 giude 是否绘制在 canvas 最上层，默认为 false, 即绘制在最下层
    start= {object} | {function} | {array} // 图片起始位置， 值为原始数据值，支持 callback
    end= {array} | {function} | {array} // 图片结束位置， 值为原始数据值，支持 callback
    src= {string} // 图片路径
    width= {number}
    height= {number}
    offsetX= {number} // x 方向的偏移量
    offsetY= {number} // y 方向偏移量
  />
</Guide>
```
![image](https://cdn.nlark.com/yuque/0/2018/png/100996/1539840397466-d3d5636d-aad3-4b51-bb25-83291c3523ec.png)

#### `top`
* 类型：  Boolean
* 描述：指定 giude 是否绘制在 canvas 最上层。
默认值:`false`, 即绘制在最下层。

#### `start`
* 类型：  Object | Function | Array
* 描述： 指定辅助图片的起始位置，即图片的左上角，该值的类型如下：
  - object: 使用图表 x,y 对应的原始数据例如： { time: '2010-01-01', value: 200 }
  - array: 数组来配置位置 [ x, y ]，根据数组中的值的存在以下几种形式：
    - x，y 都是原始数据 [ '2010-01-01', 200 ];
    - x，y 可以使用原始数据的替代字符串 'min', 'max', 'median' , 例如：[ 'median', 200 ]
    - x, y 都是用百分比的形式，在绘图区域定位，字符串中存在 '%', 例如 [ '50%', '50%' ] 使得辅助元素居中
  - function: 回调函数，可以动态的确定辅助元素的位置，应用于数据动态更新，辅助元素的位置根据数据变化的场景

#### `end`
* 类型：  Object | Function | Array
* 描述：可选，指定辅助图片的结束位置，即图片的右下角，该属性用法同 start。当 start 和 end 属性同时声明时，即指定了图片的宽度和高度。

#### `src`
* 类型： String
* 描述：指定图片的地址，可以是路径，也可以是 base64 编码。

#### `width`
* 类型： Number
* 描述：当仅指定了 start 属性时，用于设置图片的宽度。

#### `height`
* 类型： 	Number
* 描述：当仅指定了 start 属性时，用于设置图片的高度。

#### `offsetX`
* 类型：  Number
* 描述：x 方向的偏移量。

#### `offsetY`
* 类型：Number
* 描述：y 方向的偏移量。

<span id = "region"></span>
### Region

```js
<Guide>
  <Region
    top={boolean} // 指定 giude 是否绘制在 canvas 最上层，默认为 false, 即绘制在最下层
    start= {object} | {function} | {array} // 辅助框起始位置，值为原始数据值，支持 callback
    end= {object} | {function} | {array}// 辅助框结束位置，值为原始数据值，支持 callback
    style= {{
      lineWidth: 0, // 辅助框的边框宽度
      fill: '#f80', // 辅助框填充的颜色
      fillOpacity: 0.1, // 辅助框的背景透明度
      stroke: '#ccc' // 辅助框的边框颜色设置
    }} // 辅助框的图形样式属性
    />
</Guide>
```
![image](https://cdn.nlark.com/yuque/0/2018/png/100996/1539840452881-05e46776-a498-4037-9741-9c2df36a665e.png)

#### `top`
* 类型：Boolean
* 描述：指定 giude 是否绘制在 canvas 最上层。
默认值:false, 即绘制在最下层。

#### `start`
* 类型：Object | Function | Array
* 描述：指定辅助背景框的起始位置，即背景框的左上角，该值的类型如下：
  - object: 使用图表 x,y 对应的原始数据例如： { time: '2010-01-01', value: 200 }
  - array: 数组来配置位置 [ x, y ]，根据数组中的值的存在以下几种形式：
    - x，y 都是原始数据 [ '2010-01-01', 200 ];
    - x，y 可以使用原始数据的替代字符串 'min', 'max', 'median' , 例如：[ 'median', 200 ]
    - x, y 都是用百分比的形式，在绘图区域定位，字符串中存在 '%', 例如 [ '50%', '50%' ] 使得辅助元素居中
  - function: 回调函数，可以动态的确定辅助元素的位置，应用于数据动态更新，辅助元素的位置根据数据变化的场景

#### `end`
* 类型：Object | Function | Array
* 描述：指定辅助背景框的结束位置，即背景框的右下角，该属性用法同 start。

#### `style`
* 类型：Object
* 描述：辅助框图形样式属性。[更详细配置](./graphic)

<span id = "html"></span>
### Html

```js
<Guide>
  <Html
    position={object} | {function} | {array} // html 的中心位置， 值为原始数据值，支持 callback
    alignX='left' | 'middle' | 'right'
    alignY='top' | 'middle' | 'bottom'
    offsetX={number}
    offsetY={number}
    html={string} // html 代码
    zIndex={number}
  />
</Guide>
```
![image](https://cdn.nlark.com/yuque/0/2018/png/100996/1539840479814-39752f61-071c-4145-8705-efc0264e2c2e.png)

#### `position`
* 类型：Object | Function |Array
* 描述：设置 html 的显示位置，该值的类型如下：
  - object: 使用图表 x,y 对应的原始数据例如： { time: '2010-01-01', value: 200 }
  - array: 数组来配置位置 [ x, y ]，根据数组中的值的存在以下几种形式：
    - x，y 都是原始数据 [ '2010-01-01', 200 ];
    - x，y 可以使用原始数据的替代字符串 'min', 'max', 'median' , 例如：[ 'median', 200 ]
    - x, y 都是用百分比的形式，在绘图区域定位，字符串中存在 '%', 例如 [ '50%', '50%' ] 使得辅助元素居中
  - function: 回调函数，可以动态的确定辅助元素的位置，应用于数据动态更新，辅助元素的位置根据数据变化的场景

#### `zIndex`
* 类型：Number
* 描述：html 层级。

#### `alignX`
* 类型：String
* 描述：html 的水平对齐方式，可取值为：`'left' | 'middle' | 'right'`，默认值为 `middle`。

#### `alignY`
* 类型：String
* 描述：html 的垂直对齐方式，可取值为：`'top' | 'middle' | 'bottom'`，默认值为 `middle`。

#### `offsetX`
* 类型：Number
* 描述：x 方向的偏移量。

#### offsetY
* 类型：Number
* 描述：y 方向的偏移量。

#### html
* 类型：	String | Function
* 描述：需要显示的 html 内容。

[demo链接](https://bizcharts.net/products/bizCharts/demo/detail?id=guide-html&selectedKey=折线图)

```js
  const { Html } = Guide;
  <Chart width={600} height={400} data={data} scale={scales} padding="auto">
    <Axis name="sold" />
    <Axis name="genre" />
    <Geom type="line" position="genre*sold" color="sold" />
    <Guide>
      <Html position={[ 'Action', 150 ]} html={(xScale, yScale) => {
        console.log(yScale);
        return `<div>最大值是${yScale.sold.max}，<br/>最小值是${yScale.sold.min}</div>`; //位置信息
      }} />
    </Guide>
  </Chart>
```

<span id = "arc"></span>
### Arc

```js
<Guide>
  <Arc
    top={object} // 指定 giude 是否绘制在 canvas 最上层，默认为 false, 即绘制在最下层
    start={object} | {function} | {array} // 辅助框起始位置，值为原始数据值，支持 callback
    end={object} | {function} | {array}// 辅助框结束位置，值为原始数据值，支持 callback
    style={object} // 图形样式属性
  />
</Guide>
```
`注意`:
* arc辅助弧线仅在polar[坐标系（Coord）](./coord)下生效。

#### `top`
* 类型：Boolean
* 描述：指定 giude 是否绘制在 canvas 最上层。
默认值： `false`, 即绘制在最下层。

#### `start`
* 类型：	Object | Function | Array
* 描述：指定辅助圆弧的起始位置，该值的类型如下：
  - object: 使用图表 x,y 对应的原始数据例如： { time: '2010-01-01', value: 200 }
  - array: 数组来配置位置 [ x, y ]，根据数组中的值的存在以下几种形式：
    - x，y 都是原始数据 [ '2010-01-01', 200 ];
    - x，y 可以使用原始数据的替代字符串 'min', 'max', 'median' , 例如：[ 'median', 200 ]
    - x, y 都是用百分比的形式，在绘图区域定位，字符串中存在 '%', 例如 [ '50%', '50%' ] 使得辅助元素居中
  - function: 回调函数，可以动态的确定辅助元素的位置，应用于数据动态更新，辅助元素的位置根据数据变化的场景


#### `end`
* 类型：Object | Function | Array
* 描述：指定辅助圆弧的结束位置，该属性用法同 start。

#### `style`
* 类型：Object
* 描述：设置圆弧的显示样式。[更详细配置](./graphic)

<span id = "RegionFilter"></span>
### RegionFilter
辅助区域过滤，将图表中位于矩形选区中的图形元素提取出来，重新着色。

```js
<Guide>
  <RegionFilter
  top={boolean} // 指定 giude 是否绘制在 canvas 最上层，默认为 true, 即绘制在最上层
  start={object} | {function} | {array} // 辅助框起始位置，值为原始数据值，支持 callback
  end={object} | {function} | {array}// 辅助框结束位置，值为原始数据值，支持 callback
  color='#ccc' //染色色值
  apply={array} //可选，设定regionFilter只对特定geom类型起作用  />
</Guide>
```
![image](https://cdn.nlark.com/yuque/0/2018/png/100996/1539840527395-f4893edb-7867-43de-8b46-1ac0a9ff06a8.png)

#### `top`
* 类型: Boolean
* 描述: 指定 guide 是否绘制在 canvas 最上层，默认为 false, 即绘制在最下层。

#### `start`
* 类型： Object | Function | Array
* 描述：指定辅助线的起始位置，该值的类型如下：
  - object: 使用图表 x,y 对应的原始数据例如： { time: '2010-01-01', value: 200 }
  - array: 数组来配置位置 [ x, y ]，根据数组中的值的存在以下几种形式：
    - x，y 都是原始数据 [ '2010-01-01', 200 ];
    - x，y 可以使用原始数据的替代字符串 'min', 'max', 'median' , 例如：[ 'median', 200 ]
    - x, y 都是用百分比的形式，在绘图区域定位，字符串中存在 '%', 例如 [ '50%', '50%' ] 使得辅助元素居中
  - function: 回调函数，可以动态的确定辅助元素的位置，应用于数据动态更新，辅助元素的位置根据数据变化的场景
  ```js
    <Guide>
      <RegionFilter start={(xScale, yScale) => {
        return []; //位置信息
      }} />
    </Guide>
  ```

#### `end`
* 类型: Object | Function | Array
* 描述: 指定辅助线的结束位置，使用同 start。

#### `color`
* 类型: String
* 描述: 指定辅助过滤区域内图形元素重新着色的色值。

#### `apply`
* 类型: array
* 描述: 可选,设定regionFilter只对特定geom类型起作用，如apply:['area'],默认regionFilter的作用域为整个图表

<span id = "DataMarker"></span>
### DataMarker
特殊数据标注点，适用于折线图和面积图。默认状态的特殊数据标注点由point、line、text三部分组成，同时开放接口对各部分是否显示及显示样式等进行设置
```js
<Guide>
  <DataMarker
    top={true | false} // 指定 giude 是否绘制在 canvas 最上层，默认为true, 即绘制在最上层
    position={{object} | {function} | {array}} // 标注点起始位置，值为原始数据值，支持 callback,
    content={string} // 显示的文本内容
    style={{
      text: {object},
      point:{object},
      line:{object}
    }}//可选，文本/point/line样式
    display={{
      text:{boolean},
      point:{boolean},
      line:{boolean}
    }}//可选，是否显示文本/point/line，默认为全部显示
    lineLength={{number}}//可选，line长度，default为30
    direction={'upward' | 'downward'} //可选，朝向，默认为upwaard
  />
</Guide>
```
![image](https://cdn.nlark.com/yuque/0/2018/png/100996/1539840618454-c50783aa-0717-4c18-b2c2-5a7c4da21dd0.png)

#### `top`
* 类型: Boolean
* 描述: 指定 guide 是否绘制在 canvas 最上层，默认为 false, 即绘制在最下层。

#### `position`
* 类型： Object | Function | Array
* 描述：指定辅助线的起始位置，该值的类型如下：
  - object: 使用图表 x,y 对应的原始数据例如： { time: '2010-01-01', value: 200 }
  - array: 数组来配置位置 [ x, y ]，根据数组中的值的存在以下几种形式：
    - x，y 都是原始数据 [ '2010-01-01', 200 ];
    - x，y 可以使用原始数据的替代字符串 'min', 'max', 'median' , 例如：[ 'median', 200 ]
    - x, y 都是用百分比的形式，在绘图区域定位，字符串中存在 '%', 例如 [ '50%', '50%' ] 使得辅助元素居中
  - function: 回调函数，可以动态的确定辅助元素的位置，应用于数据动态更新，辅助元素的位置根据数据变化的场景
  ```js
    <Guide>
      <DataMarker start={(xScale, yScale) => {
        return []; //位置信息
      }} />
    </Guide>
  ```

#### `content`
* 类型: String
* 描述: 辅助文本的显示内容。

#### `style`
* 类型: Object
* 描述: style:{ point:{}, line:{}, text:{} }， point/line/text样式。[更详细配置](./graphic)

#### `display`
* 类型: Object
* 描述: display:{ point:true | false, line:true | false, text:true | false }， 是否显示point/line/text。

#### `lineLength`
* 类型: Number
* 描述: line的长度
* 默认值: 30

#### `direction`
* 类型: String
* 描述: 标注点朝向：'upward' | 'downward'
* 默认值: 'upward'，即向上

[demo链接](../demo/detail?id=other-datamarker_dataregion&selectedKey=其他图表)

<span id = "DataRegion"></span>
### DataRegion
特殊数据区间标注，适用于折线图和面积图。
```js
<Guide>
  <DataRegion
    top={true | false} // 指定 giude 是否绘制在 canvas 最上层，默认为 true, 即绘制在最上层
    start={{object} | {function} | {array}} // 标注点起始位置，值为原始数据值，支持 callback ,
    end={{object} | {function} | {array}} // 标注点结束位置，值为原始数据值，支持 callback ,
    content={string} // 显示的文本内容
    style={{
      text: {object},
      point:{object},
      line:{object}
    }}//可选，文本/point/line样式
    display={{
      text:{boolean},
      point:{boolean},
      line:{boolean}
    }}//可选，是否显示文本/point/line，默认为全部显示
    lineLength={{number}}//可选，line长度，default为30
    direction={'upward' | 'downward'} //可选，朝向，默认为upwaard
  />
</Guide>
```
![image](https://cdn.nlark.com/yuque/0/2018/png/100996/1539840654424-0c57efec-4c90-4994-a29b-5cf2404cbcd9.png)

#### `top`
* 类型: Boolean
* 描述: 指定 guide 是否绘制在 canvas 最上层，默认为 false, 即绘制在最下层。

#### `start`
* 类型： Object | Function | Array
* 描述：指定辅助线的起始位置，该值的类型如下：
  - object: 使用图表 x,y 对应的原始数据例如： { time: '2010-01-01', value: 200 }
  - array: 数组来配置位置 [ x, y ]，根据数组中的值的存在以下几种形式：
    - x，y 都是原始数据 [ '2010-01-01', 200 ];
    - x，y 可以使用原始数据的替代字符串 'min', 'max', 'median' , 例如：[ 'median', 200 ]
    - x, y 都是用百分比的形式，在绘图区域定位，字符串中存在 '%', 例如 [ '50%', '50%' ] 使得辅助元素居中
  - function: 回调函数，可以动态的确定辅助元素的位置，应用于数据动态更新，辅助元素的位置根据数据变化的场景
  ```js
    <Guide>
      <Line start={(xScale, yScale) => {
        return []; //位置信息
      }} />
    </Guide>
  ```

#### `end`
* 类型： Object | Function | Array
* 描述：指定辅助线的结束位置，使用同 start。

#### `content`
* 类型：  String
* 描述：辅助文本的显示内容。

#### `style`
* 类型： Object
* 描述：用于设置辅助文本的显示样式。[更详细配置](./graphic)

#### `display`
* 类型: Object
* 描述: display:{ point:true | false, line:true | false, text:true | false }， 是否显示point/line/text。

#### `lineLength`
* 类型: Number
* 描述: line的长度
* 默认值: 30

#### `regionStyle`
* 类型: Object
* 描述: area 区间样式，[更详细配置](./graphic)。

#### `lineLength`
* 类型: Number
* 描述: line的长度
* 默认值: 30

#### `direction`
* 类型: String
* 描述: 标注点朝向：'upward' | 'downward'
* 默认值: 'upward'，即向上

### 动态辅助标记
辅助标记接受的位置信息的参数都是原始数据值，辅助标记一旦生成后就是固定了位置，如果数据发生改变，辅助标记就需要删除掉重新创建。
```js
// 清除图表
chart.clear();
// 重新声明图形语法
chart.point().position('carat*price');
chart.guide().html([ newX, newY ], htmlstring);
chart.render();
```
* newX,newY 是重新计算的位置
如果数据是动态更新的那么这个过程需要频繁进行，基于这种场景 guide 提供两种计算动态位置的：
* 可以使用'min', 'median', 'max' 字符串代表原始值的最小值、平均值、最大值，例如： [0, 'min'] 表示 x 轴上数值为 0，y 轴位置在数值的最小值上；
* 表示位置的数组可以换成回调函数，函数原型： function(xScale, yScale) {return [];}
  - xScale, yScale 映射到 x 轴上的字段生成的度量，详情查看 度量, api;
  - 分类度量常用的值是 values 包含了所有的分类，连续度量常用的是 min, max
![image](https://cdn.nlark.com/yuque/0/2018/png/100996/1539840695501-71eaf8b1-ad9d-4bcc-9d84-46b2ca40671b.png)
