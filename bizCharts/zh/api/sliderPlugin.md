![](https://img.shields.io/badge/language-react-red.svg)  ![](https://img.shields.io/badge/license-MIT-000000.svg)  [![NPM Package](https://img.shields.io/npm/v/bizcharts-plugin-slider.svg)](https://www.npmjs.com/package/bizcharts)[![NPM Downloads](https://img.shields.io/npm/dm/bizcharts-plugin-slider.svg)](https://npmjs.org/package/bizcharts)

# bizcharts-plugin-slider

A datazoom slider plugin for BizCharts base g2-plugin-slider.

## Installation

Please make sure BizCharts has been already loaded.

### npm
```sh
$ npm install bizcharts-plugin-slider
```

### html
```html
<script src=https://unpkg.com/bizcharts-plugin-slider@2.0.0/umd/bizcharts-plugin-slider.js"> </script>
```

### dev build
```sh
$ git clone https://github.com/alibaba/BizCharts.git
$ cd BizCharts
$ cd /plugin/slider
$ npm install
$ npm run build
```

### dev demo

```sh
slider $ sudo vi /etc/hosts
// add 127.0.0.1 localhost
slider $ npm run demo
// open in browser http://localhost:3510/
```

## Usage

[see demo](../demo/detail?id=g2-area-large&selectedKey=概览)

### Create an instance

```js
<Slider
  width={{number} | {string}}
  height={number}
  padding={{object} | {number} | {array}}
  xAxis={string}
  yAxis={string}
  start={{string} | {number}}
  end={{string} | {number}}
  data={{array} | {dataview}}
  fillerStyle={object}
  backgroundStyle={object}
  textStyle={object}
  handleStyle={object}
  backgroundChart={object}
/>
```
## Api

#### `width`
* 类型：number | string
* 描述：Set the width of the `slider` component, the default is `auto`, indicating the width of the adaptive container.

#### `height`
* 类型：number
* 描述：Set the height of the `slider` component, the default is 26, the unit is '`px`'.

#### `padding`
* 类型：Array
* 描述：Sets the padding canvas's canvas's padding to align with the chart (the default chart's canvas container is padded with padding). The default is the same padding as `BizCharts` default theme, `[20, 20, 95, 80]`.

#### `xAxis`
* 类型：string
* 描述：**Must declare** Slider is a slider component with a background graph that is used to declare the horizontal axis mapping field of the background chart, which is also the data filtering field.

#### `yAxis`
* 类型：string
* 描述：**Must declare** Slider is a slider component with a background graph that is used to declare the vertical axis of the background graph.

#### `data`
* 类型：	array | dataview
* 描述：**Must declare**，data source.

#### `start`
* 类型：number | string
* 描述：The value of the slider that declares the position of the slider at the beginning of the corresponding data value, the default is the minimum value.

#### `end`
* 类型：number | string
* 描述：The data value corresponding to the position where the slider finishes the slider is declared, and the default is the maximum value.

#### `scales`
* 类型：object
* 描述：Used to define the columns for the `xAxis` and `yAxis` fields for the same column definitions in the action's chart.

Sample code:

  ```js
  <Slider
      scales={{
        [`${xAxis}`]: {
          type: 'time',
          mask: 'MM-DD'
        }
      }}
  />
  ```

* `fillerStyle`		object
* 类型：number | string
* 描述：

The selected area of the style configuration, the default configuration is as follows:

  ```js
  <Slider
    fillerStyle={{
      fill: '#BDCCED',
      fillOpacity: 0.3
    }}
  />
  ```

Red box in the picture selected area: <img src="https://gw.alipayobjects.com/zos/rmsportal/iYFxRgDjRSiCyVPFozik.png" style="width: 59%;">

#### `backgroundStyle`
* 类型：object
* 描述：slider background style.

#### `textStyle`
* 类型：	object
* 描述：slider auxiliary text font style configuration.

#### `handleStyle`
* 类型：	object
* 描述：The slider style configuration, configurable properties are as follows:

```js
  <Slider
  handleStyle={{
    img: 'https://gw.alipayobjects.com/zos/rmsportal/QXtfhORGlDuRvLXFzpsQ.png', // Can make the picture address can also be data urls
    width: 5,
    height: 26
  }}
/>
```

#### `backgroundChart`
* 类型：object
* 描述：The slider's background chart configuration allows you to configure its chart type and color:

```js
<Slider
  backgroundChart={{
  type: [ 'area' ], // The type of chart, either a string or an array
  color: '#CCD6EC'
  }}
/>
```

#### `onChange`
* 类型：function
* 描述：When the slider slider changes, trigger the callback function, mainly used to update the state of `ds`. The callback function provides a parameter, which is an object that contains the following properties:

```js
<Slider
onChange = {(obj) => {
  const { startValue, endValue, startText, endText } = obj;
}}
/>


-  `startValue` The current raw data value corresponding to the start slider, if the type is` time` or `timeCat`, the value is timestamp, please note.
-  `endValue` The current corresponding raw data value of the end slider, if the type is` time` or `timeCat`, the value is timestamp, please note.
-  `startText` Start slider current display text value
-  `endText` The current display text value of the end slider

```

> NOTE: The reason for distinguishing text from value is that users will format numbers in most cases. Therefore, when setting the state quantity and updating the state quantity, you need to ensure that the value types are the same before and after.