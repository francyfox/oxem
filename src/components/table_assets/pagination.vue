<template>
  <ul class="pagination">
    <li class="waves-effect" :class="{ disabled: page == 1}"><a href="#"><i class="material-icons" @click.prevent="lastPage">chevron_left</i></a></li>
    <li v-for="n in pagesArray" :key="n" :class="{ active: page == n ? true : false }">
      <a @click="changePage(n)" v-if="n <= pagEnd && n + 1 > pagStart" :href="'#' + n">{{ n }}</a>
    </li>
    <li class="waves-effect" :class="{ disabled: page == pageCount}"><a href="#" @click.prevent="nextPage"><i class="material-icons">chevron_right</i></a></li>
  </ul>
</template>

<script>
export default {
  name: "pagination",
  props: {
    pagesArray: [],
    page: Number,
    pageCount: Number,
    start: Number,
    end: Number
  },
  data: function () {
    return {
      pagStart: 1,
      pagEnd: 5
    }
  },
  methods: {
    lastPage: function () {
      if (this.page > 1) {
        this.changePage(--this.page);
        this.pagStart--;
      }
      if (this.pagEnd > 5) {
        this.pagEnd--;
      }
    },
    nextPage: function () {
      if (this.page !== this.pageCount) {
        this.changePage(++this.page);
      }
      if (this.pagEnd !== this.pageCount) {
        this.pagStart++;
        this.pagEnd++;
      }
    },
    changePage: function (currentPageNumber) {
      const step = 10
      this.$emit('changePage', {
        page: currentPageNumber,
        start: ( currentPageNumber * step ) - step,
        end: currentPageNumber * step
      })
    }
  }
}
</script>

<style scoped>

</style>