<template lang="pug">
  .task
    checkbox(
      :checked="completed"
      @change="$emit('change', { completed: $event.target.checked, id })"
    )
    input.text(
      type="text"
      :value="text"
      :class="{ '-completed': completed }"
      @input="$emit('change', { text: $event.target.value, id })"
      :autofocus="autofocus"
    )
    svg.remove(@click="$emit('remove', id)" width="20" height="20")
      image(v-if="completed" xlink:href="@/assets/images/close-icon_white.svg" width="20" height="20")
      image(v-else xlink:href="@/assets/images/close-icon_purple.svg" width="20" height="20")
</template>

<script>
import Checkbox from './Checkbox'

export default {
  props: {
    id: {
      type: String,
      required: true
    },
    text: {
      type: String,
      required: true
    },
    completed: {
      type: Boolean,
      default: false
    },
    autofocus: {
      type: Boolean,
      default: false
    }
  },
  components: {
    Checkbox
  }
}
</script>

<style scoped lang="sass">
@import '../assets/styles/_colors'

.task
  width: 100%
  display: flex
  align-items: center
  border-bottom: 1px dashed $main-gradient
  padding: 10px 0

  &:hover > .remove
    visibility: visible
    opacity: 1
    transform: scale(1)

  > .text
    width: 85%
    margin-left: 10px
    border: none
    outline: none
    background: transparent
    color: mix(black, $main-gradient, 30%)
    font-size: 14px
    transition: color 0.2s

    &.-completed
      text-decoration: line-through
      color: mix(black, $main-gradient, 50%)

    &:focus
      text-decoration: none

    &.-completed:focus
      color: mix(white, $main-gradient, 80%)

  > .remove
    width: 20px
    cursor: pointer
    opacity: 0
    visibility: hidden
    transform: scale(0.5)
    transition: opacity 0.2s, transform 0.2s
    filter: drop-shadow(0 0 10px rgba($brand-color, 0.5))
</style>
