<template>
  <the-header
    @create-task="createTask"
  ></the-header>

  <div class="container">
    <the-loader v-if="loading"></the-loader>

    <div class="card" v-else>
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
                :date="task.date"
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
import TheLoader from '@/components/TheLoader'
import axios from 'axios'

export default {
  data () {
    return {
      taskList: [],
      doneTaskList: [],
      loading: false
    }
  },

  created () {
    this.loadTaskList()
    this.loadDoneTaskList()
  },

  methods: {
    async createTask (task) {
      const obj = {
        title: task,
        description: '',
        isOpen: false,
        date: new Date().toLocaleDateString('ru', { hour: 'numeric', minute: 'numeric', second: 'numeric' })
      }

      try {
        const response = await axios.post('https://vue-my-todo-list-default-rtdb.europe-west1.firebasedatabase.app/taskList.json', obj)
        this.taskList.push({
          id: response.data.name,
          ...obj
        })
      } catch (e) {
        console.log(e.message)
      }
    },

    async loadTaskList () {
      try {
        this.loading = true
        const { data } = await axios.get('https://vue-my-todo-list-default-rtdb.europe-west1.firebasedatabase.app/taskList.json')
        if (data) {
          this.taskList = Object.keys(data).map(key => {
            return {
              id: key,
              ...data[key]
            }
          })
        }
        this.loading = false
      } catch (e) {
        this.loading = false
        console.log(e.message)
      }
    },

    async loadDoneTaskList () {
      try {
        const { data } = await axios.get('https://vue-my-todo-list-default-rtdb.europe-west1.firebasedatabase.app/doneTaskList.json')
        if (data) {
          this.doneTaskList = Object.keys(data).map(key => {
            return {
              id: key,
              ...data[key]
            }
          })
        }
      } catch (e) {
        console.log(e.message)
      }
    },

    async doneTask (id) {
      try {
        await axios.delete(`https://vue-my-todo-list-default-rtdb.europe-west1.firebasedatabase.app/taskList/${id}.json`)
        const task = this.taskList.find(item => item.id === id)
        this.taskList = this.taskList.filter(task => task.id !== id)
        delete task.id
        const response = await axios.post('https://vue-my-todo-list-default-rtdb.europe-west1.firebasedatabase.app/doneTaskList.json', task)
        this.doneTaskList.push({
          id: response.data.name,
          ...task
        })
      } catch (e) {
        console.log(e.message)
      }
    },

    async removeTask (id) {
      try {
        await axios.delete(`https://vue-my-todo-list-default-rtdb.europe-west1.firebasedatabase.app/doneTaskList/${id}.json`)
        this.doneTaskList = this.doneTaskList.filter(task => task.id !== id)
      } catch (e) {
        console.log(e.message)
      }
    },

    async cancelDone (id) {
      try {
        await axios.delete(`https://vue-my-todo-list-default-rtdb.europe-west1.firebasedatabase.app/doneTaskList/${id}.json`)
        const task = this.doneTaskList.find(item => item.id === id)
        this.doneTaskList = this.doneTaskList.filter(task => task.id !== id)
        delete task.id
        const response = await axios.post('https://vue-my-todo-list-default-rtdb.europe-west1.firebasedatabase.app/taskList.json', task)
        this.taskList.push({
          id: response.data.name,
          ...task
        })
      } catch (e) {
        console.log(e.message)
      }
    },

    async addDescription (id, str) {
      try {
        const task = this.taskList.find(item => item.id === id)
        task.description = str
        await axios.put(`https://vue-my-todo-list-default-rtdb.europe-west1.firebasedatabase.app/taskList/${id}.json`, task)
      } catch (e) {
        console.log(e.message)
      }
    },

    getZero (num) {
      if (num > 0 && num < 10) {
        return `0${num}`
      } else {
        return num
      }
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
    TheHeader, TheTask, TheDoneTask, TheLoader
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
