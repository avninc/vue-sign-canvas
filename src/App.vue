<template>
  <div class="vueSignCanvas">
    <canvas id="vueSignCanvas-c" style="width: 100%;height: 100%"></canvas>
    <div class="vueSignCanvas__btn">
      <button @click="clear">清空</button>
      <button @click="save">保存</button>
    </div>
  </div>
</template>

<script>
import { fabric } from 'fabric'
import Color from 'color'
export default {
  name: 'vueSignCanvas',
  props: ['options'],
  data: () => {
    return {
      __canvas: null,
      defaultOptions: {
        width: window.screen.availWidth,
        height: window.screen.availHeight,
        canvasBackgroundColor: '#fff',
        brushColor: '#000',
        brushWidth: 4,
        shadowEnable: false,
        shadowWidth: 10
      }
    }
  },
  methods: {
    save: function () {
      this.$emit('result', this.__canvas.toDataURL('png'))
    },
    clear: function () {
      const newOptions = Object.assign({}, this.defaultOptions, this.options)
      this.$emit('clear')
      this.__canvas.clear()
      this.__canvas.setBackgroundColor(newOptions.canvasBackgroundColor)
    },
    init: function () {
      var canvas = this.__canvas
      var newOptions = Object.assign({}, this.defaultOptions, this.options)

      canvas.setBackgroundColor(newOptions.canvasBackgroundColor)
      canvas.setWidth(newOptions.width)
      canvas.setHeight(newOptions.height)

      if (canvas.freeDrawingBrush) {
        canvas.freeDrawingBrush.color = newOptions.brushColor
        canvas.freeDrawingBrush.width = newOptions.brushWidth
        if (newOptions.shadowEnable) {
          canvas.freeDrawingBrush.shadow = new fabric.Shadow({
            blur: parseInt(newOptions.shadowWidth, 10) || 0,
            offsetX: 0,
            offsetY: 0,
            affectStroke: true,
            color: newOptions.shadowColor ? newOptions.shadowColor : Color(newOptions.brushColor).fade(0.5).hsl().string()
          })
        }
      }
    }
  },
  watch: {
    'options': function (val) {
      this.init()
    }
  },
  mounted() {
    this.__canvas = new fabric.Canvas('vueSignCanvas-c', {
      isDrawingMode: true
    })
    fabric.Object.prototype.transparentCorners = false
    this.init()
  }
}
</script>

<style scoped >
.vueSignCanvas {
  width: 100%;
  height: 100%;
  position: relative;
}
.vueSignCanvas__btn {
  position: absolute;
  top: 10px;
  left: 10px;
}
.vueSignCanvas__btn button {
  padding: 6px 16px;
  background: #fff;
  border: 1px solid #ececec;
  border-radius: 3px;
  margin-right: 14px;
}
@media screen and (max-width: 414px) {
  .vueSignCanvas__btn {
    position: fixed;
    bottom: 0;
    top: auto;
    left: 0;
    right: 0;
    display: flex;
  }
  .vueSignCanvas__btn button {
    flex-grow: 1;
    margin-right: 0;
    margin-left: 0;
    border-right: 0;
    border-left: 0;
    border-bottom: 0;
    border-radius: 0;
    padding: 16px;
    font-size: 16px;
  }
  .vueSignCanvas__btn button:first-child {
    border-right: 1px solid #ececec;
  }
}
</style>
