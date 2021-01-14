<template>
  <div class="card card-w70">
    <div v-if="box.length>0">
      <div v-for="item in box" :key="item.id">
        <h1  v-if="item.tip==='title'">{{item.text}}</h1>
        <h2  v-if="item.tip==='subtitle'">{{item.text}}</h2>
        <div v-if="item.tip==='avatar'" class="avatar"> <img :src="item.text"></div>
        <p   v-if="item.tip==='text'">{{item.text}}</p>
      </div>
    </div>
    <h3 v-else>Добавьте первый блок, чтобы увидеть результат</h3>
  </div>
</template>

<script>
export default {
  props: {
    box: Array
  }

}
</script>

<style scoped>
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
