<template>
  <div class="text block-content">
    <p>{{ content }}</p>
    <span class="material-icons danger delete-btn" @click="$emit('delete', id)">delete</span>
  </div>
</template>

<script>
export default {
  name: 'AppText',
  props: {
    content: {
      type: String,
      default: ''
    },
    id: {
      type: String,
      required: true
    }
  },
  emits: {
    delete (id) {
      if (id) {
        return true
      } else {
        console.log('No data in delete emit')
        return false
      }
    }
  }
}
</script>

<style scoped>

</style>
