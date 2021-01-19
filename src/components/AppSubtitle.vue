<template>
  <div class="subtitle">
    <h2>{{ content }}</h2>
    <span class="material-icons danger delete-btn" @click="$emit('delete', id)">delete</span>
  </div>
</template>

<script>
export default {
  name: 'AppSubtitle',
  props: {
    content: {
      type: String,
      default: 'Default Title'
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
