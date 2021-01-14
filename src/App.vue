<template>
  <div class="container">
    <app-alert :alert="alert" @close="alert=null"></app-alert>
  </div>
  <div class="container column">
    <app-modify @record="writeRecord">
      <template #default="{}">
        <button v-if="box.length !== 0"
          class="btn danger"
          @click.prevent="removeRecord"
        >
          Удалить блок #{{box.length}}
        </button>
      </template>
    </app-modify>
    <app-performance :box="box"></app-performance>
  </div>

  <app-loader v-if="loading"></app-loader>

  <div class="container">
    <p v-if="!isBtnComment">
      <button class="btn primary" @click="loadComment">Загрузить комментарии</button>
    </p>
    <app-comment :comment="comment" >
      <template #default="{ email, body}">
          <div>
            <p><strong>{{ email }}</strong></p>
            <small>{{ body }}</small>
          </div>
      </template>
    </app-comment>
  </div>
</template>

<script>
import AppModify from './AppModify'
import AppPerformance from './AppPerformance'
import AppComment from './AppComment'
import AppAlert from './AppAlert'
import AppLoader from './AppLoader'
import axios from 'axios'

export default {
  data () {
    return {
      alert: null,
      loading: false,
      isBtnComment: false,
      box: [],
      comment: []
    }
  },
  components: { AppModify, AppPerformance, AppComment, AppAlert, AppLoader },

  mounted () {
    this.loadRecords()
  },

  methods: {

    async writeRecord ({ tip, text }) {
      const { data } = await axios.post(
        'https://vue-with-https-default-rtdb.firebaseio.com/performance.json',
        { tip, text }
      )
      this.box.push({ id: data.name, tip, text })
    },

    async loadRecords () {
      try {
        const { data } = await axios.get(
          'https://vue-with-https-default-rtdb.firebaseio.com/performance.json')
        if (data) {
          this.box = Object.keys(data).map(key => {
            return {
              id: key,
              ...data[key]
            }
          })
        }
      } catch (e) {
        this.alert = { type: 'danger', title: 'Ошибка', text: e.message }
      }
    },

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
        this.alert = { type: 'danger', title: 'Ошибка', text: e.message }
      }
    },

    async removeRecord () {
      const id = this.box[this.box.length - 1].id
      await axios.delete(`https://vue-with-https-default-rtdb.firebaseio.com/performance/${id}.json`)
      this.box.pop()
    }
  }
}
</script>
