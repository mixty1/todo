<template lang="pug">
.checkbox
  input.checkbox__input(
    type='checkbox',
    :id='`checkbox-${uuid}`',
    :checked='checked',
    @change='$emit("change", $event)'
  )
  label.checkbox__label(:for='`checkbox-${uuid}`', :class='{ "checkbox__label--small": small }')
</template>

<script>
import { uniqueId } from 'lodash'

export default {
  props: {
    small: {
      type: Boolean,
      default: false
    },
    checked: {
      type: Boolean,
      default: false
    }
  },
  data: () => ({
    uuid: uniqueId()
  })
}
</script>

<style scoped lang="sass">
@import '../assets/styles/_colors'
@import '../assets/styles/_mixins'

.checkbox
  position: relative
  width: 15px
  height: 15px

  &__input
    display: none

    &:checked + .checkbox__label:after
      border: 1px solid transparent

    &:checked + .checkbox__label:before
      opacity: 1

  &__label
    cursor: pointer
    width: 100%
    height: 100%
    +vh-align

    &:after
      content: ''
      background: #fff
      width: 100%
      height: 100%
      border-radius: 50%
      border: 1px solid $brand-color
      transition: border-color
      +vh-align
      z-index: 2

    &:before
      content: ''
      background: url('../assets/images/check-icon_purple.svg') no-repeat center
      background-size: 100% 100%
      width: 18px
      height: 18px
      opacity: 0
      transition: opacity 0.2s, transform 0.2s
      +vh-align
      z-index: 3

    &--small:before
      width: 13px
      height: 13px
</style>
