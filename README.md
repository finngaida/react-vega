<!--**Introduction**-->
<!--| [API Reference](https://github.com/kristw/react-vega/blob/master/docs/api.md)-->
<!--| [Demo](https://kristw.github.io/react-vega)-->

# react-vega [![NPM version][npm-image]][npm-url] [![Dependency Status][daviddm-image]][daviddm-url]

<!--[![Build Status][travis-image]][travis-url]-->

Convert Vega spec into React class conveniently, inspired by this [tutorial](https://medium.com/@pbesh/react-and-vega-an-alternative-visualization-example-cd76e07dc1cd#.omslw1xy8) by @pbeshai

Examples

- https://jsfiddle.net/kristw/htg4uron/
- https://jsfiddle.net/kristw/qr8a1v8d/

## Install

```bash
# via npm
npm install react-vega --save
# or via bower
bower install react-vega --save
```

## Example Usage

There are two approaches to use this libary.

### Approach#1 Create class from spec, then get a React class to use

#### BarChart.js

See the rest of the spec in [barChart.json](examples/barChart.json).

```javascript
import React, { PropTypes } from 'react';
import {createClassFromSpec} from 'react-vega';

export default createClassFromSpec('BarChart', {
  "width": 400,
  "height": 200,
  "padding": {"top": 10, "left": 30, "bottom": 30, "right": 10},
  "data": [{ "name": "table" }],
  "signals": [
    {
      "name": "hover", "init": null,
      "streams": [
        {"type": "@bar:mouseover", "expr": "datum"},
        {"type": "@bar:mouseout", "expr": "null"}
      ]
    }
  ],
  ... // See the rest in barChart.json
});
```

#### main.js

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import BarChart from './BarChart.js';

const barData = {
  table: [
    {"x": 1,  "y": 28}, {"x": 2,  "y": 55},
    {"x": 3,  "y": 43}, {"x": 4,  "y": 91},
    {"x": 5,  "y": 81}, {"x": 6,  "y": 53},
    ...
  ]
};

function handleHover(...args){
  console.log(args);
}

ReactDOM.render(
  <BarChart data={barData} onSignalHover={handleHover}/>,
  document.getElementById('bar-container')
);
```

### Approach#2 Use `<Vega>` generic class and pass in `spec` for dynamic component.

Provides a bit more flexibility, but at the cost of extra checks for spec changes.

#### main.js

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import Vega from 'react-vega';

const spec = {
  "width": 400,
  "height": 200,
  "padding": {"top": 10, "left": 30, "bottom": 30, "right": 10},
  "data": [{ "name": "table" }],
  "signals": [
    {
      "name": "hover", "init": null,
      "streams": [
        {"type": "@bar:mouseover", "expr": "datum"},
        {"type": "@bar:mouseout", "expr": "null"}
      ]
    }
  ],
  ... // See the rest in barChart.json
}

const barData = {
  table: [
    {"x": 1,  "y": 28}, {"x": 2,  "y": 55},
    {"x": 3,  "y": 43}, {"x": 4,  "y": 91},
    {"x": 5,  "y": 81}, {"x": 6,  "y": 53},
    ...
  ]
};

function handleHover(...args){
  console.log(args);
}

ReactDOM.render(
  <Vega spec={spec} data={barData} onSignalHover={handleHover}/>,
  document.getElementById('bar-container')
);
```

### Props

React class `Vega` and any output from `createClassFromSpec` have these properties

These properties corresponds to [Vega's View Component API](https://github.com/vega/vega/wiki/Runtime#view-component-api)

- width
- height
- padding
- viewport
- renderer

However, for `data`. There is a slight different. As it takes an Object with keys being dataset names defined in the spec's data field.

- data

Any signal defined in the spec can be listened to via these listeners.

- onSignal[...] - Include all signals defined in the spec automatically.

For example, to listen to signal `hover`, add handler for onSignal+capitalize(`hover`)

```javascript
 <Vega spec={spec} data={barData} onSignalHover={handleHover}/>
```

## License

© 2016 [Krist Wongsuphasawat](http://kristw.yellowpigz.com)  ([@kristw](https://twitter.com/kristw)) Apache-2.0 License

[npm-image]: https://badge.fury.io/js/react-vega.svg
[npm-url]: https://npmjs.org/package/react-vega
[travis-image]: https://travis-ci.org/kristw/react-vega.svg?branch=master
[travis-url]: https://travis-ci.org/kristw/react-vega
[daviddm-image]: https://david-dm.org/kristw/react-vega.svg?theme=shields.io
[daviddm-url]: https://david-dm.org/kristw/react-vega