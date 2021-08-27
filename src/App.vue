<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <h1>vue3-js-panel</h1>

  <button @click="triggerPanel">open panel</button>

  {{obj.show}}

  <JsPanel :visible="obj.show" :options="options" @close="obj.show = false">
    <div>My awesome content</div>
  </JsPanel>
</template>

<script>
import { reactive } from 'vue'
import JsPanel from './components/JsPanel.vue'
import 'jspanel4/dist/jspanel.min.css'

export default {
  name: 'App',
  components: { JsPanel },
  setup() {
    const obj = reactive({ 
      show: false 
    })

    const options = {
      headerTitle: 'Aesome Panel',
    }

    const triggerPanel = () => {
      obj.show = true
    }

    return {
      obj,
      options,
      triggerPanel
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
