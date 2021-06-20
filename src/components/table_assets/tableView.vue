<template>
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
      <template v-if="index <= end - 1 && index > start - 1">
        <td>
          <ins>{{ index }}</ins>
          {{ item.id }}
        </td>
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
          </div>
          <div class="gap-patch">
            <div class="circle"></div>
          </div>
          <div class="circle-clipper right">
            <div class="circle"></div>
          </div>
        </div>
      </div>
      <td></td>
      <td></td>
    </tr>

    </tbody>
  </table>
</template>

<script>
export default {
  name: "tableView",
  props: {
    tableData: Array,
    load: Boolean,
    start: Number,
    end: Number
  },
  methods: {
    setUserInfo: function (user) {
      this.$emit('setUserInfo', {
        user: user
      })
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
            if (a[colname] < b[colname]) {
              return -1;
            }
            return 0;
          })
          if (onUp) {
            this.tableData.reverse();
          }
      }
    }
  }
}
</script>