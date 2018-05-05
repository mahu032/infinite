<template>
  <ol>
    <li v-for="(li, index) in list" :key="index">{{li}}</li>
  </ol>
</template>

<script>
export default {
  props: {
    list: {
      type: Array,
      default: function() {
        return []
      }
    }
  },
}
</script>
<style scoped>
li {
  height: 80px;
  line-height: 80px;
  margin-bottom: 20px;
  border: 1px solid #000;
}

li:nth-child(2n) {
  background: orange;
}
</style>
