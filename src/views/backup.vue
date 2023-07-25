<template>
  <div class="container min-h-content py-5 text-center">
    <div class="row py-lg-5">
      <div class="row row-cols-1 row-cols-sm-2 row-cols-md-4 g-3">
        <div class="col" v-for="product in products"  :key="product.id">
          <div class="card shadow-sm">
            <img class="bd-placeholder-img card-img-top" width="100%"  :src="product.image" alt="">
            <div class="card-body">
              <p class="card-text">{{ product.name }}</p>
              <div class="d-flex justify-content-between align-items-center">
                <div class="btn-group">
                  <CartBTN :product="product"/>
                </div>
                <small class="text-muted"><i class="bi bi-currency-dollar"></i>{{ product.price }}</small>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
  <div class="card">
    <div class="text-center">
      <Boton class="left-button" @click="fecthDatos">Verificar</Boton>
    </div>
  </div>
  <div>
    <Boton button-text="Verificar estado servicio de facturación" @click="fetchDatos"></Boton>
  </div>
  <div>
    <Boton  button-text="Obtener CUFD" @click="fetchCufd"></Boton>
  </div>
</template>
<script>
import CartBTN from '../components/CartBTN.vue'
import Button from '../components/Button.vue'
import Boton from '../components/Boton.vue'
import axios from 'axios';
import { toast } from 'vue3-toastify';
import 'vue3-toastify/dist/index.css';
export default {

  setup() {

  },
  components :{Boton, Button, CartBTN},
  data(){
    return{
      products:[
        {id:1, name:'Tomografía',image:'', price:250},
        {id:2, name:'Radiografía',image:'', price:500},
        {id:3, name:'Arteriografía',image:'', price:800},
        {id:4, name:'Examen Médico',image:'', price:200}
      ],
      responseData: null
    }
  },
  methods: {
    fetchDatos() {
      const url = 'https://py-kc-rest-v1-xkqvciodha-rj.a.run.app/oper/facturacion?codigoSector=1';
      const headers = { 'accept': 'application/json' };
      axios.get(url, { headers })
          .then(response => {
            this.responseData = response.data;
            if (this.responseData.ok) {
              toast("Comunicación con el servicio de facturación electrónica disponible")
            }
          })
          .catch(error => {
            console.error(error);
          });
    }
  }
}
</script>

<style>
.card {
  background-color: #f2f2f2;
  padding: 10px;
  border-radius: 4px;
  display: flex;
  justify-content: flex-start;
  align-items: center;
}

.left-button {
  margin-right: 10px;
}
</style>