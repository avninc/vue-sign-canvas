# vue-signature

> Vue signature component

this component is based on Fabric, a great lib to deal with canvas

__How to install__

- npm install vue-sign-canvas

__How to use__

```js
import VueSignCanvas from 'vue-sign-canvas'
require('vue-sign-canvas/dist/vue-sign-canvas.min.css')
Vue.component('VueSignCanvas', VueSignCanvas)

const options = {
  brushColor: '#000',
  brushWidth: 10,
  shadowEnable: false,
  shadowWidth: 10
}

function saveResult (data) {

}
function clear () {

}
<div>
  <VueSignCanvas :options="options" @result="saveResult" @clear="clear" />
<div>
```
__options__

| name | type | defaultValue | description |
| :---: | :----: | :----: | :----: |
| brushColor | string | '#000' | set brush color |
| brushWidth | number | 10     | set brush width |
| width | number | window.screen.availWidth | set canvas width |
| height | number | window.screen.availWidth | set canvas height |
| shadowEnable | boolean | false | set brush shadow enable or not |
| shadowWidth | number | 10 | set shadow width
| shadowColor | string | null | set shadow color



