<template>
  <div class="card indigo lighten-4">
    <find-row></find-row>
    <table class="striped highlight responsive-table">
      <thead>
      <tr>
        <th>
          <div class="row">
            <div class="col">
              <i class="material-icons" @click="sortColumnBy('int','id', true)">arrow_drop_up</i>
              <i class="material-icons" @click="sortColumnBy('int','id', false)">arrow_drop_down</i>
            </div>
            id
          </div>
        </th>
        <th>
          <div class="row">
            <div class="col">
              <i class="material-icons" @click="sortColumnBy('string','firstName', true)">arrow_drop_up</i>
              <i class="material-icons" @click="sortColumnBy('string','firstName', false)">arrow_drop_down</i>
            </div>
            firstName
          </div>
        </th>
        <th>
          <div class="row">
            <div class="col">
              <i class="material-icons" @click="sortColumnBy('string','lastName', true)">arrow_drop_up</i>
              <i class="material-icons" @click="sortColumnBy('string','lastName', false)">arrow_drop_down</i>
            </div>
            lastName
          </div>
        </th>
        <th>
          <div class="row">
            <div class="col">
              <i class="material-icons" @click="sortColumnBy('string','email', true)">arrow_drop_up</i>
              <i class="material-icons" @click="sortColumnBy('string','email', false)">arrow_drop_down</i>
            </div>
            email
          </div>
        </th>
        <th>
          <div class="row">
            <div class="col hidden">
              <i class="material-icons" @click="sortColumnBy('int','phone', true)">arrow_drop_up</i>
              <i class="material-icons" @click="sortColumnBy('int','phone', false)">arrow_drop_down</i>
            </div>
            phone
          </div>
        </th>
      </tr>
      </thead>
      <tbody v-if="!load">
      <tr v-for="(item, index) in tableData" :key="item.index" @click="setUserInfo(item)">
        <template v-if="index <= end && index > start - 1">
          <td><ins>{{ index }}</ins>{{ item.id }}</td>
          <td>{{ item.firstName }}</td>
          <td>{{ item.lastName }}</td>
          <td>{{ item.email }}</td>
          <td>{{ item.phone }}</td>
        </template>
      </tr>
      </tbody>
      <tbody v-else class="preload">
        <tr>
          <td></td>
          <td></td>
          <div class="preloader-wrapper big active">
            <div class="spinner-layer spinner-red-only">
              <div class="circle-clipper left">
                <div class="circle"></div>
              </div><div class="gap-patch">
              <div class="circle"></div>
            </div><div class="circle-clipper right">
              <div class="circle"></div>
            </div>
            </div>
          </div>
          <td></td>
          <td></td>
        </tr>

      </tbody>
    </table>
    <ul class="pagination">
      <li class="disabled"><a href="#"><i class="material-icons">chevron_left</i></a></li>
      <li v-for="n in setPageCount" :key="n" :class="{ active: page == n ? true : false }">
        <a @click="changePage(n)" :href="'#' + n">{{ n }}</a>
      </li>
      <li class="waves-effect"><a href="#!"><i class="material-icons">chevron_right</i></a></li>
    </ul>
    <div v-if="showUser" class="user-panel row">
      <div class="col">
        <div class="card-panel blue-grey darken-1">
        <span class="white-text">
          <p>Выбран пользователь <b>{{ user.firstName }} {{ user.lastName}}</b></p>
          <p>Описание:</p>
          <p><i> {{ user.description }} </i></p>
          <p>Адрес проживания: <b>{{ user.address.streetAddress }}</b></p>
          <p>Город: <b>{{ user.address.city }}</b> </p>
          <p>Провинция/штат: <b>{{ user.address.state }}</b></p>
          <p>Индекс: <b>{{ user.address.zip }}</b></p>
        </span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import findRow from "./table/findRow";

export default {
  name: "table",
  components: {
    findRow
  },
  data: function () {
    return {
      showUser: false,
      user: {},
      load: true,
      tableData: [],
      // pagination
      page: 1,
      pageCount: 1,
      start: 0,
      end: 10
    }
  },
  watch: {
    rowSearchResult: function () {
      console.log('changed')
    }
  },
  created() {
    axios.get('http://www.filltext.com/?rows=80&id={number|1000}&firstName={firstName}&lastName={lastName}&email={email}&phone={phone|(xxx)xxx-xx-xx}&address={addressObject}&description={lorem|32}')
        .then((response) => {
          this.load = false
          this.tableData = response.data
        }).catch((error) => {
          this.log = error
        })
  },
  computed: {
    setPageCount: function (){
      const del = this.tableData.length / 10
      const count = Math.ceil(del)
      return count
    }
  },
  methods: {
    setUserInfo: function (user) {
      this.user = user
      this.showUser = true
    },
    changePage: function (currentPageNumber) {
      this.tableData.slice(this.start)
      const step = 10
      this.page = currentPageNumber
      this.start = (currentPageNumber * step) - step
      this.end = currentPageNumber * step
    },
    sortColumnBy: function (valType, colname, onUp) {

      switch (valType) {
        case 'int':
          if (onUp) {
            this.tableData = this.tableData.sort((a, b) => b[colname] - a[colname])
          } else {
            this.tableData = this.tableData.sort((a, b) => a[colname] - b[colname])
          }
          break
        case 'string':
          this.tableData = this.tableData.sort(function (a, b) {
            if(a[colname] < b[colname]) { return -1; }
            return 0;
          })
          if (onUp) {
            this.tableData.reverse()
          }
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.card {
  padding: 20px;
}
.preload .preloader-wrapper{
  margin: 0 auto;
}
table tr{
  cursor: pointer;
}
table td{
  position: relative;
}

ins{
  font-size: 12px;
  color: rgba(128, 128, 128, 0.64);
  text-decoration: none;
  position: absolute;
  top: 0;
  left: 0;
}

.col{
  display: flex;
  flex-direction: column;
}

.row {
  display: flex;
  align-items: center;
  .material-icons{
    &:hover{
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

.user-panel{
  .col {
    width: 70%;
  }
  p{
    padding: 0;
    margin: 0 0 5px;
    text-align: left;
  }
}

.hidden {
  visibility: hidden;
}
</style>