# Vue jsPanel

> Vue jsPanel wrapper

## Documentation

Check out the <a href="https://64robots.github.io/vue-js-panel/">docs</a>.

`npm install --save vue-js-panel`

>這是Vue3的版本，因為原本的`this.$slots.default[0].elm`在vue3上會拿到undefined所以改寫法，
解法可以參考[Vue 中怎样获取具名 slot 的 DOM 节点](https://segmentfault.com/q/1010000022089341)

## 使用

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
