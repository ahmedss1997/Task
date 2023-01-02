<template>
  <div class="home content-emp transition-all duration-500 ease-in-out pt-10">
    <v-container>
      <v-row 
        no-gutters
        class="mb-4 px-3"
      >
        <v-col cols="12">
          <div class="main-table w-full rounded-lg">
            <div class="">
              <div class="add-product d-flex align-center mb-6">
                <h2 class="text-h4">products</h2>
                <v-spacer></v-spacer>
                <!-- dialog in tables Add -->
                <v-dialog v-model="dialog" persistent max-width="800" class="dig">
                  <template v-slot:activator="{ on, attrs }">
                    <v-btn
                      class="primary txtOnPrimary--text font-weight-bold text-capitalize px-3 py-2"
                      height="41"
                      v-bind="attrs"
                      v-on="on"
                    >
                      <v-icon>mdi-plus</v-icon>
                      <span class="mx-1 ">Add Product</span>
                    </v-btn>
                  </template>
                  <v-card>
                    <v-card-actions>
                      <v-btn
                        absolute
                        top
                        right
                        class="primary rounded-circle pa-0"
                        @click="dialog = false"
                        height="20"
                        min-width="auto"
                        width="20"
                      >
                        <v-icon
                          class="text-caption txtOnPrimary--text"
                          >mdi-close-thick</v-icon
                        >
                      </v-btn>
                    </v-card-actions>
                    <v-card-title
                      class="d-flex justify-center text-h5"
                    >
                      <h3 class="mt-4">Add Product</h3>
                    </v-card-title>
                    <v-card-text>
                      <v-container>
                        <v-form ref="addProduct">
                          <v-row no-gutters>
                            <v-col class="px-3 mb-4" cols="12" md="6">
                              <span class="mb-2 d-inline-block font-medium textOnThirdy--text">
                                Name
                              </span>
                              <v-text-field
                                class=""
                                height="40"
                                label="Full Name"
                                solo
                                :hide-details="false"
                                v-model="newData.Name"
                                :rules="validationRules.textRules"
                              ></v-text-field>
                            </v-col>
                            <v-col class="px-3 mb-4" cols="12" md="6">
                              <span
                                class="mb-2 d-inline-block font-medium textOnThirdy--text"
                                >Parcode</span
                              >
                              <v-text-field
                                class=""
                                height="40"
                                label="Parcode"
                                solo
                                :hide-details="false"
                                v-model="newData.parCode"
                                :rules="validationRules.numberRules"
                              ></v-text-field>
                            </v-col>
                            <v-col class="px-3 mb-4" cols="12" md="6">
                              <span
                                class="mb-2 d-inline-block font-medium textOnThirdy--text"
                                >Product</span
                              >
                              <v-text-field
                                class=""
                                height="40"
                                label="Product"
                                solo
                                :hide-details="false"
                                v-model="newData.Product"
                                :rules="validationRules.textRules"
                              ></v-text-field>
                            </v-col>
                            <v-col class="px-3 mb-4" cols="12" md="6">
                              <span
                                class="mb-2 d-inline-block font-medium textOnThirdy--text"
                                >Price</span
                              >
                              <v-text-field
                                class=""
                                height="40"
                                label="Price"
                                solo
                                :hide-details="false"
                                v-model="newData.Price"
                                :rules="validationRules.numberRules"
                              ></v-text-field>
                            </v-col>
                            <v-btn
                              class="primary txtOnPrimary--text text-capitalize font-bold text-h6 my-10 mx-auto pt-3 pb-4 px-10 d-block rounded-pill"
                              height="50"
                              width="200"
                              @click="addProduct()"
                            >
                              Submit
                            </v-btn>
                          </v-row>
                        </v-form>
                      </v-container>
                    </v-card-text>
                  </v-card>
                </v-dialog>
              </div>
              <v-divider class='my-6'></v-divider>
              <v-text-field
                v-model="searchResults"
                outlined
                dense
                label="Search By Name / Parcode"
                @input="filter()"
              ></v-text-field>
              <!-- <v-autocomplete
                v-model="searchResults"
                :items="Products"
                dense
                filled
                item-text="Name"
                item-value='Name'
                label="Filled"
                @input="filter()"
              ></v-autocomplete> -->
            </div>
            <v-data-table 
              :items="filteredResults.slice(
                (page - 1) * showNum,
                showNum * page
              )"
              :headers="headers"
              hide-default-footer
            >
              <!-- <tbody>
                <tr
                  v-for="(Product, i) in filteredResults.slice(
                    (page - 1) * showNum,
                    showNum * page
                  )"
                  :key="i"
                  class="h-14"
                >
                  <th class="text-left px-3">
                    <v-avatar size="30" class="mx-2">
                      <v-img
                        src="https://cdn.pixabay.com/photo/2020/06/24/19/12/cabbage-5337431_1280.jpg"
                        class="rounded-circle"
                      ></v-img>
                    </v-avatar>
                    <span>{{ Product.Name }}</span>
                  </th>
                  <td class="text-left px-3">{{ Product.parCode }}</td>
                  <td class="text-left px-3">{{ Product.Product }}</td>
                  <td class="text-left px-3">{{ Product.Price }}</td>
                  <td class="text-right px-6">
                    
                  </td>
                </tr>
              </tbody> -->
              <template
                v-slot:[`item.Action`]="props"
              >
                <v-menu bottom left nudge-top="-40" min-width="160">
                  <template v-slot:activator="{ on, attrs }">
                    <v-btn
                      dark
                      icon
                      v-bind="attrs"
                      v-on="on"
                      class="mx-1 my-2 primary"
                    >
                      <v-icon>mdi-dots-vertical</v-icon>
                    </v-btn>
                  </template>

                  <v-list>
                    <v-list>
                      <!-- dialog in tables edit -->
                      <v-dialog
                        v-model="dialog2"
                        persistent
                        max-width="800"
                        class="dig"
                      >
                        <template v-slot:activator="{ on, attrs }">
                          <v-list-item
                            v-bind="attrs"
                            v-on="on"
                            @click="sendID(props.item)"
                          >
                            <v-icon class="mx-1 text-h6">
                              mdi-grease-pencil
                            </v-icon>
                            <v-list-item-title> Edit </v-list-item-title>
                          </v-list-item>
                        </template>
                        <v-card>
                          <v-card-actions>
                            <v-btn
                              absolute
                              top
                              right
                              class="primary rounded-circle pa-0"
                              @click="dialog2 = false"
                              height="20"
                              min-width="auto"
                              width="20"
                            >
                              <v-icon
                                class="text-caption txtOnPrimary--text"
                                >mdi-close-thick</v-icon
                              >
                            </v-btn>
                          </v-card-actions>
                          <v-card-title
                            class="d-flex justify-center text-h5"
                          >
                            <h3 class="mt-4">Edit Product</h3>
                          </v-card-title>
                          <v-card-text>
                            <v-container>
                              <v-form ref="changeData">
                                <v-row no-gutters>
                                  <v-col class="px-3 mb-4" cols="12" md="6">
                                    <span
                                      class="mb-2 d-inline-block font-medium textOnThirdy--text"
                                      >Name</span
                                    >
                                    <v-text-field
                                      class=""
                                      height="40"
                                      label="Full Name"
                                      solo
                                      :hide-details="false"
                                      v-model="editData.Name"
                                      :rules="validationRules.textRules"
                                    ></v-text-field>
                                  </v-col>
                                  <v-col class="px-3 mb-4" cols="12" md="6">
                                    <span
                                      class="mb-2 d-inline-block font-medium textOnThirdy--text"
                                      >Parcode</span
                                    >
                                    <v-text-field
                                      class=""
                                      height="40"
                                      label="Solo"
                                      solo
                                      :hide-details="false"
                                      v-model="editData.parCode"
                                      :rules="validationRules.numberRules"
                                    ></v-text-field>
                                  </v-col>
                                  <v-col class="px-3 mb-4" cols="12" md="6">
                                    <span
                                      class="mb-2 d-inline-block font-medium textOnThirdy--text"
                                      >Product</span
                                    >
                                    <v-text-field
                                      class=""
                                      height="40"
                                      label="Solo"
                                      solo
                                      :hide-details="false"
                                      v-model="editData.Product"
                                      :rules="validationRules.textRules"
                                    ></v-text-field>
                                  </v-col>
                                  <v-col class="px-3 mb-4" cols="12" md="6">
                                    <span
                                      class="mb-2 d-inline-block font-medium textOnThirdy--text"
                                      >Price</span
                                    >
                                    <v-text-field
                                      class=""
                                      height="40"
                                      label="Solo"
                                      solo
                                      :hide-details="false"
                                      v-model="editData.Price"
                                      :rules="validationRules.numberRules"
                                    ></v-text-field>
                                  </v-col>
                                  <v-btn
                                    class="primary txtOnPrimary--text text-capitalize font-bold text-h6 my-10 mx-auto pt-3 pb-4 px-10 d-block rounded-pill"
                                    height="50"
                                    width="200"
                                    @click="editProduct()"
                                  >
                                    Submit
                                  </v-btn>
                                </v-row>
                              </v-form>
                            </v-container>
                          </v-card-text>
                        </v-card>
                      </v-dialog>
                      <!-- dialog in tables Delete -->
                      <v-dialog
                        v-model="dialog3"
                        persistent
                        max-width="560"
                        class="dig"
                      >
                        <template v-slot:activator="{ on, attrs }">
                          <v-list-item v-bind="attrs" v-on="on">
                            <v-icon class="mx-1 text-h6">
                              mdi-delete-forever-outline
                            </v-icon>
                            <v-list-item-title>Delete</v-list-item-title>
                          </v-list-item>
                        </template>
                        <v-card>
                          <v-card-actions>
                            <v-btn
                              absolute
                              top
                              right
                              class="primary rounded-circle pa-0"
                              @click="dialog3 = false"
                              height="20"
                              min-width="auto"
                              width="20"
                            >
                              <v-icon
                                class="text-caption txtOnPrimary--text"
                                >mdi-close-thick</v-icon
                              >
                            </v-btn>
                          </v-card-actions>
                          <v-card-title
                            class="d-flex justify-center text-h5"
                          >
                            <h3 class="mt-4">Delete Product</h3>
                          </v-card-title>
                          <v-card-text>
                            <p class="text-center text-base">
                              Are you sure want to delete?
                            </p>
                            <v-container>
                              <v-row no-gutters class="justify-center">
                                <div
                                  @click="deleteProduct(props.item.id)"
                                  class="shadow-none border-primary border-solid border hover:bg-primary hover:text-white text-primary transition-all duration-500 ease-in-out flex flex-none justify-center items-center text-capitalize font-bold text-h6 mx-3 rounded-pill w-52 h-12 cursor-pointer"
                                >
                                  Delete
                                </div>
                                <div
                                  @click="dialog3 = false"
                                  class="shadow-none border-primary border-solid border hover:bg-primary hover:text-white text-primary transition-all duration-500 ease-in-out flex flex-none justify-center items-center text-capitalize font-bold text-h6 mx-3 rounded-pill w-52 h-12 cursor-pointer"
                                >
                                  Cancel
                                </div>
                              </v-row>
                            </v-container>
                          </v-card-text>
                        </v-card>
                      </v-dialog>
                    </v-list>
                  </v-list>
                </v-menu>
              </template>
            </v-data-table>
          </div>
          <div class="foot-table w-full mt-4">
            <v-row no-gutters class="align-center">
              <v-col cols="4" class="d-flex align-center">
                <span class="mx-3">Go</span>
                <v-sheet max-width="60" color="transparent">
                  <v-text-field
                    v-model="page"
                    solo
                    dense
                    hide-details
                    @input="page = parseInt(page)"
                    type="number"
                  ></v-text-field>
                </v-sheet>
              </v-col>
              <v-col cols="4">
                <template>
                  <div class="text-center">
                    <v-pagination
                      v-model="page"
                      :total-visible="5"
                      :length="Math.ceil(filteredResults.length / (showNum || 1))"
                      circle
                    ></v-pagination>
                  </div>
                </template>
              </v-col>
              <v-col cols="4">
                <div class="d-inline-flex align-center justify-center py-4 w-full">
                  <span class="">Show</span>
                  <v-select
                    v-model="showNum"
                    :items="showPerPage"
                    solo
                    dense
                    hide-details
                    class="mx-3 w-20 block flex-none rounded-lg"
                  ></v-select>
                  <span>Entries</span>
                </div>
              </v-col>
            </v-row>
          </div>
        </v-col>
      </v-row>
      <v-snackbar v-model="error.displayError" multi-line>
        <div v-if="error.errorCode" class="text-center">
          <span class="mx-2">Something Went Wrong!</span>
        </div>
        <div v-else class="text-center">
          <span class="mx-2">Something Went Wrong tow!</span>
        </div>
        <template v-slot:action="{ attrs }">
          <v-btn
            color="red"
            text
            v-bind="attrs"
            @click="error.displayError = false"
          >
            Close
          </v-btn>
        </template>
      </v-snackbar>
      <v-snackbar
        :timeout="2000"
        :value="true"
        fixed
        bottom
        color="success"
        elevation="24"
        v-model="dialoges.success"
      >
        <div class="text-center">
          Saved successfully <v-icon class="mx-2">mdi-check-bold</v-icon>
        </div>
      </v-snackbar>
    </v-container>
  </div>
</template>

<script lang="ts">

import { Component, Vue } from "vue-property-decorator";
import * as models from "@/code/models";
// import * as Emp from "@/code/apis/api.employee";
import * as validate from "@/code/validation";

export default Vue.extend({
  name: "home",
  components: {
    
  },
  data: () => ({
    searchResults: "",
    page: 1,     // which page i'm in 
    showNum: 1,   // row numbers in page
    dialog: false,
    dialog2: false,
    dialog3: false,
    dialoges: {
      success: false,
    },
    error: {
      displayError: false,
    },
    validationRules: validate.validation,
    newData: {} as models.product,
    editData: {} as models.product,
    allProduct: [] as models.product[],
    showPerPage: [] as number[],   // on each page how many rows would I show 
    filteredResults: [] as any[],
    lists: [
      { title: "Edit", iconn: "mdi-grease-pencil" },
      { title: "Delete", iconn: "mdi-delete-forever-outline" },
    ],
    headers: [
      {
        text: 'Name',
        align: 'start',
        filterable: false,
        value: 'Name',
      },
      { text: 'ParCode', value: 'parCode' },
      { text: 'List', value: 'Product' },
      { text: 'Price', value: 'Price' },
      { text: 'Action', value: 'Action', sortable:false },
    ]
  }),
  watch: {
    filteredResults: {
      deep: true,
      immediate: true,
      handler() {
        const that = this as any;
        // console.log(that.Products.length);
        that.showPerPage = [];
        if (that.filteredResults.length >= 5) { 
          // that.filteredResults.length = 12
          // 5 <= 12 // true
          // i = 5
          // i is multiplied by 2 on each loop
          // 5 => 10 => 15 <= 12 // false
          for (let i = 5; i <= that.filteredResults.length; i = i * 2) {
            that.showPerPage.push(i); // [5, 10]
          }
          // if the arr length is like (11) => will remain 1 item left
          if (that.filteredResults.length % 5 != 0) {
            that.showPerPage.push(that.filteredResults.length); // [5, 10, 12]
          }
        } else {
          that.showPerPage.push(that.filteredResults.length);
        }
        that.showNum = that.showPerPage[0];
      },
    },
    "$store.state.Products": {
      deep: true,
      immediate: true,
      handler() {
        this.allProduct = this.$store.state.Products;
        this.filter();
      }
    }
  },
  computed: {
  },
  methods: {
    // 1- list => the array we search on; [{Name: 'Jhone', ParCode:1}]
    // 2- value => the thing I'm searching for; 55
    // 3- filterBy => what am I filtering by => ['parcode', 'Name']
    filter() {
      const filterBy = ['parCode', 'Name', 'Product', 'Price'];
      const value    = this.searchResults;
      const list     = this.allProduct;
      console.log('inside filter ' + list);
      let FilterValue = "";
      let results = [] as any[];
      if (value) {
        for (let i = 0; i < filterBy.length; i++) {
          FilterValue = filterBy[i];
          results.push(list.filter((x: any) => x[FilterValue].toString().toLowerCase().indexOf(value.toLowerCase()) > -1));
          results = results.flat();
        }
        this.filteredResults = results;
        this.filteredResults = this.filteredResults.filter((element, index, arr) =>
          index === arr.findIndex((x) => (
            x.id == element.id
          ))
        )
      } else {
        this.filteredResults = this.allProduct;
      }
      // return results;
    },
    addProduct() {
      const form = this.$refs.addProduct as HTMLFormElement;
      console.log(form);
      if (form && form.validate()) {
        this.newData.id = Math.random();
        this.$store.state.Products.push(this.newData);
        console.log(this.$store.state.Products);
        this.dialoges.success = true;
        this.dialog = false;
      } else {
        this.error.displayError = true;
      }
    },
    sendID(Product: models.product) {
      this.editData = Object.assign({}, Product)
      // console.log(Product);
      // console.log(this.editData);
    },
    editProduct() {
      const form = this.$refs.changeData as HTMLFormElement;
      console.log(form);
      if (form && form.validate()) {
        let element = this.$store.state.Products;
        for (let i = 0; i < element.length; i++) {
          if (element[i].id == this.editData.id) {
            element[i] = this.editData;
          }
        }
        this.$store.commit("get_currentproduct", element);
        console.log('test')
        this.dialoges.success = true;
        this.dialog2 = false;
      } else {
        this.error.displayError = true;
      }
    },
    deleteProduct(id: number) {
      let elements = this.$store.state.Products.filter((x: models.product) => x.id != id);
      this.$store.commit("get_currentproduct", elements);
      this.dialoges.success = true;
      this.dialog3 = false;
    }
  },
  // created() {
  // },
});


</script>

<style lang="scss">

.content-emp {
  .main-table {
    table {
      background-color: #F2F7FD;
      // thead {}
      tbody {
        tr:nth-child(odd) {
          background-color: #E9E9E9;
          &:hover {
            background-color: #E9E9E9 !important;
          }
        }
        tr {
          &:hover {
            background-color: transparent !important;
          }
        }
      }
    }
  }
}


</style>





































