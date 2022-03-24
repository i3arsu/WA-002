<template>
  <v-container>
    <div class="sredina">
      <router-link to="/about">Zadaca</router-link>
    </div>
  </v-container>
</template>

<script>
  export default {
    name: 'HelloWorld',

    data: () => ({
    })
  }
</script>

<style>
.sredina{
  margin: auto;
  width: 50%;
  padding: 10px;
  text-align: center;
}
</style>
