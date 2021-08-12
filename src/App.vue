<template>
  <div class="content">
    <ul class="item-list" v-if="!isPostLoading">
      <li class="item" v-for="title in titles" :key="title.id">
        <span>{{ title.id }}</span>
        <span class="title"><b>Название:</b> {{ title.title }}</span>
        <span class="body"><b>Описание:</b> {{ title.body }}</span>
      </li>
    </ul>
    <div v-else>Идет загрузка...</div>
  </div>
  <new-pagi
    :fetch-data="fetchData"
    :page="page"
    :totalPage="totalPage"
    :changePage="changePage"
    :decrementPage="decrementPage"
    :incrementPage="incrementPage"
  ></new-pagi>
</template>

<script>
import axios from "axios";
import NewPagi from "./components/NewPagi.vue";
export default {
  components: {
    NewPagi,
  },
  data() {
    return {
      titles: [],
      isPostLoading: false,
      page: 1,
      limit: 10,
      totalPage: 0,
    };
  },
  methods: {
    changePage(pageNum) {
      this.page = pageNum;
      this.fetchData();
    },
    incrementPage() {
      this.page++;
      this.fetchData();
    },
    decrementPage() {
      this.page--;
      this.fetchData();
    },
    async fetchData() {
      try {
        this.isPostLoading = true;
        const response = await axios.get(
          "https://jsonplaceholder.typicode.com/posts",
          {
            params: {
              _page: this.page,
              _limit: this.limit,
            },
          }
        );
        this.totalPage = Math.ceil(
          response.headers["x-total-count"] / this.limit
        );
        this.titles = response.data;
      } catch (e) {
        alert("error");
      } finally {
        this.isPostLoading = false;
      }
    },
  },
  mounted() {
    this.fetchData();
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  background: lightgray;
}
.content {
  padding: 25px;
}
.item-list {
  display: flex;
  flex-direction: column;
}
.item {
  display: flex;
  flex-direction: column;
  border: 3px solid black;
  padding: 15px 10px;
  margin-top: 10px;
  background: lightgreen;
}
</style>
