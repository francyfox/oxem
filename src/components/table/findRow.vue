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
    <div v-if="isFind" class="row blue-grey darken-1">
      <table class="white-text" v-for="item in SearchResult" :key="item.index">
        <tr>
          <td>{{ item.id }}</td>
          <td>{{ item.firstName }}</td>
          <td>{{ item.lastName }}</td>
          <td>{{ item.email }}</td>
          <td>{{ item.phone }}</td>
        </tr>
      </table>
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
      loadingSearch: false,
      isFind: false,
      SearchResult: {},
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
      this.SearchResult = this.$parent.tableData.filter(item => {
        if (checkEmpty && !isNumber) {
          return Object.values(item).some(word => word.toString().toLowerCase().includes(text))
        } else if (checkEmpty && isNumber) {
          return item['id'] == this.inputText
        }
      });
      if (this.SearchResult.length < 2) {
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