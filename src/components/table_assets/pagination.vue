<template>
  <ul class="pagination">
    <li class="disabled"><a href="#"><i class="material-icons">chevron_left</i></a></li>
    <li v-for="n in pageCount" :key="n" :class="{ active: page == n ? true : false }">
      <a @click="changePage(n)" :href="'#' + n">{{ n }}</a>
    </li>
    <li class="waves-effect"><a href="#!"><i class="material-icons">chevron_right</i></a></li>
  </ul>
</template>

<script>
export default {
  name: "pagination",
  props: {
    page: Number,
    pageCount: Number,
    start: Number,
    end: Number
  },
  methods: {
    changePage: function (currentPageNumber) {
      const step = 10
      this.$emit('changePage', {
        page: currentPageNumber,
        start: (currentPageNumber * step) - step,
        end: currentPageNumber * step
      })
    }
  }
}
</script>

<style scoped>

</style>