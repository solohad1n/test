<template>
  <div class="table-row" v-for="post in posts" :key="post.id">
    <div class="table-row__main main"></div>
    <div class="main__date row">{{ post.date }}</div>
    <div class="main__title row">{{ post.title }}</div>
    <div class="main__distance row">{{ post.distance }}</div>
    <div class="main__quantity row">{{ post.quantity }}</div>
  </div>
</template>

<script>
export default {
  props: {
    posts: {
      type: Array,
      required: true,
    },
  },
};
</script>

<style>
.table-row {
  display: flex;
  justify-content: space-around;
}
.row {
  flex-basis: 25%;
  padding: 8px 16px;
  text-align: center;
}
</style>