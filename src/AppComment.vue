<template>
  <app-loader v-if="loading"></app-loader>

  <div class="container">
    <p v-if="!isBtnComment">
      <button class="btn primary" @click="loadComment">Загрузить комментарии</button>
    </p>

    <div v-else class="card">
      <h2>Комментарии</h2>
      <ul class="list">
        <li class="list-item" v-for="item in comment" :key="item.id">
          <div>
            <p><strong>{{ item.email }}</strong></p>
            <small>{{ item.body }}</small>
          </div>
         </li>
      </ul>
    </div>
  </div>
</template>

<script>
import AppLoader from './AppLoader'
import axios from 'axios'
export default {
  emits: ['alertComment'],
  data () {
    return {
      loading: false,
      isBtnComment: false,
      comment: []
    }
  },
  components: { AppLoader },
  methods: {
    async loadComment () {
      try {
        this.loading = true
        const { data } = await axios.get(
          'https://jsonplaceholder.typicode.com/comments?_limit=42')
        if (data) {
          this.comment = Object.keys(data).map(key => {
            return {
              id: key,
              email: data[key].email,
              body: data[key].body
            }
          })
        }
        this.isBtnComment = true
        this.loading = false
      } catch (e) {
        this.loading = false
        this.$emit('alertComment', { type: 'danger', title: 'Ошибка', text: 'Comment: ' + e.message })
      }
    }
  }
}
</script>
