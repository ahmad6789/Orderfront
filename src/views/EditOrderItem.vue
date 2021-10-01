<template>
  <div class="EditProduct">
    <p>{{ Order.orderId }}</p>

    <form @submit.prevent="submitForm">
      <div>
        <label for="ProductId">ProductId:</label><br />
        <input :value="Order.ProductId" id="ProductId" type="text" required />
      </div>
      <div>
        <label for="Quantity">Quantity:</label><br />
        <input :value="Order.Quantity" id="Quantity" type="text" required />
      </div>
      <div>
        <label for="UnitPrice">UnitPrice:</label><br />
        <input :value="Order.UnitPrice" id="UnitPrice" type="text" required />
      </div>
      <button class="btn btn-secondary" type="submit">Submit</button>
    </form>
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
      var url = "http://localhost:8000/api/edit-OrderItem/" + this.orderId;
      axios
        .get(url)
        .then((response) => {
          console.log(response.data);
          this.Order = response.data.OrderItem;
        })
        .catch((error) => {
          console.log(error);
          this.error = error;
        });
    },
    submitForm() {
      var ProductId = document.getElementById("ProductId").value;
      var UnitPrice = document.getElementById("UnitPrice").value;
      var Quantity = document.getElementById("Quantity").value;

      var url2 = "http://localhost:8000/api/update-OrderItem/" + this.Order.id;
      axios
        .post(url2, {
          OrderId: this.Order.id,
          ProductId: ProductId,
          UnitPrice: UnitPrice,
          Quantity: Quantity,
        })
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
