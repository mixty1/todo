<template lang="pug">
.todo-list
  header.todo-list__header
    h3.todo-list__title TO DO
    img.todo-list__logo(src='@/assets/images/logo.png', width='70', height='50')
    .todo-list__actions
      .todo-list__add(@click='addTask') Добавить
  .todo-list__tasks(v-if='tasks.length')
    .todo-list__task-group(
      v-for='date in reversedGrouppedTasks',
      :class='{ "todo-list__task-group--ungroupped": date === "ungroupped" }'
    )
      .todo-list__date(v-if='date !== "ungroupped"') {{ date }}
      transition-group(name='tasks-list')
        task.todo-list__task(
          v-for='task in grouppedTasks[date]',
          :key='task.id',
          :ungroupped='date === "ungroupped"',
          v-bind='task',
          @change='onChange',
          @remove='onRemove'
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
  scheduledAt: null,
  completed: false
}

export default {
  data: () => ({
    tasks: [
      {
        id: '1',
        text: 'Поставить задачи на завтра',
        scheduledAt: new Date(),
        completed: true
      },
      {
        id: '2',
        text: 'Подготовить презентацию',
        scheduledAt: new Date(),
        completed: true
      },
      {
        id: '3',
        text: 'Состряпать бэк для этой тудушки',
        scheduledAt: new Date(new Date() - 300 * 24 * 60 * 60 * 1000),
        completed: false
      },
      {
        id: '4',
        text: 'Изучть новый инструмент',
        scheduledAt: new Date(new Date() - 2000 - 300 * 24 * 60 * 60 * 1000),
        completed: true
      }
    ]
  }),
  computed: {
    grouppedTasks() {
      return _.groupBy(this.tasks, task => {
        if (task.scheduledAt) {
          return this.getDate(task.scheduledAt)
        }

        return 'ungroupped'
      })
    },
    reversedGrouppedTasks() {
      return _.keys(this.grouppedTasks).reverse()
    }
  },
  methods: {
    getDate(date) {
      let day = date.getDate()
      let month = date.getMonth() + 1

      day = day < 10 ? `0${day}` : day
      month = month < 10 ? `0${month}` : month

      return `${day}.${month}.${date.getFullYear()}`
    },
    addTask() {
      const lastTask = this.tasks[this.tasks.length - 1]

      if (_.isEmpty(this.tasks) || (_.has(lastTask, 'text') && lastTask.text.trim())) {
        let task = { ...emptyTask }

        task.id = _.uniqueId()
        this.tasks.push(task)
      }
    },
    onChange(value) {
      const index = this.tasks.findIndex(task => task.id === value.id)

      this.$set(this.tasks, index, _.merge(this.tasks[index], { ...value }))
    },
    onRemove(id) {
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
@import '../assets/styles/_mixins'

.todo-list
  width: 420px
  margin: 0 auto

  &__header
    display: flex
    justify-content: space-between
    align-items: center
    position: relative
    z-index: 5

  &__title
    color: $brand-color
    margin-left: 80px

  &__logo
    position: absolute
    top: 50%
    left: 0
    transform: translateY(-50%)

  &__tasks
    position: relative
    border-radius: 5px
    box-shadow: 0 0 40px rgba(mix(black, $brand-color, 20%), 0.15)
    background: #fff
    z-index: 3

  &__task-group
    padding: 50px
    border-bottom: 1px solid rgba(mix(black, $brand-color, 20%), 0.15)
    transition: background-color 0.2s

    &--ungroupped
      background: $main-gradient
      border-radius: 5px 5px 0 0
      border-bottom: none

      &:last-child
        border-radius: 5px

    &:last-child
      border-bottom: none

  &__task
    transition: transform 0.3s, opacity 0.3s

  &__date
    color: #bbb
    font-size: 13px
    font-weight: bold

  &__empty-list
    background: #fff
    box-shadow: 0 0 40px rgba(mix(black, $brand-color, 20%), 0.15)
    border-radius: 5px
    padding: 50px
    text-align: center
    color: mix(black, $brand-color, 50%)

  &__add
    cursor: pointer
    color: mix(white, $brand-color, 20%)
    font-size: 13px
    background: #fff
    box-shadow: 0 0 10px rgba($brand-color, 0.2)
    padding: 7px 12px
    border-radius: 5px
    font-weight: bold
    transition: color 0.2s

    &:hover
      color: mix(black, $brand-color, 20%)

.tasks-list-enter,
.tasks-list-leave-to
  opacity: 0
  transform: translateY(30px)

.tasks-list-leave-active
  position: absolute
  width: calc(100% - 100px)
</style>
