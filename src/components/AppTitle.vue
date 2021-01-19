<template>
  <div class="title">
    <h1>{{ content }}</h1>
    <span class="material-icons danger delete-btn" @click="$emit('delete', id)">delete</span>
  </div>
</template>

<script>
export default {
  name: 'AppTitle',
  data () {
    return {
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
  },
  props: {
    content: {
      type: String,
      required: true
    },
    id: {
      type: String,
      required: true
    }
  }
}
</script>

<style lang="scss" scoped>

</style>
