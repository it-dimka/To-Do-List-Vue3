<template>
  <the-header
    @create-task="createTask"
  ></the-header>

  <div class="container">
    <div class="card">
      <div class="card__title">
      <h2>{{ numberOfTasks ? 'Список задач:' : 'Список задач пуст'}}</h2>
      <span v-if="numberOfTasks" class="rate">{{ getZero(numberOfTasks) }}</span>
      </div>
      <the-task v-for="(task, idx) in taskList"
                :key="task.id"
                :idx="idx"
                :id="task.id"
                :title="task.title"
                :description="task.description"
                :is-open="task.isOpen"
                @done-task="doneTask"
                @add-description="addDescription"
      ></the-task>
    </div>

    <div class="card" v-if="numberOfDoneTasks">
      <div class="card__title">
        <h2>Выполненые задачи:</h2>
        <span class="rate">{{ getZero(numberOfDoneTasks) }}</span>
      </div>
      <the-done-task v-for="(task, idx) in doneTaskList"
                     :key="task.id"
                     :idx="idx"
                     :id="task.id"
                     :title="task.title"
                     @remove-task="removeTask"
                     @cancel="cancelDone"
      ></the-done-task>

    </div>

  </div>
</template>

<script>
import TheHeader from '@/components/TheHeader'
import TheTask from '@/components/TheTask'
import TheDoneTask from '@/components/TheDoneTask'

export default {
  data () {
    return {
      id: 1,
      taskList: [],
      doneTaskList: []
    }
  },

  methods: {
    createTask (task) {
      const obj = {
        id: Number(('1000' + this.id++)),
        title: task,
        description: '',
        isOpen: false
      }
      this.taskList.push(obj)
    },

    doneTask (id) {
      this.taskList.forEach((item, idx) => {
        if (item.id === id) {
          const task = this.taskList.splice(idx, 1)
          this.doneTaskList.push(task[0])
        }
      })
    },

    getZero (num) {
      if (num > 0 && num < 10) {
        return `0${num}`
      } else {
        return num
      }
    },

    removeTask (id) {
      this.doneTaskList.forEach((item, idx) => {
        if (item.id === id) {
          this.doneTaskList.splice(idx, 1)
        }
      })
    },

    cancelDone (id) {
      this.doneTaskList.forEach((item, idx) => {
        if (item.id === id) {
          const task = this.doneTaskList.splice(idx, 1)
          this.taskList.push(task[0])
        }
      })
    },

    addDescription (id, str) {
      const task = this.taskList.find(item => item.id === id)
      task.description = str
    }
  },

  computed: {
    numberOfTasks () {
      return this.taskList.length
    },

    numberOfDoneTasks () {
      return this.doneTaskList.length
    }
  },

  components: {
    TheHeader, TheTask, TheDoneTask
  }
}
</script>

<style scoped>

.card__title {
  margin-bottom: 10px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.rate {
  padding: 8px;
  background: #67696c;
  border: 2px solid #232426;
  border-radius: 50%;
  font-weight: bold;
  color: white;
}
</style>
