<template>
    <div class="card">
      <h2>Комментарии</h2>
      <ul class="list">
        <li class="list-item" v-for="item in comment" :key="item.id">
          <slot :email="item.email" :body="item.body"></slot>
        </li>
      </ul>
    </div>
</template>

<script>
export default {
  props: {
    comment: Array
  }
}
</script>
