<template lang="pug">
  .todo-list
    header.header
      h3.title TO DO
      .header-actions-bar
        .add(@click="addTask") Добавить
    .lists(v-if="tasks.length")
      .uncompleted-list(v-if="uncompletedTasks.length")
        task.item.-uncompleted(
          v-for="task in uncompletedTasks"
          :key="task.id"
          v-bind="task"
          @change="onChange"
          @remove="onRemove"
          autofocus
        )
      .completed-list(v-if="completedTasks.length")
        task.item(
          v-for="task in completedTasks"
          :key="task.id"
          v-bind="task"
          @change="onChange"
          @remove="onRemove"
        )
    .empty-list(v-else)
      p Отлично, Работаем дальше!
</template>

<script>
import Task from './Task'
import _ from 'lodash'

const emptyTask = {
  id: null,
  text: '',
  completed: false
}

export default {
  data: () => ({
    tasks: []
  }),
  computed: {
    completedTasks () {
      return this.tasks.filter(task => task.completed)
    },
    uncompletedTasks () {
      return this.tasks.filter(task => !task.completed)
    }
  },
  methods: {
    addTask () {
      const lastTask = this.tasks[this.tasks.length - 1]

      if (_.isEmpty(this.tasks) || (_.has(lastTask, 'text') && lastTask.text.trim())) {
        let task = { ...emptyTask }

        task.id = _.uniqueId()
        this.tasks.push(task)
      }
    },
    onChange (value) {
      const index = this.tasks.findIndex(task => task.id === value.id)

      this.$set(
        this.tasks,
        index,
        _.merge(this.tasks[index], { ...value })
      )
    },
    onRemove (id) {
      const index = this.tasks.findIndex(task => task.id === id)

      this.$delete(this.tasks, index)
    }
  },
  components: {
    Task
  }
}
</script>

<style scoped lang="sass">
@import '../assets/styles/_colors'

.todo-list
  width: 420px
  margin: 0 auto

  > .header
    display: flex
    justify-content: space-between
    align-items: center

  > .header > .title
    color: $brand-color

  > .lists
    border-radius: 5px
    overflow: hidden
    box-shadow: 0 0 20px rgba(100, 100, 100, 0.2)

  > .lists > .completed-list,
  > .lists > .uncompleted-list
    padding: 50px

  > .lists > .completed-list
    background: linear-gradient(180deg, rgba(176,168,254,1) 0%, rgba(122,124,214,1) 100%)

  > .empty-list
    text-align: center
    color: mix(black, $brand-color, 50%)

.header-actions-bar

  > .add
    cursor: pointer
    color: mix(black, $brand-color, 50%)
</style>
