
# BizCharts

> 如果是 umd 文件引入， BizCharts = Window.BizCharts;
> 如果是 package 引入， BizCharts 为: import * as BizCharts from "bizcharts";

全局命名空间 BizCharts 包含：

## 组件

#### Chart
图表最顶级的组件，控制着图表的创建、绘制、销毁等。
详细文档见 [Chart api](chart)

#### Coord
坐标系组件，设置 Chart 或者 View 的坐标系类型。
详细文档见 [Coord api](coord)

#### Axis
坐标轴组件，控制图表中坐标轴的样式等。
详细文档见 [Axis api](axis)

#### Geom
几何标记对象，决定创建图表的类型。
详细文档见 [Geom api](geom)。

#### Label
几何标记对象上的文本。
详细文档见 [Label api](label)

#### Legend
图例。
详细文档见 [Legend api](legend)

#### Guide
坐标轴组件，控制图表中坐标轴的样式等。
详细文档见 [Guide api](guide)

#### Facet
控制 Chart 分面。
详细文档见 [Facet api](facet)

#### View
视图组件。
详细文档见 [View api](view)

## G2
G2 的命名空间，在有需要的情况下用户可以直接拿到该对象去工作。

## Animate
用来注册用户自定义动画。
详细文档见 [动画教程](../docs/animate)。

## Shape
构成图表具体的形状类。
详细文档见 [Shape api](shape)。

## setTheme
设置 BizCharts 中使用的主题类型。
详细文档见 [theme tutorial](../docs/theme)。

## track
该方法用于 G2 情况的打点监控，默认处于开启状态，如果您不想让我们知道您的版本使用情况，请配置为 false。
```js
BizCharts.track(false);
```

## DomUtil
用于操作 dom 相关的工具类。具体该工具类包含的方法如下：

| 方法                                            | 参数说明                                                                                 | 返回结果                                                                      |
| ----------------------------------------------- | ---------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------- |
| `getBoundingClientRect(node)`                   | `node`:HTMLElement，dom 节点                                                             | 返回该节点在页面中的位置，返回结果格式为： `{top: , bottom: , left: , right}` |
| `getStyle(dom, name)`                           | `dom`:HTMLElement，DOM 节点；`name`:String，样式名                                       | 返回该节点对应样式名 name 的具体样式。                                        |
| `modifyCSS(dom, css)`                           | `dom`:HTMLElement，DOM 节点；`css`:Object，样式属性                                      | 修改对应节点的 css 样式，返回修改样式后的 dom 对象。                          |
| `createDom(str)`                                | `str`:String，Dom 字符串                                                                 | 按照传入的 str 创建 dom 节点，并返回创建的节点。                              |
| `getRatio()`                                    | --                                                                                       | 返回屏幕的像素分辨率。                                                        |
| `getWidth(el)`                                  | `el`:HTMLElement，dom 节点                                                               | 返回 dom 节点的宽度，不包含 padding、border                                   |
| `getHeight(el)`                                 | `e`l:HTMLElement，dom 节点                                                               | 返回 dom 节点的高度，不包含 padding、border                                   |
| `getOuterWidth(el)`                             | `el`:HTMLElement，dom 节点                                                               | 返回 dom 节点的宽度，包含 padding、border                                     |
| `getOuterHeight(el)`                            | `el`:HTMLElement，dom 节点                                                               | 返回 dom 节点的高度，包含 padding、border                                     |
| `addEventListener(target, eventType, callback)` | `target`:HTMLElement，DOM对象；`eventType`:String，事件名；`callback`:Function，回调函数 | 添加事件监听器                                                                |
| `requestAnimationFrame(fn)`                     | `fn`:Function，回调函数                                                                  | 用于定时循环操作。                                                            |

## MatrixUtil
来自 G2，用于操作矩阵、向量的工具类。该工具类提供了操作三阶矩阵、二维向量和三维向量的方法，这些方法直接使用了 glMatrix 库，并且在其基础上添加了一些额外的遍历方法，具体如下代码：
1. G2.MatrixUtil.mat3: 三阶矩阵，详见 http://glmatrix.net/docs/module-mat3.html；
2. G2.MatrixUtil.vec2: 二维向量，详见 http://glmatrix.net/docs/module-vec2.html；
3. G2.MatrixUtil.vec3: 三维向量，详见 http://glmatrix.net/docs/module-vec3.html；
4. G2.MatrixUtil.transform(m, ts): 对三阶矩阵参数 m 按照 ts 进行变换，变换包含 t: translate，s: scale，r: rotate，m: multiply，具体使用如下：
```js
import {Util} from 'bizcharts';
Util.MatrixUtil.transform([ 1, 0, 0, 0, 1, 0, 0, 0, 1 ], [
  [ 'r', Math.PI / 2 ], 
  [ 't', 10, 10 ], 
  [ 'r', -1 * Math.PI / 2 ]
]);
```

## PathUtil
来自 G2，用于操作图形路径的工具类。具体提供的方法如下：

| 方法                                             | 参数说明                                                                                 | 返回结果                                                                      |
| ----------------------------------------------- | ---------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------- |
| `parsePathString(pathString)`                   | `pathString`:String，字符串格式的路径，如 'M 10,39 L 20,50'                                                             | 将字符串格式的路径转换为数组格式，[ [ 'M', 10, 39 ], [ 'L', 20, 50 ] ] |
| `parsePathArray(pathArray)`                     | `pathArray`:Array，数组格式的路径，如 [ [ 'M', 10, 39 ], [ 'L', 20, 50 ] ]                                    | 将数组格式的路径转化为字符串，'M 10,39 L 20,50'                                       |
| `pathTocurve(path)`                             | `dom`:Array，数组格式的路径                                       | 路径转曲                          |
| `pathToAbsolute(path)`                          | `str`:Array，数组格式的路径                                                               | 将所有的路径命令转换为绝对定位。                              |
| `catmullRomToBezier(pointsArray)`               | `str`:Array，点的数组，如 [ 10, 12, 22, 1, ... ]                                                                                 | 将传入的点（至少四组点）转曲。                                                        |
| `intersection(path1, path2)`                    | `path1`:Array，数组格式的路径；`path2`:Array，数组格式的路径；                                                               | 两条路径差值计算。                                   |

## Util
默认提供的常见的工具类，大部分基于 lodash 封装。
如下：
```js
const Util = {
  each: require('lodash/each'),
  map: require('lodash/map'),
  isObject: require('lodash/isObject'),
  isNumber: require('lodash/isNumber'),
  isString: require('lodash/isString'),
  isFunction: require('lodash/isFunction'),
  ...
};
```

#### [绘图属性](./graphic)


