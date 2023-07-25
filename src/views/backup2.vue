<template>
  <section class="h-100 h-custom min-h-content" >
    <div class="container py-5 h-100">
      <div class="row d-flex justify-content-center align-items-center h-100">
        <div class="col">
          <div class="card border-0">
            <div class="card-body p-4">

              <div class="row">
                <div class="col-lg-7">
                  <h5 class="mb-3"><router-link :to="{name:'Home'}" class="text-body"><i
                      class="fas fa-long-arrow-alt-left me-2"></i>Volver atrás</router-link></h5>
                  <hr>

                  <div class="d-flex justify-content-between align-items-center mb-4">
                    <div>
                      <p class="mb-0">Usted tiene {{ $store.state.cart.length }} items en su factura.</p>
                    </div>
                  </div>

                  <div v-for="item in $store.state.cart" class="card mb-3 shadow-sm border-0" :key="item.id">
                    <div class="card-body">
                      <div class="d-flex justify-content-between">
                        <div class="d-flex flex-row align-items-center">
                          <div>
                            <img
                                :src="item.image"
                                class="img-fluid rounded-3" alt="Shopping item" style="width: 65px;">
                          </div>
                          <div class="ms-3">
                            <p>{{ item.name }}</p>
                          </div>
                        </div>
                        <div class="d-flex flex-row align-items-center">
                          <div >
                            <CartAddRemove :product="item"/>
                          </div>
                        </div>
                        <div class="d-flex flex-row align-items-center">
                          <div >
                            <h5 class="mb-0"><i class="bi bi-currency-dollar"></i>{{ item.price*item.qty }}</h5>
                            <small v-if="item.hasDiscount" class="text-muted text-decoration-line-through"><i class="bi bi-currency-dollar"></i>{{ item.price}}</small>
                          </div>
                          <a role="button" @click="removeItem(item)" class="ms-4" style="color: #cecece;"><i class="bi bi-trash3 h4"></i></a>
                        </div>
                      </div>
                    </div>
                  </div>

                </div>
                <div class="col-lg-5">

                  <div class="card bg-primary text-white rounded-0 border-0">
                    <div class="card-body">
                      <div class="d-flex justify-content-between align-items-center mb-4">
                        <h5 class="mb-0">Detalles de factura</h5>
                        <i class="bi bi-cart3 h1"></i>
                      </div>
                      <hr class="my-4">
                      <div class="d-flex justify-content-between">
                        <p class="mb-2">Subtotal</p>
                        <p class="mb-2"><i class="bi bi-currency-dollar"></i>{{ $store.state.cartTotal }}</p>
                      </div>
                      <div class="d-flex justify-content-between mb-4">
                        <p class="mb-2">Total</p>
                        <p class="mb-2"><i class="bi bi-currency-dollar"></i>{{ $store.state.cartTotal }}</p>
                      </div>

                      <div class="card">
                        <form @submit.prevent="submitForm">
                          <label for="name">Apellido:</label>
                          <input type="text" id="name" v-model="formData.data.nombreRazonSocial" />
                          <br>
                          <label for="nit">NIT:</label>
                          <input type="text" id="nit" v-model="formData.data.numeroDocumento" />
                          <br>
                          <button type="submit">Enviar</button>
                        </form>
                      </div>
                    </div>
                  </div>

                </div>

              </div>

            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>
<script>
import axios from 'axios';
import CartAddRemove from '../components/CartAddRemove.vue';
export default{
  components :{CartAddRemove},
  methods:{
    removeItem(item){
      this.$store.commit('addRemoveCart',{product:item,toAdd:false})
    },
  },
  mounted(){

  },
  data(){
    return {
      formData: {
        "ok": true,
        "messages": [],
        "data": {
          "cabecera": {
            "codigoRecepcion": null,
            "codigoEstado": null,
            "proveedorKey": "T1-N6985868019-S0-P0",
            "nitEmisor": 6985868019,
            "razonSocialEmisor": "KCStore",
            "municipio": "La Paz",
            "telefono": "465456",
            "numeroFactura": 137,
            "cuf": null,
            "cufd": "BQXxCPzfDiUdBNzTI3MTQxNUY2MUY=Q2VkWlFLQkhYVUFczQjFDMDkyMTc0O",
            "codigoSucursal": 0,
            "direccion": "AVENIDA 23 DE MARZO NRO.538 ZONA PAMPAHASI BAJO",
            "codigoPuntoVenta": 0,
            "fechaEmision": "2023-06-30T09:48:39.617",
            "nombreRazonSocial": "",
            "codigoTipoDocumentoIdentidad": 1,
            "numeroDocumento": "",
            "complemento": null,
            "codigoCliente": "Poppe",
            "modalidadServicio": null,
            "codigoMetodoPago": 1,
            "numeroTarjeta": null,
            "montoTotal": 100,
            "montoTotalSujetoIva": 100,
            "codigoMoneda": 150,
            "tipoCambio": 1,
            "montoTotalMoneda": 100,
            "montoGiftCard": null,
            "descuentoAdicional": null,
            "codigoExcepcion": null,
            "cafc": null,
            "leyenda": "Ley N° 453: Tienes derecho a recibir información sobre las características y contenidos de los routers que utilices.",
            "usuario": "pperez",
            "codigoDocumentoSector": 17
          },
          "detalles": [
            {
              "facturaKey": null,
              "actividadEconomica": 749000,
              "codigoProductoSin": 83111,
              "codigoProducto": "P1",
              "descripcion": "Tomografia",
              "especialidad": null,
              "especialidadDetalle": null,
              "nroQuirofanoSalaOperaciones": 1,
              "especialidadMedico": null,
              "nombreApellidoMedico": "Salazar",
              "nitDocumentoMedico": 51151889019,
              "nroMatriculaMedico": null,
              "nroFacturaMedico": 1,
              "cantidad": 1,
              "unidadMedida": 58,
              "precioUnitario": 100,
              "montoDescuento": 0,
              "subTotal": 100
            }
          ]
        }
      }
    };
  },
  methods: {
    submitForm: function () {
      const config = { headers:{ accept: application/json } }
      const jsonData = JSON.stringify(this.formData);

    }
  }
}
</script>

<style scoped>
.card {
  background-color: #fff;
  border-radius: 4px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  padding: 16px;
}
h2 {
  margin-top: 0;
}
form {
  display: flex;
  flex-direction: column;
}
label {
  margin-bottom: 8px;
}
input,
button {
  padding: 8px;
  margin-bottom: 16px;
  border-radius: 4px;
  border: 1px solid #ccc;
}
button {
  background-color: #007bff;
  color: #fff;
  border: none;
}
</style>