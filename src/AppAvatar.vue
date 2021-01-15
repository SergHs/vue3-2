<template>
  <div class="avatar">
      <img :src="text">
  </div>

</template>

<script>
export default {
  props: {
    text: String
  }

}
</script>
