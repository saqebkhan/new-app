<template>
  <v-container>
    <v-btn @click="move">Go to squared Array</v-btn><br /><br />
    <TravelExpense />
  </v-container>
</template>

<script>
import TravelExpense from "../components/TravelExpense.vue";

export default {
  name: "Home-view",
  components: {
    TravelExpense,
  },
  methods: {
    move() {
      this.$router.push("/AboutView");
    },
  },
};
</script>
