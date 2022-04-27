<template>
  <v-container>
    <v-btn @click="move">Go To Travel Expense</v-btn>
    <v-row
      ><v-col cols="12" md="4"></v-col
      ><v-col cols="12" md="4">
        <v-card-title>{{ result }}</v-card-title>
        <v-form style="border: 1px solid purple">
          <v-text-field
            label="Search Between 0 to 250,000"
            class="ma-5"
            type="number"
            :rules="rules.loanMin"
            v-model="input"
          >
          </v-text-field>
          <v-btn style="margin: 0px 0px 20px 140px" @click="isSquare"
            >Search</v-btn
          >
        </v-form></v-col
      >
    </v-row>
  </v-container>
</template>

<script>
export default {
  data() {
    return {
      input: null,
      result: "",
      rules: {
        loanMin: (value) =>
          value <= 250000 || "Number should be less than 250000",
      },
    };
  },
  methods: {
    isSquare() {
      let arr = [];
      for (let i = 0; i < 500; i++) {
        arr.push(i ** 2);
      }
      if (arr.includes(Number(this.input))) {
        this.result = this.input + " is a square";
      } else {
        this.result = this.input + " is not a square";
      }
      this.input = "";
    },
    move() {
      this.$router.push("/");
    },
  },
};
</script>
