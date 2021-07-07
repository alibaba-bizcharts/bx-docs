# Shape

在 `<Geom shape={shapeType} />` 中指定几何图形时，可以使用[内置的 shape](../docs/chartType#图表类型和几何标记)，也可以通过 `Shape` 来自定义 shape。

## 自定义语法

自定义 `shape` 的使用如下：

```js
import { Shape } from 'bizcharts';
//往 interval 几何标记对象（决定了图表类型，即柱状图、饼图等）上注册名字为 shapeName 的 Shape
const shapeObj = Shape.registerShape(geomName, 'shapeName', {
  getPoints: function(pointInfo) {
    // 获取 shape 绘制的关键点
  },
  draw: function(cfg, container) {
    // 自定义最终绘制的逻辑
  }
});

ReactDOM.render(<Chart><Geom type='interval' shape='shapeName' /></Chart> , container);
```

  参数详解：
  - geomName 几何标记名, 如 point, line 等
  - shapeName 注册的具体图形名，自定义的图形的名称
  - getPoints 自定义形状绘制时需要的节点，比如柱状图需要 4 个节点
  - draw 执行图形绘制逻辑、调用绘图引擎


## 自定义语法配置

### getPoints

计算绘制 shape 的关键点，每个几何形状都是由特定的几个关键点通过线连接而成。

#### 参数

- `pointInfo`       **Object**
    pointInfo 数据结构如下，所有的数值都是归一化后的结果（即 0 至 1 范围内的数据）

    ```js
    {
        size: 0.1,  // 形状的尺寸，不同的 shape 该含义不同，0 - 1 范围的数据
        x: 0.2,     // 该点归一化后的 x 坐标
        y: 0.13,    // 该点归一化后的 y 坐标
        y0: 0.1     // 整个数据集 y 轴对应数据的最小值，也是归一化后的数据，注意如果 y 对应的源数据是数组则 y 也将是个数组
    }
    ```

#### 返回值

返回包含绘制关键点的数组，即 `Array<{x: number, y: number}>`。

下表列出了 G2 各个 geom 几何形状的关键点形成机制：

geom 类型 | 解释
---- | ----
point | 点的绘制很简单，只要获取它的坐标以及大小即可，其中的 `size` 属性代表的是点的半径。<br>![image](https://zos.alipayobjects.com/skylark/940c75cf-8400-415a-9e2d-040ce46e6a03/attach/3378/269e0e2c77a555a5/image.png)
line | 线是由无数个点组成，在 G2 中我们将参与绘制的各个数据转换成坐标上的点然后通过线将逐个点连接而成形成线图，其中的 `size` 属性代表的是线的粗细。<br>![image](https://zos.alipayobjects.com/skylark/f9b84b83-1cc8-4b81-9319-f643ef0e280a/attach/3378/d49e02be2f48a136/image.png)
area | area 面是在 line 线的基础之上形成的, 它将折线图中折线与自变量坐标轴之间的区域使用颜色或者纹理填充。<br>![image](https://zos.alipayobjects.com/skylark/dbcd60f3-7662-4ebd-8e0e-85d7d754d0c7/attach/3378/f67277978d5d8e3e/image.png)
interval | interval 默认的图形形状是矩形，而矩形实际是由四个点组成的，在 G2 中我们根据 `pointInfo` 中的 `x`、`y`、`size` 以及 `y0` 这四个值来计算出这四个点，然后**顺时针**连接而成。<br>![image](https://zos.alipayobjects.com/skylark/f36a2e27-13e8-4d55-8c93-b698e15bcc1f/attach/3378/94a6515e2eb60265/image.png)
polygon | polygon 多边形也是由多个点连接而成，在 `pointInfo` 中 `x` 和 `y` 都是数组结构。<br>![image](https://zos.alipayobjects.com/skylark/b4f6981c-ccd3-4237-97bd-dd88950758ea/attach/3378/ed2b5c05a1ff3581/image.png)
schema | schema 作为一种自定义的几何图形，在 G2 中默认提供了 `box` 和 `candle` 两种 shape，分别用于绘制箱型图和股票图，注意这两种形状的矩形部分四个点的连接顺序都是顺时针，并且起始点均为左下角，这样就可以无缝转换至极坐标。<br>![image](https://zos.alipayobjects.com/skylark/340c229d-be30-4f98-8a2a-8d55c8422645/attach/3378/1bfed6f3f5f90e13/image.png)![image](https://zos.alipayobjects.com/skylark/8afa13da-95d1-4282-a08b-f1c421b0d972/attach/3378/d82c45d3a526bd80/image.png)
edge | edge 边同 line 线一致，区别就是 edge 是一个线段，连接边的两个端点即可。

### draw

`getPoints` 用于计算绘制 shape 的关键点，那么 `draw` 方法就是用来定义如何连接这些关键点的。

#### 参数

- `cfg`     **object**

该参数包含经过图形映射后的所有数据以及该数据对应的原始数据，结构如下图所示：

![image](https://zos.alipayobjects.com/skylark/505c6cb1-fde7-4714-98b6-43cb77099f19/attach/3378/332f7e3e64bc48f5/image.png)

原始数据存储于 `cfg.origin._origin` 中，通过 `getPoints` 计算出的图形关键点都储存于 `points` 中。而 `cfg` 对象中的 `color`、`size`、`shape` 都是通过映射之后的图形属性数据，可以直接使用。

- `container`       **G2.G.Group**

图形容器，需要将自定义的 shape 加入该容器中才能最终渲染出来。

#### 返回值

返回创建好的 shape，即 `container.add(geomType, shapAttrs)` 的返回值。

## Shape 方法

自定义 shape 时，可使用内置的工具方法来快速将归一化后的数据转换为画布上的坐标，如下面示例使用了 `parsePath` 方法：

```js
Shape.registerShape('interval', 'rect', {
  getPoints(pointInfo) {
    // ...
  },
  draw(cfg, container) {
    // ...
    path = this.parsePath(path);
    // ...
  }
});
```

### parsePoint
说明：将 0 - 1 范围内的点转化为画布上的实际坐标。
#### 参数
- `point`       **object{x: number, y: number}**

如：

```js
{
  x: 0.3,
  y: 0.34
}
```
#### 返回值
返回画布上的坐标，数据结构同参数 `point`。

### parsePoints
说明：将一组 0 - 1 范围内的点转化为画布上的实际坐标。
#### 参数
- `points`       **Array<{x: number, y: number}>**

`point` 的数组，表示一组归一后的坐标。

如：
```js
[
  { x: 0.3, y: 0.34 },
  { x: 0.3, y: 0.34 }
]
```
#### 返回值
返回对应的画布上的坐标点数组，数据结构同 `points`。

### parsePath
将归一化的 path 转为基于画布坐标的 path.

#### 参数
- `path`        **Path**

连接各个关键的路径，例如：`[[ 'M', 0, 0 ], [ 'L', 1, 1 ]]`。
- `isCircle`        **boolean**

是否是极坐标。如果是极坐标，则把 path 转换为圆弧。
#### 返回值     **Path**
返回基于画布坐标的 path。

## 代码示例

下面通过一个例子来加深下理解。

<div id="c1"></div>

```js
import { Chart, Axis, Geom, Tooltip, Shape } from 'bizcharts';
// 如果是 umd 引入，可使用下面的方式
// const { Chart, Axis, Geom, Tooltip, Shape } = window.BizCharts;

Shape.registerShape('interval', 'triangle', {
  getPoints(cfg) {
    const x = cfg.x;
    const y = cfg.y;
    const y0 = cfg.y0;
    const width = cfg.size;
    return [
      { x: x - width / 2, y: y0 },
      { x: x, y: y },
      { x: x + width / 2, y: y0 }
    ]
  },
  draw(cfg, container) {
    const points = this.parsePoints(cfg.points); // 将0-1空间的坐标转换为画布坐标
    const polygon = container.addShape('polygon', {
      attrs: {
        points: [
          [ points[0].x, points[0].y ],
          [ points[1].x, points[1].y ],
          [ points[2].x, points[2].y ]
        ],
        fill: cfg.color
      }
    });
    return polygon; // 将自定义Shape返回
  }
});

const data = [
  { genre: 'Sports', sold: 275 },
  { genre: 'Strategy', sold: 115 },
  { genre: 'Action', sold: 120 },
  { genre: 'Shooter', sold: 350 },
  { genre: 'Other', sold: 150 }
];

ReactDOM.render((
  <div>
    <Chart height={400} data={data} forceFit>
      <Axis name="genre" />
      <Axis name="sold" />

      <Geom type="interval" position="genre*sold" color="genre" shape="triangle" />
    </Chart>
  </div>
), document.getElementById("mountNode"));
```

![custom shape](https://img.alicdn.com/tfs/TB1INKnzrSYBuNjSspfXXcZCpXa-1458-692.png)
