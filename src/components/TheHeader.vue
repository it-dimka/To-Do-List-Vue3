<template>
  <div class="theHeader">
    <div class="container">
      <div class="theHeader__inner">
        <h1>{{ title }}</h1>
        <form @submit.prevent="addNewTask">
          <input class="theHeader__input" :class="{theHeader__input_error: error.placeholder}"
                 type="text" :placeholder="error.placeholder ? error.placeholder : placeHolderText"
                 v-model="inputValue">
          <the-button color="theButton__header">add new task</the-button>
        </form>
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
      inputValue: '',
      error: {
        placeholder: null
      }
    }
  },

  methods: {
    addNewTask () {
      if (this.inputValue.length) {
        this.error.placeholder = null
        this.$emit('add-new-task', this.inputValue)
        this.inputValue = ''
      } else {
        this.error.placeholder = 'Введите название задачи'
      }
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

.theHeader__input_error::-moz-placeholder { color: #fb6221; }
.theHeader__input_error::-webkit-input-placeholder { color: #fb7221; }

</style>
