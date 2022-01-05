<template>
  <the-header
    @add-new-task="addNewTask"
  ></the-header>

  <div class="container">
    <div class="card">
      <div class="card__title">
      <h2>{{ taskListTitle ? 'Список задач:' : 'Список задач пуст'}}</h2>
      <span v-if="taskRate" class="rate">{{ getZero(taskRate) }}</span>
      </div>
      <the-task v-for="(task, idx) in taskList"
                :key="task.id"
                :idx="idx"
                :id="task.id"
                :title="task.title"
                :description="task.description"
                :is-open="task.isOpen"
                @done-task="doneTask"
                @add-descr="addDescription"
      ></the-task>
    </div>

    <div class="card" v-if="doneList.length">
      <div class="card__title">
        <h2>Выполненые задачи:</h2>
        <span class="rate">{{ getZero(doneRate) }}</span>
      </div>
      <the-done-task v-for="(task, idx) in doneList"
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
      doneList: [],
      taskRate: 0,
      doneRate: 0
    }
  },

  methods: {
    addNewTask (task) {
      const obj = {}
      obj.id = +('1000' + this.id++)
      obj.title = task
      obj.description = ''
      obj.isOpen = false
      this.taskList.push(obj)
      this.taskRate++
    },

    doneTask (id) {
      this.taskRate--
      this.doneRate++
      this.taskList.forEach((item, idx) => {
        if (item.id === id) {
          const task = this.taskList.splice(idx, 1)
          this.doneList.push(task[0])
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
      this.doneRate--
      this.doneList.forEach((item, idx) => {
        if (item.id === id) {
          this.doneList.splice(idx, 1)
        }
      })
    },

    cancelDone (id) {
      this.doneRate--
      this.taskRate++
      this.doneList.forEach((item, idx) => {
        if (item.id === id) {
          const task = this.doneList.splice(idx, 1)
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
    taskListTitle () {
      return this.taskList.length
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
