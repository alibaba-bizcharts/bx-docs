
# Label

[`<Geom>`](geom) 几何标记上的标注文本组件。

## 父组件
[`<Geom />`](geom)

## 子组件
无

## API
#### `content`
* 类型：String | Array:[String, Function]
* 描述：指定 label 上显示的文本内容，可以是数据纬度，也可以自定义。

使用示例:
```js
<Label content="常量字符串" />
// 使用数据
<Label content="sales*date"/>
// 使用回调函数
<Label content={["sales*date", (sales, date)=>{
    return `${data}:${sales}`;
  }]}
/>
```

#### `labelLine`
* 类型：Object
* 描述：文本距离几何线的配置，如果值为`false`，表示不展示文本线。默认不展示。

使用示例:
```js
<Label
  content="some label"
  labelLine={{
    lineWidth: 1, // 线的粗细
    stroke: '#ff8800', // 线的颜色
    lineDash: [ 2, 1 ], // 虚线样式
  }}
/>
```

#### `offset`
* 类型：Number
* 描述：设置文本距离几何图形的的距离

#### `textStyle`
* 类型：Object
* 描述：文本的图形样式。其他样式请参考[绘图属性](./graphic)
```js
<Label
  content='sales'
  textStyle={{
    textAlign: 'start', // 文本对齐方向，可取值为： start middle end
    fill: '#404040', // 文本的颜色
    fontSize: '12', // 文本大小
    fontWeight: 'bold', // 文本粗细
    rotate: 30,
    textBaseline: 'top' // 文本基准线，可取 top middle bottom，默认为middle
  }}
/>
```

样式值支持回调：
```js
<Label
  content='sales'
  textStyle={sales=>{
    const style = {textAlign: 'center'};
    if(if(sales > 1000)) {
      style.fill = '#ff0000';
    } else {
      style.fill = '#00ff00';
    }
    return style;
  }}
/>
```

#### `autoRotate`
* 类型：Boolean
* 描述：是否需要自动旋转，默认值：`true`。

#### `formatter`
* 类型：Function
* 描述：用于格式化坐标轴上显示的文本信息。
```js
<Label
  content='name'
  formatter={(text, item, index)=>{
    // text 为每条记录 x 属性的值
    // item 为映射后的每条数据记录，是一个对象，可以从里面获取你想要的数据信息
    // index 为每条记录的索引
	var point = item.point; // 每个弧度对应的点
	var percent = point['percent'];
	percent = (percent * 100).toFixed(2) + '%';
	return name + ' ' + percent;
  }}
/>
```

#### `htmlTemplate`
* 类型：Function
* 描述：自定义 html 文本
```js
<Label
  content='name'
  htmlTemplate={(text, item, index)=>{
    // text 为每条记录 x 属性的值
    // item 为映射后的每条数据记录，是一个对象，可以从里面获取你想要的数据信息
    // index 为每条记录的索引
	var point = item.point; // 每个弧度对应的点
	var percent = point['percent'];
	percent = (percent * 100).toFixed(2) + '%';
	// 自定义 html 模板
	return '<span class="title" style="display: inline-block;width: 50px;">' + text + '</span><br><span style="color:' + point.color + '">' + percent + '</span>';
  }
/>
```
