<template>
  <v-app>
    <v-row>
      <v-col>
        <v-container>
          <form @submit.prevent="card = true">
            <h4>Personal Details</h4>
            <v-row
              ><v-col>
                <v-text-field
                  label="Full Name"
                  outlined
                  dense
                  v-model="name"
                ></v-text-field> </v-col
              ><v-col>
                <v-text-field label="Number" outlined dense></v-text-field
              ></v-col>
            </v-row>
            <v-text-field
              label="Address"
              outlined
              dense
              v-model="address"
            ></v-text-field>
            <h4>Travel Details</h4>
            <v-row>
              <v-col>
                <v-text-field
                  label="Starting Point"
                  outlined
                  dense
                  v-model="startingPoint"
                ></v-text-field
              ></v-col>
              <v-col>
                <v-text-field
                  label="Destination"
                  outlined
                  dense
                  v-model="destination"
                ></v-text-field
              ></v-col>
            </v-row>
            <h4>Your Comfort</h4>
            <v-row
              ><v-col>
                <v-select
                  :items="seater"
                  label="Seaters"
                  outlined
                  dense
                  v-model="seats"
                ></v-select></v-col
              ><v-col>
                <v-select
                  :items="drivers"
                  label="Number Of Drivers"
                  outlined
                  dense
                  v-model="numberOfDrivers"
                ></v-select></v-col
            ></v-row>
            <v-text-field
              label="Number of Days"
              outlined
              dense
              v-model="days"
            ></v-text-field>
            <v-btn :disabled="validate" type="submit">Submit</v-btn>
            <v-btn @click="clear" class="ml-10">Clear</v-btn>
          </form></v-container
        ></v-col
      ><v-col>
        <!-- </v-container> -->
        <v-card
          class="mx-auto"
          max-width="500"
          style="height: 350px; margin-top: 30px"
          v-if="card == true"
        >
          <v-img
            class="white--text align-end"
            height="200px"
            src="https://cdn.vuetifyjs.com/images/cards/docks.jpg"
          >
            <v-card-title>Welcome Mr. {{ name }}</v-card-title>
          </v-img>
          <v-card-subtitle class="pb-0">
            Journey From {{ startingPoint }} to {{ destination }}
          </v-card-subtitle>
          <v-card-text class="text--primary">
            <div>
              The rent for {{ seats }} seater is
              {{ seats == 5 ? 2000 : seats == 12 ? 8000 : 25000 }} and 300 per
              night is charged, Hence the estimated travel expense for
              {{ days }} days journey would be
              {{ days * (seats == 5 ? 2300 : seats == 12 ? 8300 : 25300) }}
              Rupees.
            </div>
          </v-card-text>
          <v-card-actions>
            <v-btn color="orange" text> Share </v-btn>
            <v-btn color="orange" text> Learn More </v-btn>
          </v-card-actions>
        </v-card></v-col
      >
    </v-row>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      seater: [5, 12, 50],
      drivers: [1, 2],
      travelDetails: "",
      name: "",
      startingPoint: "",
      destination: "",
      seats: null,
      days: null,
      card: false,
      numberOfDrivers: "",
      address: "",
    };
  },
  methods: {
    clear() {
      this.seater = "";
      this.days = null;
      this.name = "";
      this.startingPoint = "";
      this.destination = "";
      this.card = false;
      this.address = "";
    },
  },
  computed: {
    validate() {
      if (
        this.seats &&
        this.days &&
        this.name &&
        this.destination &&
        this.startingPoint
      ) {
        return false;
      } else {
        return true;
      }
    },
  },
};
</script>
