<template>
  <div class="pagination">
    <button class="left" @click="decrementPage()" :disabled="page == 1">
      prev
    </button>
    <div class="buttons">
      <div v-for="pages in getPages" :key="pages">
        <!--<button
          v-for="pageNum in totalPage"
          :key="pageNum"
          @click="changePage(pageNum)"
          href="#"
          :class="{
            'current-page': page === pageNum,
          }"
        >
          {{ pageNum }}
        </button>-->
        <button
          v-if="pages.type === 'page'"
          @click="changePage(pages.title)"
          :class="{
            'current-page': page === pages.title,
          }"
        >
          {{ pages.title }}
        </button>
        <button v-if="pages.type === 'dotted'">{{ pages.title }}</button>
      </div>
    </div>

    <button
      class="right"
      @click="incrementPage()"
      :disabled="page == totalPage"
    >
      next
    </button>
  </div>
</template>

<script>
export default {
  data() {
    return {};
  },
  props: {
    changePage: {
      type: Function,
      required: true,
    },
    incrementPage: {
      type: Function,
      required: true,
    },
    decrementPage: {
      type: Function,
      required: true,
    },
    page: {
      type: Number,
      required: true,
    },
    totalPage: {
      type: Number,
      required: true,
    },
  },
  computed: {
    getPages() {
      let myMas = [];
      if (this.page < 4) {
        myMas.push(
          { type: "page", enabled: "true", title: 1 },
          { type: "page", enabled: "true", title: 2 },
          { type: "page", enabled: "true", title: 3 },
          { type: "dotted", enabled: "false", title: "..." },
          { type: "page", enabled: "true", title: +`${this.totalPage}` }
        );
      } else if (this.page >= 3 && this.page < this.totalPage - 3) {
        myMas.push(
          { type: "page", enabled: "true", title: 1 },
          { type: "dotted", enabled: "false", title: "..." },
          { type: "page", enabled: "true", title: +`${this.page - 1}` },
          { type: "page", enabled: "true", title: +`${this.page}` },
          { type: "page", enabled: "true", title: +`${this.page + 1}` },
          { type: "dotted", enabled: "false", title: "..." },
          { type: "page", enabled: "true", title: +`${this.totalPage}` }
        );
      } else if (this.page > +this.totalPage - 7) {
        myMas.push(
          { type: "page", enabled: "true", title: 1 },
          { type: "dotted", enabled: "false", title: "..." },
          { type: "page", enabled: "true", title: +`${this.totalPage - 4}` },
          { type: "page", enabled: "true", title: +`${this.totalPage - 3}` },
          { type: "page", enabled: "true", title: +`${this.totalPage - 2}` },
          { type: "page", enabled: "true", title: +`${this.totalPage - 1}` },
          { type: "page", enabled: "true", title: +`${this.totalPage}` }
        );
      }
      return myMas;
    },
  },
};
</script>

<style>
.pagination {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  margin: 20px auto;
  background-color: #fff;
  max-width: 545px;
  border-radius: 10px;
}
.left {
  border-radius: 10px 0 0 10px;
}
.right {
  border-radius: 0 10px 10px 0;
}
.buttons {
  display: flex;
  justify-content: center;
}
button {
  padding: 15px;
  background: none;
  border: 0;
  cursor: pointer;
  outline: none;
  list-style-type: none;
}
button:hover {
  background: rgb(240, 237, 237);
  transition: 0.3s;
}
.current-page {
  color: green;
  font-weight: bold;
}

@media only screen and (max-width: 600px) {
  .pagination {
    margin: 20px;
    flex-direction: column;
  }
  .left,
  .right {
    border-radius: 10px;
  }
  .buttons {
    margin: 0 auto;
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
  }
  button {
    padding: 20px;
  }
}
</style>
