<template>
  <div>
    <b-col md="12" class="m-3"  >
      <b-card header="Add Product">
        <b-card-body>
          <b-form autocomplete="off">
            <b-form-group label="Name">
              <b-form-input
                v-validate="{ required: true, min: 3 }"
                v-model="form.name"
                id="name"
                name="name"
                trim
              ></b-form-input>
              <div v-if="submitted" class="error-messsage">
                {{ errors.first("name") }}
              </div>
            </b-form-group>
            <b-form-group label="Price ($)">
              <b-form-input
                v-validate="{ required: true, numeric: true }"
                v-model="form.price"
                id="price"
                name="price"
                trim
              ></b-form-input>
              <div v-if="submitted" class="error-messsage">
                {{ errors.first("price") }}
              </div>
            </b-form-group>
            <b-form-group label="Brand">
              <b-form-input
                v-validate="{ required: true }"
                v-model="form.brand"
                id="brand"
                name="brand"
                trim
              ></b-form-input>
              <div v-if="submitted" class="error-messsage">
                {{ errors.first("brand") }}
              </div>
            </b-form-group>
            <b-form-group label="Inventory" v-slot="{ ariaDescribedby }">
              <div v-if="submitted" class="error-messsage">
                {{ errors.first("inventoryStatus") }}
              </div>
              <b-form-radio
                v-validate="{ required: true }"
                :aria-describedby="ariaDescribedby"
                v-model="form.inventoryStatus"
                name="inventoryStatus"
                value="true"
                >In Stock</b-form-radio
              >
              <b-form-radio
                :aria-describedby="ariaDescribedby"
                v-model="form.inventoryStatus"
                name="inventoryStatus"
                value="false"
                >Out Of Stock</b-form-radio
              >
            </b-form-group>
          </b-form>
        </b-card-body>
        <b-button block style="background-color: purple;" v-on:click="addProduct"
          >Add product</b-button
        >
      </b-card>
    </b-col>
  </div>
</template>

<script>
export default {
  data() {
    return {
      form: {
        name: "",
        price: "",
        brand: "",
        inventoryStatus: "",
      },
      submitted: false,
    };
  },
  methods: {
    async addProduct() {
      this.submitted = true;
      let result = await this.$validator.validate();
      if (result) {
        this.$emit("addProduct", {
          name: this.form.name,
          price: "$" + this.form.price,
          brand: this.form.brand,
          inventoryStatus: this.form.inventoryStatus === "true",
        });
        this.form = {
          name: "",
          price: "",
          brand: "",
          inventoryStatus: "",
        };
        this.submitted = false
      }
      console.log("result: ", result);
      console.log(this.form);
    },
  },
};
</script>

<style scoped>
</style>