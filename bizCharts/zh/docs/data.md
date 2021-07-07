
# 源数据的处理

*文档转自G2*

## 如何装载数据
Bizcharts 支持两种数据载入的方式：
* 方式 1：data 属性传入
```js
var data = [
  {"gender":"男","count":40},
  {"gender":"女","count":30}
];
 <Chart height={400} data={data} forceFit />
```
* 方式 2：调用 chart.source(data) 方法，每个字段的列定义也可以在这里传入
```js
var data = [
  {"gender":"男","count":40},
  {"gender":"女","count":30}
];
 <Chart 
    height={400} 
    scale={cols} 
    onGetG2Instance={g2Chart => {
      g2Chart.source(data, {
        x: {
          type: 'cat'
        },
        y: {
          min: 0
        }
      })
    }}
    forceFit
/>
```

## 支持的数据格式
**BizCharts 支持两种格式的源数据**
- JSON数组
- DataView 对象

### JSON 数组
Example:

```js
var data = [
  {"gender":"男","count":40},
  {"gender":"女","count":30}
];
```

### DataView 对象
* 单独使用 DataView
    *  如果仅仅是对数据进行加工，不需要图表联动

*  通过状态量实现图表联动
在G2 3.0 中使用 DataSet 的状态量(State) 可以很容易的实现图表的联动，步骤如下：
    1. 创建 DataSet 对象，指定状态量
    2. 创建 DataView 对象，在 transform 中使用状态量
    3. 创建图表，引用前面创建 DataView
    4. 改变状态量，所有 DataView 更新

## 更新数据
Bizcharts更新数据的方式主要有三种：
* 仅仅是更新图表的数据
* 清理所有，重新绘制
* 使用 DataView 时的更新

### 更新数据
如果需要马上更新图表，使用 `chart.changeData(data)` 即可
```js
chart.changeData(newData);
// view 也支持 view.changeData(data)
```
如果仅仅是更新数据，而不需要马上更新图表，可以调用 `chart.source(data)`，需要更新图表时调用 `chart.repaint()`
```js
chart.source(newData);
chart.guide().clear();// 清理guide
chart.repaint();
```
### 清理图形语法
更新数据时还可以清除图表上的所有元素，重新定义图形语法，重新绘制
```js
chart.line().position('x*y');
chart.render();
chart.clear(); // 清理所有
chart.source(newData); // 重新加载数据
chart.interval().position('x*y').color('z');
chart.render();
```

### 使用 DataView  更新
由于 `DataSet` 支持状态量 `state`，一旦更改状态量，图表即一起刷新，详见 [DataSet Package](./dataset)。
