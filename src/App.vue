<template>
  <div class="container column">
    <div class="card card-w30">
      <form @submit.prevent="addBlock">
        <div class="form-control">
          <label for="type">Тип блока</label>
          <select id="type" v-model="blockType">
            <option value="title">Заголовок</option>
            <option value="subtitle">Подзаголовок</option>
            <option value="avatar">Аватар</option>
            <option value="text">Текст</option>
          </select>
        </div>

        <div class="form-control">
          <label for="value">Значение</label>
          <textarea id="value" rows="3" v-model="blockContent"></textarea>
        </div>

        <button class="btn primary" :disabled="!isAddActive">Добавить</button>
      </form>
    </div>

    <div class="card card-w70">
      <template v-for="block in blocks" :key="block.id">
        <component
          :is="`app-${block.type}`"
          v-bind="{ content: block.content, id: block.id }"
          @delete="deleteBlock"
          class="block-component"
        ></component>
      </template>

      <h3 v-if="!blocks.length">Добавьте первый блок, чтобы увидеть результат</h3>
    </div>
  </div>
  <div class="container">
    <p v-if="!comments.length">
      <button class="btn primary" @click="getComments">Загрузить комментарии</button>
    </p>
    <div class="card" v-else>
      <h2>Комментарии</h2>
      <ul class="list">
        <li class="list-item" v-for="comment in comments" :key="comment.id">
          <div>
            <p><strong>{{ comment.email }}</strong></p>
            <small>{{ comment.body }}</small>
          </div>
        </li>
      </ul>
    </div>
    <div class="loader" v-if="isLoading"></div>
  </div>
</template>

<script>
import AppTitle from './components/AppTitle'
import AppSubtitle from './components/AppSubtitle'
import AppAvatar from './components/AppAvatar'
import AppText from './components/AppText'
import AppComment from './components/AppComment'
import axios from 'axios'

const commentUrl = 'https://jsonplaceholder.typicode.com/comments?_limit=42'
const dbUrl = 'https://vue-cv-6eff5-default-rtdb.firebaseio.com/cv'

export default {
  data () {
    return {
      comments: [],
      isLoading: false,
      blockType: 'title',
      blockContent: '',
      blocks: []
    }
  },
  components: {
    AppTitle,
    AppSubtitle,
    AppAvatar,
    AppText,
    AppComment
  },
  methods: {
    async getComments () {
      this.isLoading = true
      const resp = await axios.get(commentUrl)
      this.comments = resp.data
      this.isLoading = false
    },
    addBlock () {
      this.blocks.push({
        type: this.blockType,
        content: this.blockContent,
        id: Date.now().toString()
      })
      this.saveBlock({
        type: this.blockType,
        content: this.blockContent
      })
      this.blockType = 'title'
      this.blockContent = ''
    },
    async saveBlock (item) {
      const { data } = await axios.post(dbUrl + '.json', item)
      console.log('Save item', data)
    },
    async getCV () {
      const { data } = await axios.get(dbUrl + '.json')
      if (!data) return
      this.blocks = Object.keys(data).map(key => {
        return {
          id: key,
          ...data[key]
        }
      })
      console.log('Bloks', this.blocks)
    },
    async deleteBlock (id) {
      const { data } = await axios.delete(`${dbUrl}/${id}.json`)
      const idx = this.blocks.findIndex(item => item.id === id)
      console.log('After delete', data)
      if (idx >= 0) {
        this.blocks.splice(idx, 1)
      }
    }
  },
  created () {
    this.getCV()
  },
  computed: {
    isAddActive () {
      return this.blockContent.length > 3
    }
  }
}
</script>

<style>
  .avatar {
    display: flex;
    justify-content: center;
  }

  .avatar img {
    width: 150px;
    height: auto;
    border-radius: 50%;
  }
</style>
