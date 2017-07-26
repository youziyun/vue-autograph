<template>
  <div id="app">
    <autograph></autograph>
  </div>
</template>

<script>
import Autograph from './components/index'

export default {
  name: 'app',
  components: {
    Autograph
  }
}
</script>

<style>
html, body, #app {
  margin: 0; padding: 0; background-color: #f7f7f7; height: 100%; width: 100%;
}
</style>
