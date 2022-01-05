<template>
  <div class="theHeader">
    <div class="container">
      <div class="theHeader__inner">
        <h1>{{ title }}</h1>
        <div>
          <input class="theHeader__input" type="text" :placeholder="placeHolderText" v-model="inputValue" @keydown.enter="addNewTask(inputValue)">
          <the-button color="theButton__header" @action="addNewTask(inputValue)">add new task</the-button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import TheButton from '@/components/TheButton'

export default {
  emits: {
    'add-new-task' (value) {
      if (value) {
        return true
      }
      console.warn('Not value in "add-new-task" emit')
    }
  },

  data () {
    return {
      title: 'ToDo List',
      placeHolderText: 'New Task',
      inputValue: ''
    }
  },

  methods: {
    addNewTask (value) {
      this.$emit('add-new-task', value)
      this.inputValue = ''
    }
  },

  components: {
    TheButton
  }
}
</script>

<style scoped>
h1 {
  font-size: 48px;
  letter-spacing: 2px;
  color: white;
}

.theHeader {
  background: #232426;
  margin-bottom: 10px;
}

.theHeader__inner {
  padding: 10px 20px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.theHeader__inner > div {
  display: flex;
  align-items: center;
}

.theHeader__input {
  margin-right: 20px;
  padding-left: 15px;
  width: 300px;
  height: 48px;
  border: none;
  border-radius: 8px;
  outline: none;
  font-size: 20px;
  color: #232426;
}
</style>
