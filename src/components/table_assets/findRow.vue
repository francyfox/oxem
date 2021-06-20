<template>
  <div id="searchWrap">
    <div class="row flex">
      <div class="input-field col s6">
        <input
            v-model="inputText"
            @keyup="searchItem"
            placeholder="Placeholder"
            id="search"
            type="text"
        >
        <label for="search">Search</label>
      </div>
    </div>
    <div v-if="loadingSearch" class="progress white">
      <div class="indeterminate"></div>
    </div>
  </div>
</template>

<script>
export default {
  name: "search",
  props: {
    finded: Object,
  },
  data: function (){
    return {
      ArrayFiltred: [],
      loadingSearch: false,
      isFind: false,
      searchResultId: {},
      inputText: '',
      inputType: 'id'
    }
  },
  methods: {
    searchItem: function () {
      this.loadingSearch = true
      const text = this.inputText.toLowerCase()
      const checkEmpty = text !== null && text !== undefined && text !== ''
      const isNumber = parseInt(text) ?? false
      this.ArrayFiltred = this.$parent.tableData.filter(item => {
        if (checkEmpty && !isNumber) {
          return Object.values(item).some(word => word.toString().toLowerCase().includes(text))
        } else if (checkEmpty && isNumber) {
          return item['id'] == this.inputText
        }
      });
      this.$emit('searchItem', this.ArrayFiltred)
      if (this.ArrayFiltred.length < 2) {
        this.isFind = true
        this.loadingSearch = false
      } else {
        this.isFind = true
        this.loadingSearch = true
      }
    }
  }
}
</script>

<style lang="scss" scoped>
  .browser-default {
    max-width: 200px;
    margin-right: 10px;
  }

  .flex {
    display: flex;
    align-items: center;
  }
</style>