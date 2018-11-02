<template lang="pug">
  .task
    input.completed(
      type="checkbox"
      :checked="completed"
      @change="$emit('change', { completed: $event.target.checked, id })"
    )
    input.text(
      type="text"
      :value="text"
      @input="$emit('change', { text: $event.target.value, id })"
    )
</template>

<script>
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
    }
  }
}
</script>

<style scoped lang="sass">

</style>
