<template>
  <div class="ProductList">
    <div class="container productTable" v-for="(item, i) in items" :key="i">
      <div class="card">
        <div class="card-body">
          <h5 class="card-title CmName h2">{{ item.CompanyName }}</h5>
          <h6 class="card-subtitle mb-2 text-muted h4 CoName">
            {{ item.ContactName }}
          </h6>
          <p class="city">{{ item.City }}</p>
          <div class="row">
            <p class="card-text col-12 col-sm-3">
              <span style="color: #ababab; font-size: 13px" v-if="item.Fax"
                >Fax :{{ item.Fax }}</span
              >
              <br />
              <span style="color: #ababab; font-size: 13px" v-if="item.Phone"
                >Phone :{{ item.Phone }}</span
              >
            </p>
            <div class="col-0 col-sm-6"></div>

            <div class="col-12 col-sm-3">
              <router-link
                :to="{ name: 'AddProduct', params: { Supplier_id: item.id } }"
                class="btn btn-primary"
                name="sup_id"
                >Add New Product <i class="bi bi-plus-circle"></i
              ></router-link>
            </div>
          </div>
        </div>
      </div>

      <table class="table table-striped">
        <thead>
          <tr>
            <th scope="col">#</th>
            <th scope="col">Product Name</th>
            <th scope="col">Unit Price</th>
            <th scope="col">Action</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(product, i) in item.products" :key="i">
            <th scope="row">{{ product.id }}</th>
            <td>{{ product.ProductName }}</td>
            <td>{{ product.UnitPrice }}</td>
            <td>
              <router-link
                :to="{
                  name: 'EditProduct',
                  params: {
                    UnitPrice: product.UnitPrice,
                    Supplier: item.id,
                    ProductName: product.ProductName,
                    productid: product.id,
                  },
                }"
                class="btn btn-success mb-1 mt-1"
                name="sup_id"
                >Edit<i class="bi bi-pencil-square"></i
              ></router-link>
              &nbsp;<a
                class="btn btn-danger mb-1 mt-1"
                :href="'http://localhost:8000/api/delete-Product/' + product.id"
              >
                Delete <i class="bi bi-trash"></i>
              </a>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import axios from "axios";

export default {
  name: "ProductList",
  created() {
    this.getItems();
  },
  components: {},
  data() {
    return {
      items: [],
      error: [],
      Supplier_id: 0,
    };
  },
  methods: {
    getItems() {
      axios
        .get("http://localhost:8000/api/list-Product")
        .then((response) => {
          console.log(response.data);
          this.items = response.data.Supplier;
        })
        .catch((error) => {
          console.log(error);
          this.error = error;
        });
    },
  },
};
</script>

<style scoped>
* {
  font-size: 16px;
}
#app {
  display: flex;
  justify-content: center;
  font-family: "Work Sans", sans-serif;
}
.productTable {
  border: solid 1px #ddd;
  border-radius: 20px;
  padding: 17px;
  margin-top: 20px;
  background-color: #fff;
}
.CmName {
  color: rgb(66, 185, 131);
  text-align: left;
  display: block;
  text-decoration-line: underline;
}
.CoName {
  text-align: left;
  display: block;
}
.btn-add {
  height: 50px;
  width: 172px;
}
.card {
  border-radius: 20px;
  width: 100%;
  background-color: #f9fffb;
}
.city {
  text-align: left;
}
</style>
