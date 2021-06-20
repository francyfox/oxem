<template>
  <div class="user-panel row">
    <div class="col">
      <div class="card-panel blue-grey darken-1">
        <span class="white-text">
          <p>Выбран пользователь <b>{{ user.firstName }} {{ user.lastName}}</b></p>
          <p>Описание:</p>
          <p><i> {{ user.description }} </i></p>
          <div v-if="user.address.streetAddress">
            <p>Адрес проживания: <b>{{ user.address.streetAddress }}</b></p>
            <p>Город: <b>{{ user.address.city }}</b> </p>
            <p>Провинция/штат: <b>{{ user.address.state }}</b></p>
            <p>Индекс: <b>{{ user.address.zip }}</b></p>
          </div>
        </span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "userPanel",
  props: {
    user: Object
  }
}
</script>

<style scoped>

</style>