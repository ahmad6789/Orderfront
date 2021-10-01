<template>
  <div class="AddOrder">
    {{ Supplier_id }}

    <button class="btn btn-success" @click="addrow">addrow</button>
    <form @submit.prevent="submitForm">
      <div>
        <div>
          <label for="OrderNumber">OrderNumber:</label><br />
          <input id="OrderNumber" type="text" required />
        </div>
        <label for="CustomerId">CustomerId:</label><br />
        <input id="CustomerId" type="text" required />
        <label for="TotalAmount">TotalAmount:</label><br />
        <input id="TotalAmount" type="text" required />
      </div>
      <div>
        <div v-for="(row, index) in rows" :key="index">
          <div>
            <label for="ProductId">ProductId:</label><br />
            <input :id="'ProductId' + index" type="text" required />
          </div>
          <div>
            <label for="Quantity">Quantity:</label><br />
            <input :id="'Quantity' + index" type="text" required />
          </div>

          <div>
            <label for="UnitPrice">UnitPrice:</label><br />
            <input :id="'UnitPrice' + index" type="text" required />
          </div>

          <div>
            <label for="OrderId">OrderId:</label><br />
            <input :id="'OrderId' + index" type="text" required />
          </div>
        </div>
      </div>
      <button class="btn btn-secondary" type="submit">Submit</button>
    </form>
  </div>
</template>

<script>
// @ is an alias to /src
import axios from "axios";

export default {
  name: "AddOrder",
  components: {},
  data() {
    return {
      ProductName: "",
      UnitPrice: "",
      error: [],
      rows: 1,
    };
  },
  methods: {
    addrow() {
      this.rows = this.rows + 1;
    },
    submitForm() {
      var CustomerId = document.getElementById("CustomerId").value;
      var TotalAmount = document.getElementById("TotalAmount").value;
      var OrderNumber = document.getElementById("OrderNumber").value;

      var url = "http://localhost:8000/api/create-Order/";
      axios
        .post(url, {
          OrderNumber: OrderNumber,
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
      var url2 = "http://localhost:8000/api/create-OrderItem/";
      for (let i = 0; i < this.rows; i++) {
        var X = "OrderId" + i;
        var Y = "ProductId" + i;
        var Z = "UnitPrice" + i;
        var H = "Quantity" + i;

        var OrderId = document.getElementById(X).value;
        var ProductId = document.getElementById(Y).value;
        var UnitPrice = document.getElementById(Z).value;
        var Quantity = document.getElementById(H).value;

        axios
          .post(url2, {
            OrderId: OrderId,
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
      }
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
