<template>
  <div class="col-md-10 offset-1">
    <button class="btn btn-primary" v-on:click="getAllProducts">Refresh</button>
    <table class="table table-striped">
      <thead>
      <tr>
        <th scope="col">id</th>
        <th scope="col">Product id</th>
        <th scope="col">Name</th>
        <th scope="col">TypeID</th>
        <th scope="col">Cost</th>
        <th scope="col">Address</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="(item, index) in items" :key="index">
        <td>{{ index + 1 }}</td>
        <td id="itemId">{{ item[0] }}</td>
        <td>{{ item[4] }}</td>
        <td>{{ item[5] }}</td>
        <td>{{ item[2] }}</td>
        <td>{{ item[3] }}</td>
        <td>
          <button class="btn btn-warning" data-bs-toggle="modal" data-bs-target="#updateProduct"
                  v-on:click="getProductUpdate(item[0])">Update
          </button>
        </td>
        <td>
          <button class="btn btn-danger" v-on:click="deleteProduct(item[0])"> Delete</button>
        </td>
      </tr>
      </tbody>
    </table>
  </div>

  <div class="modal " id="updateProduct" data-bs-backdrop="static" data-bs-keyboard="false" tabindex="-1"
       aria-labelledby="addProductLabel" aria-hidden="true">
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title" id="addProductLabel">Modal title</h5>
          <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
        </div>
        <div class="modal-body">
          <div class="mb-3">
            <label for="productName" class="form-label">Product name</label>
            <input type="text" id="productName" class="form-control" placeholder="Enter product name: " v-bind:value="item.name_uz">
          </div>
          <div class="mb-3">
            <label for="productType" class="form-label">Product type</label>
            <select id="productType" class="form-select">
              <option v-for="(ty, index) in types" :key="index" v-bind:value="ty.id">{{ ty.name_uz }}</option>
            </select>
          </div>
          <div class="mb-3">
            <label for="productCost" class="form-label">Product Cost</label>
            <input type="number" id="productCost" class="form-control" placeholder="Enter product Cost: " v-bind:value="item.cost">
          </div>
          <div class="mb-3">
            <label for="productAddress" class="form-label">Product Address</label>
            <input type="text" id="productAddress" class="form-control" placeholder="Enter product Address: " v-bind:value="item.address">
          </div>
        </div>
        <div class="modal-footer">
          <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
          <button type="button" class="btn btn-primary" v-on:click="updateProduct(item.id)" data-bs-dismiss="modal">Update
            product
          </button>
        </div>
      </div>
    </div>
  </div>
</template>


<script>
import axios from "axios";

export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: 'Products',
  data() {
    return {
      items: [],
      types: [],
      item: ''
    }
  },
  props: {},
  methods: {
    updateProduct(index) {
      let selectedOption = document.querySelector('select').value;
      console.log(selectedOption);
      let name_uz = document.getElementById('productName').value
      let cost = document.getElementById('productCost').value
      let address = document.getElementById('productAddress').value
      let type_id = selectedOption
      console.log(type_id)
      const requestOptions = {
        method: 'PUT',
        headers: {
          'Content-Type': 'application/json'// replace with your authorization token
        },
        body: JSON.stringify({
          id: index,
          name_uz: name_uz,
          cost: cost,
          address: address,
          productTypeId: type_id
        })

      };
      fetch('http://localhost:8080/crud_app_jersey-1/crudApi/crud/update', requestOptions)
          .then(response => response.json())
          .then(data => console.log(data))
          .then(this.getAllProducts)
          .catch(error => console.error(error));

    },

    getAllProducts() {
      axios.get('http://localhost:8080/crud_app_jersey-1/crudApi/crud/getAll')
          .then(response => {
            this.items = response.data
          })
          .catch(error => {
            console.log(error)
          })
    },
    deleteProduct(index) {
      axios.delete('http://localhost:8080/crud_app_jersey-1/crudApi/crud/'+index)
          .then(response => {
            console.log(response)
          }).then(this.getAllProducts)
          .catch(error => {
            console.log(error)
          })
    },
    getProduct(index) {
      axios.get('http://localhost:8080/crud_app_jersey-1/crudApi/crud/'+index,)
          .then(response => {
            this.item = response.data
            console.log(this.item)
          })
          .catch(error => {
            console.log(error)
          })
    },
    getProductUpdate(index) {
      this.getTypes()
      this.getProduct(index)
    },
    getTypes() {
      axios.get('http://localhost:8080/crud_app_jersey-1/crudApi/crud/productTypes')
          .then(response => {
            this.types = response.data
          })
          .catch(error => {
            console.log(error)
          })
    },


  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
