<template lang="pug">
  .task(:class="{ 'task--border_white': ungroupped }")
    checkbox(
      :checked="completed"
      :small="ungroupped"
      @change="$emit('change', { completed: $event.target.checked, id })"
    )
    date-picker(
      :ungroupped="ungroupped"
      :date="scheduledAt"
      @input="$emit('change', { scheduledAt: $event, id })"
    )
    input.task__text(
      type="text"
      :value="text"
      :class="{ 'task__text--completed': completed, 'task__text--color_white': ungroupped }"
      @input="$emit('change', { text: $event.target.value, id })"
      @keydown="onType"
      @keyup="onStopType"
      :autofocus="autofocus"
    )
    .task__time(v-if="scheduledAt") Время: {{ scheduledAt | time }}
    transition(name="fade" mode="out-in")
      svg.task__typing(
        v-if="isTyping"
        :class="{ 'task__remove--visible': isTyping }"
        width="20"
        height="20"
        key="typing"
      )
        image(
          v-if="ungroupped"
          xlink:href="@/assets/images/pen-icon_white.svg"
          width="20"
          height="20"
        )
        image(
          v-else
          xlink:href="@/assets/images/pen-icon_purple.svg"
          width="20"
          height="20"
        )
      svg.task__remove(
        v-else
        @click="$emit('remove', id)"
        width="20"
        height="20"
        key="remove"
      )
        image(
          v-if="ungroupped"
          xlink:href="@/assets/images/close-icon_white.svg"
          width="20"
          height="20"
        )
        image(
          v-else
          xlink:href="@/assets/images/close-icon_purple.svg"
          width="20"
          height="20"
        )
</template>

<script>
import { debounce } from 'lodash'
import DatePicker from './DatePicker'
import Checkbox from './Checkbox'

export default {
  props: {
    ungroupped: {
      type: Boolean,
      default: false
    },
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
    scheduledAt: Date
  },
  data: () => ({
    isTyping: false,
    date: null
  }),
  filters: {
    time (date) {
      let hours = date.getHours()
      let minutes = date.getMinutes()
      let seconds = date.getSeconds()

      hours = hours < 10 ? `0${hours}` : hours
      minutes = minutes < 10 ? `0${minutes}` : minutes
      seconds = seconds < 10 ? `0${seconds}` : seconds

      return `${hours}:${minutes}:${seconds}`
    }
  },
  methods: {
    onType (e) {
      if (!(e.keyCode === 9)) {
        this.isTyping = true
      }
    },
    onStopType: debounce(function () {
      this.isTyping = false
    }, 1000)
  },
  components: {
    Checkbox,
    DatePicker
  }
}
</script>

<style scoped lang="sass">
@import '../assets/styles/_colors'

.task
  width: 100%
  display: flex
  align-items: center
  border-bottom: 1px dashed $brand-color
  padding: 10px 0
  position: relative

  &--border_white
    border-bottom-color: #fff

  &:hover .task__remove
    visibility: visible
    opacity: 1
    transform: scale(1)

  &:hover .task__time
    visibility: visible
    opacity: 1

  &__text
    flex: 1
    border: none
    outline: none
    background: transparent
    color: mix(black, $brand-color, 40%)
    font-size: 14px
    transition: color 0.2s

    &--completed
      text-decoration: line-through
      color: mix(black, $brand-color, 40%)

    &--color_white
      color: #fff

    &:focus
      text-decoration: none
      color: mix(white, $brand-color, 10%)

    &--color_white:focus
      color: white

  &__time
    width: 100px
    opacity: 0
    visibility: hidden
    color: #fff
    text-align: center
    background: $brand-color
    padding: 5px 10px
    border-radius: 3px
    font-size: 12px
    font-weight: bold
    position: absolute
    top: 50%
    left: 105%
    transform: translateY(-50%)
    transition: visibility 0.3s, opacity 0.3s ease-in-out

    &:before
      content: ''
      border-right: 5px solid $brand-color
      border-top: 5px solid transparent
      border-bottom: 5px solid transparent
      position: absolute
      top: 50%
      right: 100%
      transform: translateY(-50%)

  &__remove,
  &__typing
    width: 20px
    cursor: pointer
    opacity: 0
    visibility: hidden
    transform: scale(0.5)
    transition: opacity 0.2s, transform 0.2s, visibility 0.2s
    filter: drop-shadow(0 0 10px rgba($brand-color, 0.5))

    &--visible
      visibility: visible
      opacity: 1
      transform: scale(1)

.fade-enter-active,
.fade-leave-active
  transition: opacity 0.3s ease

.fade-enter,
.fade-leave-to
  opacity: 0
</style>
