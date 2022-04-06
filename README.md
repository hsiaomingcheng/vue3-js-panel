## Documentation

based on the <a href="https://github.com/64robots/vue-js-panel">vue-js-panel</a>
you can check out the <a href="https://64robots.github.io/vue-js-panel/">docs</a>.
<br>
<a href="https://hsiaomingcheng.github.io/vue3-js-panel/">Demo</a>.

## install

`npm install --save vue3-js-panel`

>這是Vue3的版本，因為原本的`this.$slots.default[0].elm`在vue3上會拿到undefined所以改寫法，
解法可以參考[Vue 中怎样获取具名 slot 的 DOM 节点](https://segmentfault.com/q/1010000022089341)

---

## example

```javascript
// components/JsPanel.vue 建立一個JsPanel的component

<template>
  <div v-if="visible" ref="default">
    <slot name="default" />
    <slot v-if="false" name="headerToolbar" />
  </div>
</template>
<script>
import { jsPanel } from 'jspanel4/es6module/jspanel.min.js'

export default {
  name: 'JsPanel',

  props: {
    visible: {
      type: Boolean,
      default: false
    },

    options: {
      type: Object,
      default: () => ({})
    }
  },

  computed: {
    panelOptions() {
      return Object.assign({ onclosed: this.close }, this.options)
    }
  },

  watch: {
    visible(isVisible) {
      if (isVisible) {
        this.createPanel()
      }
    }
  },

  mounted() {
    if (this.visible) {
      this.createPanel()
    }
  },

  methods: {
    async createPanel() {
      await this.$nextTick()

      let options = Object.assign(
        { content: this.$refs.default },
        this.panelOptions
      )

      if (this.$slots.headerToolbar) {
        options = Object.assign(
          { headerToolbar: this.$slots.headerToolbar()[0].el },
          options
        )
      }
      jsPanel.create(options)
    },

    close() {
      this.$emit('close')
      this.$emit('update:visible', false)
    }
  }
}
</script>

```

```javascript
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
  setup () {
    const obj = reactive({
      show: false
    })
    const options = {
      headerTitle: 'Aesome Panel'
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
```

---

## 多開範例
```javascript
<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <h1>vue3-js-panel</h1>

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
```

---

## 這是配合slot的用法

template v-slot的`default`對應到jsPanel.vue的template的`ref`跟`name`也對應到`content`
如果將default改為testDefault，`ref`、`name`、`content`這三處也要把default改為testDefault
```javascript
<JsPanel
  v-for="panel in panels"
  :key="panel.component + '-' + panel.id"
  :options="panel.options"
  :visible="panel.show"
  @close="show = false"
>
  <template v-slot:default>
    <component
      :is="panel.component"
    />
  </template>
</JsPanel>
```

```javascript
// jsPanel.vue
<template>
  <div v-if="visible" ref="default">
    <slot name="default"/>
    <slot
      v-if="false"
      name="headerToolbar"
    />
  </div>
</template>

{ content: this.$refs.default },
```
