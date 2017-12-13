<template>
  <div>
    <App :options="options"></App>
    <button class="btn" @click="changeColor">随机更换颜色</button>
  </div>
</template>

<script>
import App from './App.vue'
export default {
  name: 'main',
  data: () => {
    return {
      options: {
        brushColor: 'rgb(168,17,139)',
        brushWidth: 10,
        shadowEnable: false,
        shadowWidth: 10
      }
    }
  },
  methods: {
    save: function(data) {
      console.log(data)
    },
    getColor: function Color() {
      this.r = Math.floor(Math.random() * 255)
      this.g = Math.floor(Math.random() * 255)
      this.b = Math.floor(Math.random() * 255)
      return 'rgb(' + this.r + ',' + this.g + ',' + this.b + ')'
    },
    changeColor: function(data) {
      this.options = Object.assign({}, this.options, {
        brushColor: this.getColor()
      })
    }
  },
  components: {
    App: App
  }
}
</script>

<style>
.btn {
  position: absolute;
  top: 10px;
  right: 10px;
}
</style>
