<template>
  <div>
    <h1>Product Form</h1>
    <form @submit.prevent="submitForm">
      <div class="form-group">
        <label>Name</label>
        <input v-model="product.name" class="form-control">
      </div>
      <div class="form-group">
        <label>Price</label>
        <input v-model="product.price" type="number" class="form-control">
      </div>
      <button type="submit" class="btn btn-primary">Submit</button>
    </form>
  </div>
</template>

<script>
// import axios from 'axios'

export default {
  data() {
    return {
      product: {
        name: '',
        price: 0
      }
    }
  },
  methods: {
    // submitForm() {
    //  axios.post('http://localhost:8000/products', this.product)
    //      .then(response => {
    //        console.log(response.data)
    //      })
    // }
  }
}
</script>

<style>
.form-control {
  display: block;
  width: 200px;
  padding: 0.375rem 0.75rem;
  font-size: 1rem;
  line-height: 1.5;
  color: #495057;
  background-color: #fff;
  background-clip: padding-box;
  border: 1px solid #ced4da;
  border-radius: 0.25rem;
  transition: border-color 0.15s ease-in-out, box-shadow 0.15s ease-in-out;
}

.btn-primary {
  color: #fff;
  background-color: #007bff;
  border-color: #007bff;
}
</style>