
# BizCharts

## 特性
- 是基于 G2 封装的 React 图表库，具有 G2、React 的全部优点，可以让用户以组件的形式组合出无数种图表
- 集成了大量的统计工具，支持多种坐标系绘制，交互定制，动画定制以及图形定制等
- 性能稳定且具有强大的扩展能力和高度自定义能力

## 如何获取 BizCharts

你可以通过以下几种方式获取 BizCharts：
1. 在 BizCharts 的 GitHub 上下载最新的 release 版本 https://github.com/alibaba/BizCharts.git
2. 通过 npm 获取 BizCharts，我们提供了 BizCharts npm 包，通过下面的命令即可完成安装

```bash
npm install bizcharts --save
```

成功安装完成之后，即可使用 import 或 require 进行引用。

## 绘制一个简单的图表

在 BizCharts 引入页面后，我们就已经做好了创建第一个图表的准备了。

下面是以一个基础的柱状图为例开始我们的第一个图表创建。

1. 创建容器

	在页面的 *body* 部分创建一个节点，指定一个 id

	```html
	<div id="mountNode"></div>
	```

2. 使用组件生成图表

	- 引入图表需要的组件
	- 用组件组装成需要的图表
	- 把图表渲染到 mountNode 节点上

```js
import React from 'react';
import ReactDOM from 'react-dom';
import { Chart, Geom, Axis, Tooltip, Legend, Coord } from 'bizcharts';

// 数据源
const data = [
  { genre: 'Sports', sold: 275, income: 2300 },
  { genre: 'Strategy', sold: 115, income: 667 },
  { genre: 'Action', sold: 120, income: 982 },
  { genre: 'Shooter', sold: 350, income: 5271 },
  { genre: 'Other', sold: 150, income: 3710 }
];

// 定义度量
const cols = {
  sold: { alias: '销售量' },
  genre: { alias: '游戏种类' }
};

// 渲染图表
ReactDOM.render((
  <Chart width={600} height={400} data={data} scale={cols}>
      <Axis name="genre" title/>
      <Axis name="sold" title/>
      <Legend position="top" dy={-20} />
      <Tooltip />
      <Geom type="interval" position="genre*sold" color="genre" />
    </Chart>
), document.getElementById('mountNode'));

```

一张柱状图就绘制成功了：

![](https://img.alicdn.com/tps/TB1PVaoPFXXXXcSaXXXXXXXXXXX-519-401.png)

## 其他配置
### 获取chart实例
```js
class Line extends React.Component {
  render() {
    let chartIns; //初始化实例
    ...
    return (
      <div>
        <Chart 
	  height={400} 
	  data={data}
	  forceFit
          onGetG2Instance={chart => {
	    //获取 chart 实例的回调。每当生成一个新 chart 时就会调用该函数，并以新生成的 chart 作为回调参数。
            chartIns=chart; 
          }}
	  onPlotMove={ev => {
	    var point = {
	      x: ev.x,
	      y: ev.y
	    };
	    // 调用实例上的方法
	    var items = chartIns.getTooltipItems(point);
	    console.log(items);
	  }}
          >
        ...
        </Chart>
      </div>
    );
  }
}

ReactDOM.render(<Basiccolumn />, mountNode)

```

## Dependencies
```json
{
  "peerDependencies": {
    "react": "^15.0.0 || ^16.0.0"
  },
  "dependencies": {
    "@antv/g2": "3.2.2",
    "invariant": "^2.2.2",
    "warning": "^3.0.0",
    "prop-types": "^15.6.0",
  },
}
```

## Browser Support Versions
支持Chrome，Safari，IE11+ 浏览器

## 体验改进计划说明
G2 decided to terminate the "Experience Improvement Program". In verson @antv/g2@3.4.7（released at 2018.12.26） and above, all tracking code is removed, no unexpected remote request will be sent while you are using G2. And Bizcharts Upgrade the dependent version the first time at 2018.12.26 24:00.
