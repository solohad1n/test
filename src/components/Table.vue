<template>
  <div class="table">
    <div class="table__header header">
      <div class="header__sort">
        <TableSort v-model="selectedSort" :options="sortOptions" />
      </div>
      <div class="header__search">
        <input type="text" v-model="searchQuery" placeholder="Поиск" />
      </div>
    </div>
    <div class="table__main main">
      <div class="main__header">
        <p>Дата</p>
        <p>Название</p>
        <p>Количество</p>
        <p>Расстояние</p>
      </div>
    </div>
    <div class="main__content content">
      <div class="content__row">
        <TableRow :posts="sortedAndSearchedPosts" />
      </div>
    </div>
    <div class="table__pagination">
      <div
        v-for="pageNumber in totalPages"
        :key="pageNumber"
        class="table__page"
        :class="{
          'table__page-active': page === pageNumber,
        }"
        @click="changePage(pageNumber)"
      >
        {{ pageNumber }}
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import TableRow from "./TableRow.vue";
import TableSort from "./TableSort.vue";
export default {
  components: { TableRow, TableSort },
  data() {
    return {
      page: 1,
      limit: 5,
      totalPages: 0,
      posts: [],
      selectedSort: "",
      searchQuery: "",
      sortOptions: [
        { value: "title", name: "По Названиею" },
        { value: "distance", name: "По Расстоянию." },
        { value: "quantity", name: "По Количеству" },
      ],
    };
  },
  methods: {
    async fetchPost() {
      try {
        const response = await axios.get("http://localhost:3000/incomingData", {
          params: {
            _page: this.page,
            _limit: this.limit,
          },
        });
        this.totalPages = Math.ceil(
          response.headers["x-total-count"] / this.limit
        );
        this.posts = response.data;
      } catch (e) {
        e.message;
      }
    },
    changePage(pageNumber) {
      this.page = pageNumber;
    },
  },
  mounted() {
    this.fetchPost();
  },
  computed: {
    sortedPosts() {
      return [...this.posts].sort((a, b) =>
        a[this.selectedSort]?.localeCompare(b[this.selectedSort])
      );
    },
    sortedAndSearchedPosts() {
      return this.sortedPosts.filter((post) =>
        post.title.toLowerCase().includes(this.searchQuery.toLowerCase())
      );
    },
  },
  watch: {
    page() {
      this.fetchPost();
    },
  },
};
</script>

<style>
.table {
  max-width: 900px;
  margin: 0 auto;
}
.main__header {
  display: flex;
  justify-content: space-around;
  border: 1px solid black;
}
.main__header p {
  flex-basis: 25%;
  text-align: center;
}
.table__pagination {
  display: flex;
  margin-top: 15px;
}
.table__page {
  border: 1px solid black;
  padding: 10px;
}
.table__page-active {
  border: 2px solid red;
}
.table_btn {
  display: flex;
  justify-content: space-between;
  margin-bottom: 100px;
}
.table__header {
  display: flex;
  justify-content: space-around;
  align-items: center;
  margin-bottom: 50px;
}
</style>