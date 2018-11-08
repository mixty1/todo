<template lang="pug">
  .task
    checkbox(
      :checked="completed"
      @change="$emit('change', { completed: $event.target.checked, id })"
    )
    input.task__text(
      type="text"
      :value="text"
      :class="{ 'task__text--completed': completed }"
      @input="$emit('change', { text: $event.target.value, id })"
      :autofocus="autofocus"
    )
    .task__time {{ createdAt | time }}
    svg.task__remove(@click="$emit('remove', id)" width="20" height="20")
      image(xlink:href="@/assets/images/close-icon_purple.svg" width="20" height="20")
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
    },
    createdAt: Date
  },
  filters: {
    time (date) {
      let hours = date.getHours()
      let minutes = date.getMinutes()
      let seconds = date.getSeconds()

      hours = hours < 10 ? `0${hours}` : hours
      minutes = minutes < 10 ? `0${minutes}` : minutes
      seconds = seconds < 10 ? `0${seconds}` : seconds

      return `${hours}.${minutes}.${seconds}`
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
  position: relative

  &:hover .task__remove
    visibility: visible
    opacity: 1
    transform: scale(1)

  &:hover .task__time
    visibility: visible
    opacity: 1

  &__text
    width: 85%
    margin-left: 10px
    border: none
    outline: none
    background: transparent
    color: mix(black, $main-gradient, 30%)
    font-size: 14px

    &--completed
      text-decoration: line-through
      color: mix(black, $main-gradient, 50%)

    &:focus
      text-decoration: none

  &__time
    opacity: 0
    visibility: hidden
    color: #fff
    background: $brand-color
    padding: 5px 10px
    border-radius: 3px
    font-size: 12px
    font-weight: bold
    position: absolute
    top: 50%
    left: 105%
    transform: translateY(-50%)
    transition: opacity 0.4s ease-in-out

    &:before
      content: ''
      border-right: 5px solid $brand-color
      border-top: 5px solid transparent
      border-bottom: 5px solid transparent
      position: absolute
      top: 50%
      right: 100%
      transform: translateY(-50%)

  &__remove
    width: 20px
    cursor: pointer
    opacity: 0
    visibility: hidden
    transform: scale(0.5)
    transition: opacity 0.2s, transform 0.2s
    filter: drop-shadow(0 0 10px rgba($brand-color, 0.5))
</style>
