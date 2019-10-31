# 常见问题

## 有问题怎么办

- 建议将所有问题相关的教程、api文档及相关的 demo 都看一遍，如果还有问题，欢迎提 [issue](https://github.com/alibaba/BizCharts/issues)
- issue 中请务必提供可复现的链接，方便排查问题。链接可以在 Codepen 中 fork [Bizcharts codepen template](https://codepen.io/leslie2014/pen/xjJGER) 后生成。
- 使用过程中如果遇到问题无法自行解决，可扫码加入钉钉群群求帮助

![](https://img.alicdn.com/tfs/TB1Cu79l3HqK1RjSZFkXXX.WFXa-340-339.png)


## 坐标轴空间不够

- 通过配置 `<Chart>` 组件的 [padding属性](../api/chart#padding) 调整图表的内边距给非图形区更多空间。
- 通过配置 `<Aixs>` 组件中 [content属性](../api/axis#content) 上的 formatter 或者 rotate 值，让 label 的文字占用更少的空间。

## 坐标轴label自定义

- 通过配置 `<Aixs>` 组件中 [content属性](../api/axis#content) 上的 formatter 函数控制。

## tooltip显示

- 通过 `<Geom>` 组件上的 [tooltip 属性](../api/geom#tooltip)控制 tooltip 中的显示内容
- 通过 `<Tooltip>` 组件上的 [itemTpl](../api/tooltip#itemtpl) 和 [containerTpl](../api/tooltip#containertpl) 两个属性用通过 HTML 去控制 tooltip 的显示。
- 特别复杂的场景可以通过 `<Chart>` 上的 [onTooltipChange](../api/chart#onTooltipChange)  事件来格式化 `<Tooltip />` 显示内容。

## tooltip自定义

- 通过 `<Tooltip>` 组件上的 [itemTpl](../api/tooltip#itemTpl) 和 [containerTpl](../api/tooltip#containerTpl) 两个属性用通过 HTML 去控制 tooltip 的显示。模版中的参数值'{name}' '{value}' 可以通过GEMO的[tooltip](../api/geom#tooltip)属性进行修改
```JSX
<Chart>
    <Tooltip
      containerTpl='<div class="g2-tooltip"><p class="g2-tooltip-title"></p><table class="g2-tooltip-list"></table></div>'
      itemTpl='<tr class="g2-tooltip-list-item"><td style="color:{color}">{name}</td><td>{value}</td></tr>'
      offset={50}
      g2-tooltip={{
        position: 'absolute',
        visibility: 'hidden',
        border : '1px solid #efefef',
        backgroundColor: 'white',
        color: '#000',
        opacity: '0.8',
        padding: '5px 15px',
        transition: 'top 200ms,left 200ms'
      }}
      g2-tooltip-list={{
        margin: '10px'
      }}
    />
  <Gemo tooltip={["x*y*z", (x,y,z) => {
    return { color: 'red', value: x };
  }]}
</Chart>
```

## Legend 中分类过多，导致部分内容被遮盖该怎么处理？

这种情况下可以通过自定义 Legend 展示来解决：设置 **useHtml** 属性为 true，同时设置 **g2-legend** 对 legend 的样式做调整。具体 [g2-legend]( http://www.bizcharts.net/products/bizCharts/api/legend#g2-legend--g2-legend-item--g2-legend-list-item--g2-legend-marker--g2-legend-text---object)


## 切换到其他页面再切换回的时候，图表会超出原来宽度，必须 resize 浏览器窗口才可恢复

首先推荐容器初始化时设置容器 size，如果切换页面后还有自适应问题，可以设置图表 **forcetFit**，例如：
```javascript
render() {
    if (this.chart) {
      this.chart.forceFit();
    }
    return (<Chart
      {...opts}
      onGetG2Instance={(chart) => {
        this.chart = chart;
      }}
    />);
  }
```
除此之外，还可以在组件 componentDidMount 时，主动触发 window resize 事件。


## 标题和坐标轴重合怎么解决?

可以通过设置 ```Axis``` 的 title 属性来解决，例如：
```JSON
{
  offset: {Number}, // 设置标题 title 距离坐标轴线的距离
}
```
具体 API 可参考以下：[Axis](../api/axis#api)


## 如何修改指定某个数据的颜色？

Geom 上的 **color** 属性支持对字段自定义颜色，具体 API 可参考以下：[Geometry](../api/geom#api)


## 坐标轴刻度非常密，导致部分文字被遮盖

可以通过修改坐标轴 ```Axis``` 的 **label** 属性，调整坐标轴文字的排版，具体 API 可参考：
[Axis](../api/axis#api)


## 坐标轴连续的数据集，如何做分段展示？

可以设置 Chart ```scale```  **formatter** 属性做数据格式化处理，具体 API 可参考：[Scale](../api/chart#api)


## 如何通过拿到点图中某一个点的坐标，来拿到这个点，并这个点处于hover状态？

可以通过底层的 ```geom.setShapesActived```  ，```geom.getShapes()``` 方法来实现.

## 如何让折线图中的线变得圆滑？

可以通过设置 ```<Geom shape="smooth" />``` 解决


## 图表支持框选操作与右键操作吗？

右击操作暂不支持，未来会有支持计划；框选操作可以实现，具体可以参考 Demo: [Demo](../demo/detail?id=g2-brush-interval&selectedKey=概览)


## 图表设置 padding 为 auto 后，这个 padding 是如何计算的？

目前是只会计算 **formatter** 前 label 的边框，暂时可以先手动设置 padding 的边距来解决。


## 图表如何保存为图片格式？

1. 拿到 chart 实例：

```JSX
<chart
  onGetG2Instance={chartIns => {
    chartIns.downloadImage();
  }}
/>
```


## 如何知道 canvas 渲染完成？

可以通过拿到 chart 实例，并监听 ```afterrender``` 事件实现。如何拿到 g2 chart 实例可以参考：[API](../api/chart#%E4%BA%8B%E4%BB%B6api)

## 如何获取Canvase实例

```JSX
<Chart width={600} height={400} data={data} scale={scale} onGetG2Instance={chartIns => {
  console.log(chartIns.get('canvas')); // 可以拿到g 的canvas实例，然后你可以使用g的语法来画。
  console.log(chartIns.get('canvas')._cfg.el); // 拿到Dom
}}></Chart>
```

## 为什么图表绘制区域会有留白情况？

这种情况是因为图表默认有 padding 造成的，而且 padding 不对称，可以通过调整 padding 解决。


## 如何调整柱状图间的间距？

可以通过设置 Geom 的 **marginRatio** 调柱子间距，具体 API 可参考：[API](../api/geom#api)


## 如何知道 tootip 在鼠标左边还是右边？

tooltip 的局部目前只有CSS效果，在左边右边是没有属性值区分的，要自己去写逻辑判断，比如你可以在 tooltip ```onShow``` 的回调中做判断。


## value都为0的情况下，能否显示饼图？

不能。


## 在 Facet 中，多个图的 tooltip 可以联动显示吗？

默认是不可以的。但可以自定义tooltip的内容达到同样的效果
```shared: true```

## 怎么设置tooltip init初始化时就默认选中某一个选项

```js
const data = [
  { category: 'Sports', sold: 275 },
  { category: 'Strategy', sold: -255 },
  { category: 'Action', sold: 120 },
  { category: 'Shooter', sold: 650 },
  { category: 'Other', sold: 150 }
]

// 默认选中第四项的结果
const handleAlwaysShowTooltip = ev => {
  ev.showTooltip(ev.getXY(data[3]))
}

<Chart data={data} onGetG2Instance={handleAlwaysShowTooltip} ></Chart>
```
![](https://img.alicdn.com/tfs/TB1jAs2vmcqBKNjSZFgXXX_kXXa-569-470.png)

## 图形区域太靠左或太靠右? 用scale range
![](https://img.alicdn.com/tfs/TB1chKviAL0gK0jSZFAXXcA9pXa-1988-1150.png_500x500)
![](https://img.alicdn.com/tfs/TB1PrKriqL7gK0jSZFBXXXZZpXa-692-89.png_500x500)

## 双Y轴 刻度线和轴标题 对不齐？用scale tickCount
![](https://img.alicdn.com/tfs/TB1dqWzixD1gK0jSZFKXXcJrVXa-229-477.png)

[参考case](https://bizcharts.net/products/bizCharts/demo/detail?id=g2-line-of-dashed&selectedKey=%E6%8A%98%E7%BA%BF%E5%9B%BE)

## 雷达图的label太长了，可以折行吗？用label useHtml
[label useHtml demo](../api/label#htmltemplate)

## 雷达图的文字能隐藏么？用 axis label
![](https://img.alicdn.com/tfs/TB1taOwiBv0gK0jSZKbXXbK2FXa-796-640.png_500x500)

[axis label API文档](https://bizcharts.net/products/bizCharts/api/axis#label)

## 如何判断图表绘制完成？

```jsx
chart.on('afterrender', () => {console.log('afterRender')});
```
## 使用slider时，不是用来做为日期的调节，而是用来对x轴整数的过滤，但slider显示的是小数，怎么才让显示成整数? 用scale formatter
![](https://img.alicdn.com/tfs/TB1c8myiy_1gK0jSZFqXXcpaXXa-2218-1172.png_500x500)
 [Slider scale formatter API](../api/sliderPlugin.md#scales)

## 图表展示bizcharts error 和 slider error 是什么原因？
bizchart引入了react 16.x的Error Boundary，从而保证了发生在 UI 层的错误不会连锁导致整个应用程序崩溃；未被任何异常边界捕获的异常可能会导致整个 React 组件树被卸载。Error Boundary能够捕获渲染函数、生命周期回调以及整个组件树的构造函数中抛出的异常，捕获后返回bizcharts error这样的提示。

## 实现拖动缩放交互，用zoom 或 drag 

```jsx
<Chart
    height={window.innerHeight}
    data={dv}
    padding={[60, 30, 30]}
    scale={scale}
    onGetG2Instance={g2Chart => {
      g2Chart.animate(false);
      chart = g2Chart;

      chart.interact('drag', {
        type: 'XY'
      }).interact('zoom', {
        type: 'XY'
      });

    }}
    forceFit
></Chart>
```

[体验demo](https://codepen.io/fengyue/pen/WNNwJEZ)

###  图表在display:none 切换到 display:block 没有正常渲染？用触发resize事件
原理：display:none时，size=0，此时主动触发容器的resize事件，能能获取到最新size，从而触发渲染；

![](https://img.alicdn.com/tfs/TB1IiB6jhD1gK0jSZFsXXbldVXa-1677-596.png_500x500)

### 数据中有小时，但是坐标轴只显示年月日? 用scale mask
![](https://img.alicdn.com/tfs/TB1yWgukFT7gK0jSZFpXXaTkpXa-404-100.png)
![](https://img.alicdn.com/tfs/TB1cYwAkKH2gK0jSZJnXXaT1FXa-978-151.png_500x500)

[scale mask API](https://bizcharts.net/products/bizCharts/api/scale#mask)

