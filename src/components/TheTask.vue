<template>
  <div class="card">
    <div class="task__card">

      <h3 @click="openTask()">{{ idx + 1 }}. {{ title }}</h3>
      <button class="btn btn__done" @click="$emit('done-task', id)">завершить</button>
    </div>

    <div class="task__text" v-if="isTaskOpen">
      <p class="mb-5">Создано: {{ date }}</p>
      <input class="task__description" type="text"
             v-if="!isDescription"
             :placeholder="placeholderString"
             v-model.trim="inputValue"
             @keydown.enter="addDescription(id, inputValue)">
      <p class="bold" v-else>{{ isDescription }}</p>
    </div>

  </div>
</template>

<script>
export default {
  props: {
    id: {
      type: String,
      required: true
    },

    title: {
      type: String,
      required: true
    },

    description: {
      type: String,
      required: false,
      default: ''
    },

    isOpen: {
      type: Boolean,
      required: false,
      default: false
    },

    idx: {
      type: Number,
      required: true
    },

    date: {
      type: String,
      required: false
    }
  },
  emits: {
    'done-task' (id) {
      if (id) {
        return true
      } else {
        console.warn('No id in "done-tusk" emit')
        return false
      }
    },

    'add-description' (str) {
      if (str) {
        return true
      } else {
        console.warn('No value in "add-description" emit')
        return false
      }
    }
  },

  data () {
    return {
      isTaskOpen: this.isOpen,
      isDescription: this.description,
      placeholderString: 'Добавьте описание задачи + enter',
      inputValue: ''
    }
  },

  methods: {
    openTask () {
      this.isTaskOpen = !this.isTaskOpen
    },

    addDescription (id, str) {
      this.isDescription = this.inputValue
      this.$emit('add-description', id, str)
    }
  }

}
</script>

<style scoped>
h3 {
  cursor: pointer;
  text-transform: capitalize;
}

.task__card {
  margin-bottom: 5px;
  padding: 0 40px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.task__text {
  padding-left: 40px;
  max-width: 72%;
}

.task__description {
  width: 60%;
  padding: 5px 0 5px 15px;
  border: none;
  border-radius: 8px;
  outline: none;
  font-size: 14px;
  color: #67696c;
  background: #c5e3f9;
}
</style>
