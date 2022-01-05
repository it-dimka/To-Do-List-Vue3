<template>
  <button class="theButton" :class="color" @click="$emit('action')">
    <slot></slot>
  </button>
</template>

<script>
export default {
  props: {
    color: {
      type: String,
      default: '',
      validator (value) {
        return ['', 'theButton__header', 'theButton__remove', 'theButton__done', 'theButton__cancel'].includes(value)
      }
    }
  }

}
</script>

<style scoped>
.theButton {
  padding: 10px 20px;
  /*width: fit-content;*/
  border: none;
  border-radius: 8px;
  outline: none;
  font-weight: bold;
  font-size: 16px;
  color: white;
  text-transform: capitalize;
}

.theButton:hover {
  box-shadow: inset -2px -2px 2px rgba(0, 0, 0, 0.5);
}

.theButton:active {
  box-shadow: inset 2px 2px 2px rgba(0, 0, 0, 0.5);
}

.theButton__header {
  background: #49aaef;
  font-size: 24px;
}

.theButton__cancel {
  margin-right: 20px;
  background: #67696c;
}

.theButton__done {
  background: #49aaef;
}

.theButton__remove {
  background: #fb7221;
}

</style>
