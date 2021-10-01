<template>
  <div class="OrdertList">
    <router-link
      :to="{ name: 'AddOrder' }"
      class="btn btn-primary"
      name="sup_id"
      >Add New Order <i class="bi bi-plus-circle"></i
    ></router-link>
    <div class="container productTable" v-for="(item, i) in items" :key="i">
      <div class="card">
        <div class="card-body">
          <h5 class="card-title CmName h2">Name :</h5>
          <span style="color: #000"
            >{{ item.customer.FirstName }} {{ item.customer.LastName }}</span
          >

          <p class="city">{{ item.customer.City }}</p>
          <div class="row">
            <p class="card-text col-12 col-sm-3">
              <span
                style="color: #ababab; font-size: 13px"
                v-if="item.customer.Phone"
                >Phone :{{ item.customer.Phone }}</span
              >
            </p>
            <div class="col-0 col-sm-6"></div>

            <div class="col-12 col-sm-3">
              &nbsp;
              <router-link
                :to="{
                  name: 'EditOrder',
                  params: {
                    orderId: item.id,
                  },
                }"
                class="btn btn-success mb-1 mt-1"
                name="sup_id"
                >Edit<i class="bi bi-pencil-square"></i
              ></router-link>
            </div>
          </div>
        </div>
      </div>
      <div v-for="(orderI, i) in item.order_item" :key="i">
        <div v-for="(product, ik) in products" :key="ik">
          <table class="table table-striped">
            <thead>
              <tr v-if="orderI.ProductId == product.id">
                <th scope="col">#</th>
                <th scope="col">Product Name</th>
                <th scope="col">UnitPrice</th>
                <th scope="col">Quantity</th>
                <th scope="col">Action</th>
              </tr>
            </thead>

            <tbody>
              <tr v-if="orderI.ProductId == product.id">
                <th scope="row">{{ orderI.id }}</th>
                <td>{{ product.ProductName }}</td>
                <td>{{ orderI.UnitPrice }}</td>
                <td>{{ orderI.Quantity }}</td>
                <td>
                  <a
                    class="btn btn-danger mb-1 mt-1"
                    :href="
                      'http://localhost:8000/api/delete-Product/' + orderI.id
                    "
                  >
                    Delete <i class="bi bi-trash"></i>
                  </a>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import axios from "axios";

export default {
  name: "OrderList",
  created() {
    this.getItems();
    this.getProducts();
  },
  components: {},
  data() {
    return {
      items: [],
      error: [],
      Supplier_id: 0,
      products: [],
    };
  },
  methods: {
    getProducts() {
      axios
        .get("http://localhost:8000/api/selectProduct")
        .then((response) => {
          console.log(response.data);
          this.products = response.data;
        })
        .catch((error) => {
          console.log(error);
          this.error = error;
        });
    },
    getItems() {
      axios
        .get("http://localhost:8000/api/list-Order")
        .then((response) => {
          console.log(response.data);
          this.items = response.data.Orders;
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
