<template>
  <form class="card card-w30">
    <div class="form-control">
      <label for="type">Тип блока</label>
      <select id="type" v-model="tip">
        <option value="title">Заголовок</option>
        <option value="subtitle">Подзаголовок</option>
        <option value="avatar">Аватар</option>
        <option value="text">Текст</option>
      </select>
    </div>

    <div class="form-control">
      <label for="value">Значение</label>
      <textarea id="value" rows="3" v-model="text"></textarea>
    </div>

    <button
      class="btn primary"
      :disabled="disabledNew"
       @click.prevent="recordNew"
    >Добавить</button>
    <div class="mt-2">
      <slot/>
    </div>
  </form>
</template>
<script>
export default {
  emits: ['record'],
  props: {
    lengthBox: Number
  },
  data () {
    return {
      tip: 'title',
      text: ''
    }
  },
  computed: {
    disabledNew () { return this.text.length < 4 }
  },
  methods: {
    recordNew () {
      this.$emit('record', { tip: this.tip, text: this.text })
      this.tip = 'title'
      this.text = ''
    }
  }

}
</script>
<style scoped>
  .mt-2 {
    margin-top: 2rem;
}
</style>
