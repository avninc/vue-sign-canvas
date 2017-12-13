<template>
  <div class="vueSignCanvas">
    <canvas id="c" style="width: 100%;height: 100%"></canvas>
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
      __canvas: null
    }
  },
  methods: {
    save: function () {
      this.$emit('result', this.__canvas.toDataURL('png'))
    },
    clear: function () {
      this.$emit('clear')
      this.__canvas.clear()
    },
    init: function () {
      var canvas = this.__canvas
      var newOptions = Object.assign({}, {
        width: window.screen.availWidth,
        height: window.screen.availHeight,
        brushColor: '#000',
        brushWidth: 4,
        shadowEnable: false,
        shadowWidth: 10
      }, this.options)

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
    this.__canvas = new fabric.Canvas('c', {
      isDrawingMode: true
    })
    fabric.Object.prototype.transparentCorners = false
    this.init()
  }
}
</script>

<style scoped>
.vueSignCanvas {
  width: 100%;
  height: 100%;
  position: relative;
}
.vueSignCanvas__btn {
  position: absolute;
  top: 0;
}
</style>
