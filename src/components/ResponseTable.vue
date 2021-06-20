<template>
  <div class="card indigo lighten-4">
    <find-row
      @searchItem="onFind"
    />
    {{ log }}
    <table-view
      :end="end"
      :load="load"
      :start="start"
      :tableData="SearchResult"
      @setUserInfo="onUserInfo"
    />
    <pagination
      :end="end"
      :page="page"
      :pageCount="pageCount"
      :pages-array="pagesArray"
      :start="start"
      @changePage="onPage"
      @setPagesArray="changePagesArray"
    />
    <user-panel v-if="showUser" :user="user"/>
  </div>
</template>

<script>
import axios from "axios";
import findRow from "./table_assets/findRow";
import pagination from "./table_assets/pagination";
import Pagination from "./table_assets/pagination";
import tableView from "./table_assets/tableView";
import userPanel from "./table_assets/userPanel";

export default {
  name: "ResponseTable",
  components: {
    Pagination,
    findRow,
    tableView,
    pagination,
    userPanel,
  },
  data: function () {
    return {
      // table
      log: "",
      showUser: false,
      user: {},
      load: true,
      tableData: [],
      // pagination
      pageCount: 1,
      pagesArray: [],
      page: 1,
      start: 0,
      end: 10,
      // search
      SearchResult: [],
    };
  },
  watch: {
    rowSearchResult: function () {
      console.log("changed");
    },
  },
  created() {
    // const retApi = 'https://retoolapi.dev/KrBRnb/oxem'
    const fillApi =
      "http://www.filltext.com/?rows=80&id={number|1000}&firstName={firstName}&lastName={lastName}&email={email}&phone={phone|(xxx)xxx-xx-xx}&address={addressObject}&description={lorem|32}";
    axios
      .get(fillApi)
      .then((response) => {
        this.load = false;
        this.tableData = response.data;
        this.SearchResult = response.data;
        //
        const del = this.tableData.length / 10;
        const count = Math.ceil(del);
        this.pageCount = count;
        //
        for (let i = 1; i <= count; i++) {
          this.pagesArray.push(i);
        }
      })
      .catch((error) => {
        this.log = error + ", please update page, something wrong with api url";
      });
  },
  methods: {
    changePagesArray: function (pages) {
      this.pagesArray = pages;
      this.page++;
    },
    onFind: function (data) {
      this.SearchResult = data;
      if (this.SearchResult.length === 0) {
        this.SearchResult = this.tableData;
      }
      if (this.SearchResult.length !== 0) {
        this.pagesArray = []
        const del = this.SearchResult.length / 10;
        const count = Math.ceil(del);
        this.pageCount = count;
        for (let i = 1; i <= count; i++) {
          this.pagesArray.push(i);
        }
      }
    },
    onPage: function (data) {
      this.tableData.slice(data.start);
      this.page = data.page;
      this.start = data.start;
      this.end = data.end;
    },
    onUserInfo: function (user) {
      this.user = Object.values(user)[0];
      this.showUser = true;
    },
  },
};
</script>

<style lang="scss">
.card {
  padding: 20px;
}

.preload .preloader-wrapper {
  margin: 0 auto;
}

table tr {
  cursor: pointer;
}

table td {
  position: relative;
}

ins {
  font-size: 12px;
  color: rgba(128, 128, 128, 0.64);
  text-decoration: none;
  position: absolute;
  top: 0;
  left: 0;
}

.col {
  display: flex;
  flex-direction: column;
}

.row {
  display: flex;
  align-items: center;

  .material-icons {
    &:hover {
      color: #ee6e73;
    }

    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;
    width: 16px;
    height: 16px;
  }
}

.user-panel {
  .col {
    width: 70%;
  }

  p {
    padding: 0;
    margin: 0 0 5px;
    text-align: left;
  }
}

.hidden {
  visibility: hidden;
}
</style>
