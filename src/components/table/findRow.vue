<template>
  <div id="searchWrap">
    <div class="row flex">
      <div class="input-field col s6">
        <input v-model="inputText" placeholder="Placeholder" id="search" type="text">
        <label for="search"> Search (Lower Case)</label>
      </div>
      <select v-model="inputType" class="browser-default">
        <option value="id">id</option>
        <option value="firstName">firstName</option>
        <option value="lastName">lastName</option>
        <option value="email">email</option>
        <option value="phone">phone</option>
      </select>
      <a class="waves-effect waves-light btn" @click="searchItem()">FIND USER</a>
    </div>
    <div class="row blue-grey darken-1">
      <table v-if="isFind" class="white-text">
        <tr>
          <td>{{ rowSearchResult.id }}</td>
          <td>{{ rowSearchResult.firstName }}</td>
          <td>{{ rowSearchResult.lastName }}</td>
          <td>{{ rowSearchResult.email }}</td>
          <td>{{ rowSearchResult.phone }}</td>
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
      isFind: false,
      rowSearchResult: {},
      searchResultId: {},
      inputText: '',
      inputType: 'id'
    }
  },
  methods: {
    searchItem: function () {
      this.searchResultId = this.$parent.tableData.findIndex((item) => {
        const text = this.inputText.toLowerCase()
        const checkEmpty = text !== null && text !== undefined && text !== ''
        const isNumber = parseInt(text) ?? false
        if ( checkEmpty && isNumber) {
          return text == item[this.inputType]
        } else if (checkEmpty && !isNumber) {
          return text.toLowerCase() == item[this.inputType].toLowerCase()
        } else {
          return false
        }
      })
      this.isFind = true
      this.rowSearchResult = this.$parent.tableData[this.searchResultId]
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