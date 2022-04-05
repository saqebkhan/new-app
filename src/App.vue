<template>
  <v-app>
    <v-data-table
      :headers="headers"
      :items="examiners"
      class="elevation-1"
      checkbox-color="blue"
      :search="search"
      :page.sync="page"
      :items-per-page="itemsPerPage"
      @page-count="pageCount = $event"
    >
      <template v-slot:top>
        <v-toolbar flat>
          <v-toolbar-title>Examiner Assignment Master</v-toolbar-title>
          <template>
            <v-row justify="center">
              <v-dialog v-model="searchDialog" persistent max-width="600px">
                <template v-slot:activator="{ on, attrs }">
                  <v-btn
                    color="primary"
                    dark
                    v-bind="attrs"
                    v-on="on"
                    class="search-btn"
                  >
                    Search
                  </v-btn>
                </template>
                <v-card>
                  <v-toolbar class="text-h5" color="primary" dark
                    >Search Examiner Master</v-toolbar
                  >
                  <v-card-text>
                    <v-container>
                      <v-row>
                        <v-col cols="12" sm="6" md="4">
                          <v-text-field
                            label="Examiner Name"
                            v-model="search"
                          ></v-text-field>
                        </v-col>
                        <v-col cols="12" sm="6" md="4">
                          <v-autocomplete
                            label="Line Of Business"
                            hint="example of helper text only on focus"
                          ></v-autocomplete>
                        </v-col>
                        <v-col cols="12" sm="6" md="4">
                          <v-autocomplete label="Product Name"></v-autocomplete>
                        </v-col>
                        <v-col cols="12" sm="6">
                          <v-select
                            :items="[Acc]"
                            label="Coverage Type"
                          ></v-select>
                        </v-col>
                        <v-col cols="12" sm="6">
                          <v-autocomplete
                            :items="[1, 2, 3]"
                            label="Claim Type"
                            multiple
                          ></v-autocomplete>
                        </v-col>
                      </v-row>
                    </v-container>
                    <small>*indicates required field</small>
                  </v-card-text>
                  <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn
                      color="blue darken-1"
                      text
                      @click="searchDialog = false"
                    >
                      Close
                    </v-btn>
                    <v-btn
                      color="blue darken-1"
                      text
                      @click="searchDialog = false"
                    >
                      Search
                    </v-btn>
                  </v-card-actions>
                </v-card>
              </v-dialog>
            </v-row>
          </template>
          <v-divider class="mx-4" inset vertical></v-divider>
          <v-spacer></v-spacer>
          <v-dialog v-model="dialog" max-width="600px">
            <template v-slot:activator="{ on, attrs }">
              <v-btn color="primary" dark class="mb-2" v-bind="attrs" v-on="on">
                Create Master
              </v-btn>
            </template>
            <v-card>
              <v-toolbar class="text-h5" color="primary" dark>{{
                formTitle
              }}</v-toolbar>

              <v-card-text>
                <v-container>
                  <v-row>
                    <v-col cols="12" sm="6" md="4">
                      <v-text-field
                        v-model="editedItem.name"
                        label="Examiner name"
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12" sm="6" md="4">
                      <v-text-field
                        v-model="editedItem.lineOfBusiness"
                        label="Line of Business"
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12" sm="6" md="4">
                      <v-text-field
                        v-model="editedItem.productName"
                        label="Product Name"
                      ></v-text-field>
                    </v-col>
                    <v-col cols="18" sm="9" md="6">
                      <v-text-field
                        v-model="editedItem.coverageType"
                        label="Coverage Type"
                      ></v-text-field>
                    </v-col>
                    <v-col cols="18" sm="9" md="6">
                      <v-text-field
                        v-model="editedItem.claimType"
                        label="Claim Type"
                      ></v-text-field>
                    </v-col>
                  </v-row>
                </v-container>
              </v-card-text>

              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="blue darken-1" text @click="close">
                  Cancel
                </v-btn>
                <v-btn color="blue darken-1" text @click="save"> Submit </v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
          <v-dialog v-model="dialogDelete" max-width="500px">
            <v-card>
              <v-toolba>
                <v-card-title class="text-h5"
                  >Are you sure you want to delete this item?</v-card-title
                >
                <v-card-actions>
                  <v-spacer></v-spacer>
                  <v-btn color="blue darken-1" text @click="closeDelete"
                    >Cancel</v-btn
                  >
                  <v-btn color="blue darken-1" text @click="deleteItemConfirm"
                    >OK</v-btn
                  >
                  <v-spacer></v-spacer>
                </v-card-actions>
              </v-toolba>
            </v-card>
          </v-dialog>
        </v-toolbar>
      </template>

      <template v-slot:item.actions="{ item }">
        <v-icon small class="mr-2" @click="editItem(item)"> mdi-pencil </v-icon>
        <v-icon small @click="deleteItem(item)"> mdi-delete </v-icon>
      </template>
      <template v-slot:no-data>
        <v-btn color="primary" @click="initialize"> Reset </v-btn>
      </template>
    </v-data-table>
    <v-pagination
      v-model="page"
      class="my-4"
      :length="pageCount"
    ></v-pagination>
  </v-app>
</template>

<script>
export default {
  data: () => ({
    search: "",
    searchDialog: false,
    dialog: false,
    dialogDelete: false,
    page: 0,
    pageCount: 0,
    itemsPerPage: 10,
    headers: [
      { text: "Actions", value: "actions", sortable: false },
      {
        text: "Examiner Name",
        align: "start",
        sortable: false,
        value: "name",
      },
      { text: "Line of Business", value: "lineOfBusiness", sortable: false },
      { text: "Product Name", value: "productName", sortable: false },
      { text: "Coverage Type", value: "coverageType", sortable: false },
      { text: "Claim Type", value: "claimType", sortable: false },
    ],
    examiners: [],
    editedIndex: -1,
    editedItem: {
      name: "",
      lineOfBusiness: "",
      productName: "",
      coverageType: "",
      claimType: "",
    },
    defaultItem: {
      name: "",
      lineOfBusiness: "",
      productName: "",
      coverageType: "",
      claimType: "",
    },
  }),

  watch: {
    dialog(val) {
      val || this.close();
    },
    dialogDelete(val) {
      val || this.closeDelete();
    },
  },

  created() {
    this.initialize();
  },

  methods: {
    initialize() {
      this.examiners = [
        {
          name: "Rajaswami Shetty - RSHETTY",
          lineOfBusiness: "BTA(Retail)",
          productName: "Travel Guard With Sublimit",
          coverageType: "Accidental Damage",
          claimType: "Cashless",
        },
        {
          name: "Ashish Kumar - AKUMAR",
          lineOfBusiness: "BTA(Retail)",
          productName: "Travel Guard With Sublimit",
          coverageType: "Accidental Damage",
          claimType: "Cashless",
        },
        {
          name: "Rajaswami Shetty - RSHETTY",
          lineOfBusiness: "BTA(Retail)",
          productName: "Travel Guard With Sublimit",
          coverageType: "Accidental Damage",
          claimType: "Cashless",
        },
        {
          name: "Ashish Kumar - AKUMAR",
          lineOfBusiness: "BTA(Retail)",
          productName: "Travel Guard With Sublimit",
          coverageType: "Accidental Damage",
          claimType: "Cashless",
        },
        {
          name: "Rajaswami Shetty - RSHETTY",
          lineOfBusiness: "BTA(Retail)",
          productName: "Travel Guard With Sublimit",
          coverageType: "Accidental Damage",
          claimType: "Cashless",
        },
        {
          name: "Ashish Kumar - AKUMAR",
          lineOfBusiness: "BTA(Retail)",
          productName: "Travel Guard With Sublimit",
          coverageType: "Accidental Damage",
          claimType: "Cashless",
        },
        {
          name: "Rajaswami Shetty - RSHETTY",
          lineOfBusiness: "BTA(Retail)",
          productName: "Travel Guard With Sublimit",
          coverageType: "Accidental Damage",
          claimType: "Cashless",
        },
        {
          name: "Ashish Kumar - AKUMAR",
          lineOfBusiness: "BTA(Retail)",
          productName: "Travel Guard With Sublimit",
          coverageType: "Accidental Damage",
          claimType: "Cashless",
        },
        {
          name: "Rajaswami Shetty - RSHETTY",
          lineOfBusiness: "BTA(Retail)",
          productName: "Travel Guard With Sublimit",
          coverageType: "Accidental Damage",
          claimType: "Cashless",
        },
        {
          name: "Ashish Kumar - AKUMAR",
          lineOfBusiness: "BTA(Retail)",
          productName: "Travel Guard With Sublimit",
          coverageType: "Accidental Damage",
          claimType: "Cashless",
        },
        {
          name: "Rajaswami Shetty - RSHETTY",
          lineOfBusiness: "BTA(Retail)",
          productName: "Travel Guard With Sublimit",
          coverageType: "Accidental Damage",
          claimType: "Cashless",
        },
        {
          name: "Ashish Kumar - AKUMAR",
          lineOfBusiness: "BTA(Retail)",
          productName: "Travel Guard With Sublimit",
          coverageType: "Accidental Damage",
          claimType: "Cashless",
        },
      ];
    },

    editItem(item) {
      this.editedIndex = this.examiners.indexOf(item);
      this.editedItem = Object.assign({}, item);
      this.dialog = true;
    },

    deleteItem(item) {
      this.editedIndex = this.examiners.indexOf(item);
      this.editedItem = Object.assign({}, item);
      this.dialogDelete = true;
    },

    deleteItemConfirm() {
      this.examiners.splice(this.editedIndex, 1);
      this.closeDelete();
    },

    close() {
      this.dialog = false;
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem);
        this.editedIndex = -1;
      });
    },

    closeDelete() {
      this.dialogDelete = false;
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem);
        this.editedIndex = -1;
      });
    },

    save() {
      if (this.editedIndex > -1) {
        Object.assign(this.examiners[this.editedIndex], this.editedItem);
      } else {
        this.examiners.push(this.editedItem);
      }
      this.close();
    },
  },
  computed: {
    formTitle() {
      return this.editedIndex === -1
        ? "Create Examiner Assignment Master"
        : "Modify Examiner Assignment Master";
    },
  },
};
</script>

<style scoped>
.search-btn {
  margin-left: 800px;
}
.elevation-1 {
  margin: 12px;
}
</style>
