<template>
  <div class="card">
    <div class="task__card">

      <h3 @click="openTask()">{{ idx + 1 }}. {{ title }}</h3>
      <the-button color="theButton__done" @action="$emit('done-task', id)">завершить</the-button>
    </div>

    <div class="task__text" v-if="isTaskOpen">
      <input class="task__descr" type="text"
             v-if="!isDescription"
             :placeholder="placeholderString"
             v-model="inputValue"
             @keydown.enter="addDescr(id, inputValue)">
      <p v-else>{{ id }}: {{ isDescription }}</p>
    </div>

  </div>
</template>

<script>
import TheButton from '@/components/TheButton'

export default {
  props: {
    id: {
      type: Number,
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
    }
  },
  emits: {
    'done-task' (id) {
      if (id) {
        return true
      }
      console.warn('No id in "done-tusk" emit')
      return false
    },

    'add-descr' (str) {
      if (str) {
        return true
      }
      console.warn('No value in "add-descr" emit')
      return false
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

    addDescr (id, str) {
      this.isDescription = this.inputValue
      this.$emit('add-descr', id, str)
    }
  },

  components: {
    TheButton
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

.task__descr {
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
