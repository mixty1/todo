<template lang="pug">
  #app
    .wrap
      TodoList
</template>

<script>
import TodoList from './components/TodoList'

export default {
  name: 'app',
  components: {
    TodoList
  }
}
</script>

<style lang="sass">
@import './assets/styles/_colors'

html,
body
  height: 100%
  outline: none
  margin: 0
  box-sizing: border-box
  font-family: 'Avenir', Helvetica, Arial, sans-serif
  -webkit-font-smoothing: antialiased
  -moz-osx-font-smoothing: grayscale
  color: #2c3e50

#app
  height: 100%
  display: flex
  flex-direction: column
  justify-content: center
  align-items: center

.wrap
  width: 100%
  height: 100%
  overflow: auto
  padding: 50px 0
</style>
