<template>
  <div id="app">
    <div class="wrap card-panel indigo lighten-4">
      <h4>Oxem test users tables</h4>
      <response-table></response-table>
    </div>
  </div>
</template>
<script>
import "materialize-css/dist/css/materialize.min.css";
import "materialize-css/dist/js/materialize.js";
import "material-design-icons/iconfont/material-icons.css";
import ResponseTable from "./components/ResponseTable";

export default {
  name: "App",
  components: {
    ResponseTable
  },
  data: function () {
    return {
      firstName: ''
    }
  },
  mounted() {
    // eslint-disable-next-line no-undef
    M.updateTextFields();
  }
};
</script>

<style lang="scss">
html {
  background: #e8eaf6;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.wrap {
  margin: 0 auto;
  width: 100%;
  max-width: 900px;
}
</style>
