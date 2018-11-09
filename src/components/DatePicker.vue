<template lang="pug">
  .date-picker
    date-picker(
      @input="$emit('input', $event)"
      :value="date"
      lang="ru"
      ref="datePicker"
      type="datetime"
      @mouseup.stop
    )
    svg.date-picker__icon(
      @click="toggleCalendar"
      @mouseup.stop
      width="20"
      height="20"
    )
      image(v-if="ungroupped" xlink:href="@/assets/images/calendar-icon_white.svg" width="20" height="20")
      image(v-else xlink:href="@/assets/images/calendar-icon_purple.svg" width="20" height="20")
</template>

<script>
import DatePicker from 'vue2-datepicker'

export default {
  model: {
    prop: 'date',
    event: 'input'
  },
  props: {
    ungroupped: {
      type: Boolean,
      default: false
    },
    date: Date
  },
  data: () => ({
    showCalendar: false
  }),
  methods: {
    toggleCalendar () {
      this.showCalendar = !this.showCalendar

      if (this.showCalendar) {
        this.$refs.datePicker.showPopup()
      } else {
        this.$refs.datePicker.closePopup()
      }
    }
  },
  mounted () {
    document.addEventListener('mouseup', () => { this.showCalendar = false })
  },
  components: {
    DatePicker
  }
}
</script>

<style scoped lang="sass">
  .date-picker
    width: 20px
    height: 20px
    display: flex
    align-items: center
    margin: 0 10px
    position: relative

    &__icon
      cursor: pointer
      transition: transform 0.2s

      &:hover
        transform: scale(1.2)
</style>

<style lang="sass">
@import '../assets/styles/_colors'

.mx-datepicker
  width: auto !important
  height: 100%

.mx-datepicker-popup
  top: 100% !important
  left: 0 !important
  box-shadow: 0 6px 12px rgba($brand-color, 0.175)
  border-color: rgba($brand-color, 0.2)
  border-radius: 5px

.mx-input-wrapper
  display: none !important

.mx-calendar
  color: mix(black, $brand-color, 30%) !important

.mx-calendar-content .cell.actived
  background: $brand-color !important
  color: mix(white, $brand-color, 80%) !important

  &:hover
    color: mix(black, $brand-color, 30%) !important

.mx-calendar-content .cell
  border-radius: 5px
  transition: background-color 0.2s, color 0.2s

  &:hover
    background: mix(white, $brand-color, 80%) !important

.mx-panel-date td.today
  color: mix(white, $brand-color, 40%) !important
  font-weight: bold

.mx-calendar-header > a
  transition: background-color 0.2s, color 0.2s

  &:hover
    color: mix(white, $brand-color, 20%) !important
</style>
