<template>
    <div class="avatar">
      <img :src="content" alt="">
      <span class="material-icons danger delete-btn" @click="$emit('delete', id)">delete</span>
    </div>
</template>

<script>
export default {
  name: 'AppAvatar',
  props: {
    content: {
      type: String,
      required: true,
      validator (value) {
        const r = new RegExp('/^(ftp|http|https)://[^ "]+$/')
        return r.test(value)
      }
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
