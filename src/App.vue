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

    <div class="card card-w70">
      <div v-if="box.length>0">
        <div v-for="item in box" :key="item.id">
          <component :is="`app-${item.tip}`" :text="item.text"></component>
        </div>
      </div>
      <h3 v-else>Добавьте первый блок, чтобы увидеть результат</h3>
    </div>

  </div>
  <app-comment @alertComment="alertComment"></app-comment>
</template>

<script>
import AppAlert from './AppAlert'
import AppModify from './AppModify'
import AppComment from './AppComment'

import AppTitle from './AppTitle'
import AppSubtitle from './AppSubtitle'
import AppAvatar from './AppAvatar'
import AppText from './AppText'

import axios from 'axios'

export default {
  data () {
    return {
      alert: null,
      box: []
    }
  },
  components: {
    AppAlert,
    AppModify,
    AppComment,
    AppTitle,
    AppSubtitle,
    AppAvatar,
    AppText
  },

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

    async removeRecord () {
      const id = this.box[this.box.length - 1].id
      await axios.delete(`https://vue-with-https-default-rtdb.firebaseio.com/performance/${id}.json`)
      this.box.pop()
    },

    alertComment (err) {
      this.alert = err
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
