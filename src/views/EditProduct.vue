<template>
  <div class="EditProduct">
    {{ Supplier_id }}

    <form @submit.prevent="submitForm">
      <div>
        <label for="ProductName">ProductName:</label><br />
        <input
          id="ProductName"
          type="text"
          :value="product.ProductName"
          required
        />
      </div>
      <div>
        <select name="Supplier" id="Supplier">
          <option value="1">SwafTech</option>
          <option value="2">Durra</option>
          <option value="3">كهربائيات المصري</option>
        </select>
      </div>
      <div>
        <label for="UnitPrice">UnitPrice:</label><br />
        <input id="UnitPrice" type="text" :value="product.UnitPrice" required />
      </div>
      <button class="btn btn-secondary" type="submit">Submit</button>
    </form>
  </div>
</template>

<script>
// @ is an alias to /src
import axios from "axios";

export default {
  name: "EditProduct",
  components: {},

  data() {
    return {
      error: [],
      product: [],
      ProductName: "",
      UnitPrice: "",
    };
  },
  props: ["Supplier", "productid"],
  created() {
    this.getProduct();
  },
  methods: {
    getProduct() {
      var url = "http://localhost:8000/api/edit-Product/" + this.productid;
      axios
        .get(url)
        .then((response) => {
          console.log(response.data);
          this.product = response.data.Product;
        })
        .catch((error) => {
          console.log(error);
          this.error = error;
        });
    },
    submitForm() {
      var PName = document.getElementById("ProductName").value;
      var UPrice = document.getElementById("UnitPrice").value;

      var url = "http://localhost:8000/api/update-Product/" + this.productid;
      axios
        .post(url, {
          ProductName: PName,
          UnitPrice: UPrice,
          SupplierId: this.Supplier,
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
