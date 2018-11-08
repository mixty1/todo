<template lang="pug">
  .todo-list
    header.todo-list__header
      h3.todo-list__title TO DO
      .todo-list__actions
        .todo-list__add(@click="addTask") Добавить
    .todo-list__tasks(v-if="tasks.length")
      .todo-list__task-group(v-for="(taskGroup, date) in groupedTasks")
        .todo-list__date {{ date }}
        transition-group(name="tasks-list")
          task.todo-list__task(
            v-for="task in taskGroup"
            :key="task.id"
            v-bind="task"
            @change="onChange"
            @remove="onRemove"
          )
    .todo-list__empty-list(v-else)
      p Отлично, работаем дальше!
</template>

<script>
import Task from './Task'
import _ from 'lodash'

const emptyTask = {
  id: null,
  text: '',
  createdAt: null,
  completed: false
}

export default {
  data: () => ({
    tasks: [
      {
        id: '1',
        text: 'Лишить Мишу премии',
        createdAt: new Date(),
        completed: true
      },
      {
        id: '2',
        text: 'Подготовить презентацию по Гайдми',
        createdAt: new Date(),
        completed: true
      },
      {
        id: '3',
        text: 'Состряпать бэк для этой тудушки',
        createdAt: new Date(new Date() - 300 * 24 * 60 * 60 * 1000),
        completed: false
      },
      {
        id: '4',
        text: 'Полистать пикабу',
        createdAt: new Date(new Date() - 2000 - 300 * 24 * 60 * 60 * 1000),
        completed: true
      }
    ]
  }),
  computed: {
    groupedTasks () {
      return _.groupBy(this.tasks, task => this.getDate(task.createdAt))
    }
  },
  methods: {
    getDate (date) {
      let day = date.getDate()
      let month = date.getMonth() + 1

      day = day < 10 ? `0${day}` : day
      month = month < 10 ? `0${month}` : month

      return `${day}.${month}.${date.getFullYear()}`
    },
    addTask () {
      const lastTask = this.tasks[this.tasks.length - 1]

      if (_.isEmpty(this.tasks) || (_.has(lastTask, 'text') && lastTask.text.trim())) {
        let task = { ...emptyTask }

        task.id = _.uniqueId()
        task.createdAt = new Date()
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

  &__header
    display: flex
    justify-content: space-between
    align-items: center

  &__title
    color: $brand-color

  &__tasks
    position: relative
    border-radius: 5px
    box-shadow: 0 0 20px rgba(100, 100, 100, 0.2)
    background: #fff

  &__task-group
    padding: 50px
    border-bottom: 1px solid #ccc

    &:last-child
      border-bottom: none

  &__task
    transition: transform 0.3s, opacity 0.3s

  &__date
    color: #aaa
    font-size: 13px

  &__empty-list
    text-align: center
    color: mix(black, $brand-color, 50%)

  &__add
    cursor: pointer
    color: mix(black, $brand-color, 50%)

.tasks-list-enter,
.tasks-list-leave-to
  opacity: 0
  transform: translateY(30px)

.tasks-list-leave-active
  position: absolute
  width: calc(100% - 100px)
</style>
