<template>
  <v-app>
    <v-main>
      <!-- <v-container class="vappwidth"> -->
      <router-view></router-view>
      <!-- </v-container> -->
    </v-main>
  </v-app>
</template>

<script>
export default {
  name: "App",
};
</script>

<style scoped>
.vappwidth {
  max-width: 100%;
  padding: 0px;
}
</style>
