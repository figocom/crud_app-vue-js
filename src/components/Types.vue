<template>
  <div>

    <button type="button" class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#addType"
            v-on:click="getTypes">
      Add Product Type
    </button>
    <button type="button" class="btn btn-primary offset-1" data-bs-toggle="modal" data-bs-target="#addProduct"
            v-on:click="getTypes">
      Add Product
    </button>
    <div class="modal " id="addType" data-bs-backdrop="static" data-bs-keyboard="false" tabindex="-1"
         aria-labelledby="addTypeLabel" aria-hidden="true">
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="addTypeLabel">All types</h5>
            <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
          </div>
          <div class="modal-body">
            <ul>
              <li v-for="(item, index) in items" :key="index">
                <var> {{ item.name_uz }}</var>
              </li>
            </ul>
            <div class="input-group mb-3">
              <input type="text" class="form-control" id="name_uz" placeholder="Enter type name" aria-label="Typename"
                     aria-describedby="button-addon2">
              <button class="btn btn-outline-primary" data-bs-dismiss="modal" type="button" id="button-addon2"
                      v-on:click="saveType()">Add
              </button>
            </div>
          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
          </div>
        </div>
      </div>
    </div>
    <div class="modal " id="addProduct" data-bs-backdrop="static" data-bs-keyboard="false" tabindex="-1"
         aria-labelledby="addProductLabel" aria-hidden="true">
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="addProductLabel">Modal title</h5>
            <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
          </div>
          <div class="modal-body">
            <div class="mb-3">
              <label for="c_productName" class="form-label">Product name</label>
              <input type="text" id="c_productName" class="form-control" placeholder="Enter product name: ">
            </div>
            <div class="mb-3">
              <label for="c_productType" class="form-label">Product type</label>
              <select id="c_productType" class="form-select">
                <option v-for="(item, index) in items" :key="index" v-bind:value="item.id">{{ item.name_uz }}</option>
              </select>
            </div>
            <div class="mb-3">
              <label for="c_productCost" class="form-label">Product Cost</label>
              <input type="number" id="c_productCost" class="form-control" placeholder="Enter product Cost: ">
            </div>
            <div class="mb-3">
              <label for="c_productAddress" class="form-label">Product Address</label>
              <input type="text" id="c_productAddress" class="form-control" placeholder="Enter product Address: ">
            </div>
          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-secondary" data-bs-dismiss="modal" >Close</button>
            <button type="button" class="btn btn-primary" v-on:click="saveProduct"  data-bs-dismiss="modal">Save product</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>

import axios from 'axios'


export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: 'Types',
  data() {
    return {
      items: []
    }
  },
  methods: {
    saveType() {
      let name_uz = document.getElementById('name_uz').value
      axios.post('http://localhost:8080/crud_app_jersey-1/crudApi/crud/addType?name_uz=' + name_uz)
          .then(response => {
            console.log(response)
          })
          .catch(error => {
            console.log(error)
          })
    },
    saveProduct() {
      let selectedOption = document.getElementById('c_productType').value
      console.log(selectedOption);
      let name_uz = document.getElementById('c_productName').value
      let cost = document.getElementById('c_productCost').value
      let address = document.getElementById('c_productAddress').value
      let type_id = selectedOption
      const requestOptions = {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'// replace with your authorization token
        },
        body: JSON.stringify({
          name_uz: name_uz,
          cost: cost,
          address: address,
          type_id: type_id
        })

      };
      console.log(requestOptions)
      fetch('http://localhost:8080/crud_app_jersey-1/crudApi/crud/addProduct', requestOptions)
          .then(response => response.json())
          .then(data => console.log(data))
          .catch(error => console.error(error));

    },
    getTypes() {
      axios.get('http://localhost:8080/crud_app_jersey-1/crudApi/crud/productTypes')
          .then(response => {
            this.items = response.data
          })
          .catch(error => {
            console.log(error)
          })
    }


  },


}
</script>
