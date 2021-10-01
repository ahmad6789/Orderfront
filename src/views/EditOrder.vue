<template>
  <div class="EditProduct">
    <form @submit.prevent="submitForm">
      <div>
        <label for="CustomerId">CustomerId:</label><br />
        <input :value="Order.CustomerId" id="CustomerId" type="text" required />
      </div>
      <div>
        <label for="TotalAmount">TotalAmount:</label><br />
        <input
          :value="Order.TotalAmount"
          id="TotalAmount"
          type="text"
          required
        />
      </div>
  
      <button class="btn btn-secondary" type="submit">Submit</button>
     
    </form>
    <table class="table table-striped">
      <thead>
        <tr>
          <th scope="col">#</th>
          <th scope="col">Product Id</th>
          <th scope="col">UnitPrice</th>
          <th scope="col">Quantity</th>
          <th scope="col">Action</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(orderI, i) in Order.order_item" :key="i">
          <th scope="row">{{ orderI.id }}</th>
          <td>{{ orderI.ProductId }}</td>
          <td>{{ orderI.UnitPrice }}</td>
          <td>{{ orderI.Quantity }}</td>
          <td>
            <router-link
              :to="{
                name: 'EditOrderItem',
                params: {
                  orderId: orderI.id,
                },
              }"
              class="btn btn-success mb-1 mt-1"
              name="sup_id"
              >Edit<i class="bi bi-pencil-square"></i
            ></router-link>
            &nbsp;<a
              class="btn btn-danger mb-1 mt-1"
              :href="'http://localhost:8000/api/delete-OrderItem/' + orderI.id"
            >
              Delete <i class="bi bi-trash"></i>
            </a>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
// @ is an alias to /src
import axios from "axios";

export default {
  name: "EditOrderItem",
  components: {},
  data() {
    return {
      error: [],
      Order: [],
      inddex: "",
    };
  },
  props: ["orderId"],
  created() {
    this.getOrder();
  },
  methods: {
    getOrder() {
      var url = "http://localhost:8000/api/edit-Order/" + this.orderId;
      axios
        .get(url)
        .then((response) => {
          console.log(response.data);
          this.Order = response.data.Order;
        })
        .catch((error) => {
          console.log(error);
          this.error = error;
        });
    },
    submitForm() {
      var CustomerId = document.getElementById("CustomerId").value;
      var TotalAmount = document.getElementById("TotalAmount").value;
      var url = "http://localhost:8000/api/update-Order/" + this.orderId;
      axios
        .post(url, {
          OrderDate: this.Order.OrderDate,
          OrderNumber: this.Order.OrderNumber,
          CustomerId: CustomerId,
          TotalAmount: TotalAmount,
        })
        .then((response) => {
          console.log(response.data);
          this.items = response.data.Supplier;
        })
        .catch((error) => {
          console.log(error);
          this.error = error;
        });

      /*for (let i = 1; i < 2; i++) {
         var url2 = "http://localhost:8000/api/update-OrderItem/" + this.Order.order_item.id;
      axios
        .post(url2, {
    OrderId: this.Order.order_item.id,
    ProductId:ProductId,
    UnitPrice:UnitPrice,
    Quantity:Quantity,
          
        })
        .then((response) => {
          console.log(response.data);
          this.items = response.data.Supplier;
        })
        .catch((error) => {
          console.log(error);
          this.error = error;
        });
}*/
    },
  },
};
</script>

<style scoped lang="scss">
$primary: #5968d7;

form {
  width: 500px;
  padding: 10px 40px;
  label {
    text-transform: uppercase;
    font-size: 13px;
    letter-spacing: 0.03em;
    font-weight: bold;
  }
  input,
  textarea {
    border: 1px solid #ccc;
    color: #333;
    width: calc(100% - 30px);
  }
  input,
  textarea,
  button {
    border-radius: 4px;
    padding: 8px 15px;
    font-family: "Work Sans", sans-serif;
    font-weight: 300;
  }
  div {
    margin: 20px 0;
  }
}

button {
  color: white;
  border: none;
  width: calc(100% - 30px);
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.08em;
  background: #ccc;
  cursor: pointer;
  box-shadow: 0px 2px 3px rgba(0, 0, 0, 0.3);
  transition: 0.25s all ease;
  &:hover {
    transform: translateY(2px);
  }
}

.active {
  background: $primary;
}

pre-content {
  width: 500px;
}
</style>
