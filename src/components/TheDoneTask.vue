<template>
  <div class="card">
    <div class="task__card">
      <h3>{{ idx + 1 }}. {{ title }}</h3>
      <div>
        <the-button color="theButton__cancel" @action="$emit('cancel', id)">отменить</the-button>
        <the-button color="theButton__remove" @action="$emit('remove-task', id)">удалить</the-button>
      </div>
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

    idx: {
      type: Number,
      required: true
    }
  },
  emits: {
    'remove-task' (id) {
      if (id) {
        return true
      }
      console.warn('No id in "remove-tusk" emit')
      return false
    },
    'cancel' (id) {
      if (id) {
        return true
      }
      console.warn('No id in "cancel" emit')
      return false
    }
  },

  components: {
    TheButton
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
