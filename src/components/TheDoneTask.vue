<template>
  <div class="card">
    <div class="task__card">
      <h3>{{ idx + 1 }}. {{ title }}</h3>
      <div>
        <button class="btn btn__cancel mr-20" @click="$emit('cancel', id)">отменить</button>
        <button class="btn btn__remove" @click="$emit('remove-task', id)">удалить</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  emits: {
    'remove-task' (id) {
      if (id) {
        return true
      } else {
        console.warn('No id in "remove-tusk" emit')
        return false
      }
    },
    'cancel' (id) {
      if (id) {
        return true
      } else {
        console.warn('No id in "cancel" emit')
        return false
      }
    }
  },
  props: {
    id: {
      type: String,
      required: true
    },

    title: {
      type: String,
      required: true
    },

    idx: {
      type: Number,
      required: true
    }
  }
}
</script>

<style scoped>
h3 {
  text-transform: capitalize;
}

.task__card {
  margin-bottom: 5px;
  padding: 0 40px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
</style>
