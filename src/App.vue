<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <h1>vue3-js-panel</h1>

  <h2>多開範例</h2>
  <button
    v-for="item in testPanelArray"
    :key="item.id"
    @click="triggerPanel(item)"
  >
    open {{ item.content }}
  </button>

  <JsPanel
    v-for="item in testPanelArray"
    :key="item.id"
    :visible="item.visible"
    :options="item.options"
    @close="item.visible = false"
  >
    <div>{{item.content}}</div>
  </JsPanel>
</template>

<script setup>
import { reactive } from 'vue'
import JsPanel from './components/JsPanel.vue'
import 'jspanel4/dist/jspanel.min.css'

const testPanelArray = reactive([
  {
    id: 'a-content',
    content: 'A panel content',
    visible: false,
    options: {
      headerTitle: 'A Panel'
    }
  },
  {
    id: 'b-content',
    content: 'B panel content',
    visible: false,
    options: {
      headerTitle: 'B Panel'
    }
  },
  {
    id: 'c-content',
    content: 'C panel content',
    visible: false,
    options: {
      headerTitle: 'C Panel'
    }
  }
])

const triggerPanel = (item) => {
  item.visible = true
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
