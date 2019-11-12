
# View

视图，由 Chart 生成和管理，拥有自己独立的数据源、坐标系和图层，用于异构数据的可视化以及图表组合，一个 Chart 由一个或者多个视图 View 组成。

Chart 是一种特殊的View，两者之间也有一定的差异：
- View 的功能 Chart 都具有
- Tooltip（提示信息）和 Legend（图例）仅在 Chart 上支持
- Chart 上可以创建 View ，但是 View 不再支持创建子 View

## 使用说明
### 父组件
[`<Chart/>`](chart)

### 子组件
[`<Coord/>`](coord) [`<Axis/>`](axis) [`<Geom/>`](geom) [`<Legend/>`](legend) [`<Tooltip/>`](tooltip)  [`<Guide/>`](guide) [`<Facet/>`](facet) [`<View/>`](view)

**实际开发过程中，很多图表都可以采用View组件来实现，包含但不限于以下几个方面：**

* 在同一个容器中出现两个或者更多不同坐标系的图表时，可以采用View组件来实现，如下图所示；[Demo](../demo/detail?id=pie-multi-level&selectedKey=饼图)
![image | center](https://img.alicdn.com/tfs/TB1GqOTa4rI8KJjy0FpXXb5hVXa-1600-856.png)

* 同一个容器中的两个图表需要采用不同的数据源时，如下图所示：[Demo](../demo/detail?id=funnel-basic&selectedKey=漏斗图)
![image | center](https://img.alicdn.com/tfs/TB1w1flbhrI8KJjy0FpXXb5hVXa-1600-856.png)


```js
// 这里只显示部分核心代码
<Chart width={600} height={400} data={data}>
<Coord type="theta" radius={0.5} />
<Geom
  type="pie"
  position={Stat.summary.percent('value')}
  color={['type', ['#4E7CCC', '#36B3C3', '#F9815C']]}
/>
<View data={data}>
  <Coord type="theta" inner={0.75} />
  <Geom
    type="pie"
    position={Stat.summary.percent('value')}
    color="name"
  />
</View>
</Chart>
```
**实际开发过程中，想要在一个图表中使用多个数据源可以用 view 实现。**
[demo](https://bizcharts.net/products/bizCharts/demo/detail?id=g2-compare-donut&selectedKey=%E6%A6%82%E8%A7%88)

## API
#### `start`
* 类型：Object
* 描述：绘制区域的起始坐标，x和y的取值范围均为0到1。
我们的起始点是从**左上角**开始的。

参见:
```js
<View start={{x:0.5, y:0.5}} />
```

#### `end`
* 类型：Object
* 描述：绘制区域的结束坐标，x和y的取值范围均为0到1。

参见:
```js
<View end={{x:0.5, y:0.5}} />
```

#### `data`
* 类型：Array | DataSet
* 描述：使用同 `Chart` 的 [data 属性](chart#data)。

#### `animate`
* 类型：boolean
* 描述：视图是否执行动画。
* 默认值:`true`。

#### `scale`
* 类型：Object
* 描述：使用同 `Chart` 的 [scale 属性](chart#scale)。

#### `filter`
* 类型：Object
* 描述：使用同 `Chart` 的 [filer 属性](chart#filter)。

## 示例
在进行地理数据的可视化的时候，使用多视图的绘制方式就会非常方便。

通常情况下，地理数据的可视化会包含多份数据：一份是用于绘制地图的经纬度数据，一份是用户真正想要可视化的用户数据。

在这个例子中，需要在地图上标注各个地区的热力值，这个时候就可以使用多视图的可视化方案：

- 首先绘制地图背景，使用包含世界地图经纬度的数据；
- 然后再可视化包含各个地区的热力数据。

[demo](https://bizcharts.net/products/bizCharts/demo/detail?id=map-with-heatmap&selectedKey=%E5%9C%B0%E5%9B%BE)
![](https://img.alicdn.com/tfs/TB17MUAmuL2gK0jSZPhXXahvXXa-942-780.jpg)

