<template lang="pug">
  .todo-list
    header.header
      h3.title TO DO
      .actions
        .add(@click="addTask") Добавить
    .completed-list
      task.item(
        v-for="task in completedTasks"
        :key="task.id"
        v-bind="task"
        @change="onChange"
      )
    .uncompleted-list
      task.item.-uncompleted(
        v-for="task in uncompletedTasks"
        :key="task.id"
        v-bind="task"
        @change="onChange"
      )
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
      let task = { ...emptyTask }

      task.id = _.uniqueId()
      this.tasks.push(task)
    },
    onChange (value) {
      const index = this.tasks.findIndex(task => task.id === value.id)

      this.$set(
        this.tasks,
        index,
        _.merge(this.tasks[index], { ...value })
      )
    }
  },
  components: {
    Task
  }
}
</script>

<style scoped lang="sass">

</style>
