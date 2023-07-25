<template>
  <div class="card-container">
    <div v-for="(card, index) in cards" :key="index" class="card">
      <h3>{{ card.title }}</h3>
      <p>{{ card.content }}</p>
      <button @click="card.buttonAction">{{ card.buttonText }}</button>
    </div>
  </div>
  <div class="container mt-5">
    <div class="card">
      <div class="card-body">
        <h5 class="card-title">Facturar</h5>
        <div class="mb-3">
          <label for="razonsocial" class="form-label">Razón Social</label>
          <input type="text" class="form-control" id="razonsocial" v-model="formData.razonsocial">
        </div>
        <div class="mb-3">
          <label for="nrodocumento" class="form-label">Numero Documento</label>
          <input type="text" class="form-control" id="nrodocumento" v-model="formData.nrodocumento">
        </div>
        <div class="mb-3">
          <label for="numerofactura" class="form-label">Numero Factura</label>
          <input type="number" class="form-control" id="numerofactura" v-model="formData.numerofactura">
        </div>
        <div class="mb-3">
          <label for="email" class="form-label">Email</label>
          <input type="email" class="form-control" id="email" v-model="formData.email">
        </div>
        <div class="mb-3">
          <label for="product" class="form-label">Producto</label>
          <input type="text" class="form-control" id="product" v-model="formData.product">
        </div>
        <div class="mb-3">
          <label for="cantidad" class="form-label">Cantidad</label>
          <input type="number" class="form-control" id="cantidad" v-model="formData.cantidad">
        </div>
        <div class="mb-3">
          <label for="subtotal" class="form-label">SubTotal</label>
          <input type="number" class="form-control" id="subtotal" v-model="formData.subtotal">
        </div>
        <div class="mb-3">
          <label for="total" class="form-label">Total</label>
          <input type="number" class="form-control" id="total" v-model="formData.total">
        </div>
        <div class="mb-3">
          <label for="descuento" class="form-label">Descuento</label>
          <input type="number" class="form-control" id="descuento" v-model="formData.descuento">
        </div>
      </div>
    </div>

    <div class="mt-3">
      <button @click="submitForm" class="btn btn-primary">Enviar</button>
      <button @click="cleanForm" class="btn btn-primary">Limpiar</button>
    </div>
  </div>


  <div class="container mt-5">
    <div class="card">
      <div class="card-body">
        <h5 class="card-title">ANULAR</h5>
        <div class="mb-3">
          <label for="anular" class="form-label">Número de factura a anular</label>
          <input type="text" class="form-control" id="anular" v-model="nroanular">
        </div>
      </div>
    </div>

    <div class="mt-3">
      <button @click="anularFac" class="btn btn-primary">Anular</button>
    </div>
  </div>
  <div>
    <!-- Display the received JSON data -->
    <pre>{{ JSON.stringify(receivedData2, null, 2) }}</pre>
  </div>

</template>

<script>
import CartBTN from '../components/CartBTN.vue';
import axios from 'axios';
import { toast } from 'vue3-toastify';
import 'vue3-toastify/dist/index.css';
import {setDevtoolsHook} from "vue";
export default {
  components :{CartBTN},
  data() {
    return {
      nroanular: '',
      receivedData: null,
      receivedData2: null,
      formData: {
        razonsocial: '',
        nrodocumento: '',
        numerofactura: null,
        email: '',
        product: '',
        cantidad: null,
        subtotal: null,
        total: null,
        descuento: null
      },
      cards: [
        {
          title: "Obtener CUIS",
          content: "Renovar el CUIS para facturación",
          buttonText: "Renovar el CUIS",
          buttonAction: this.handleButton1Click
        },
        {
          title: "Obtener CUFD",
          content: "Renovar el CUFD para facturación",
          buttonText: "Obtener CUFD",
          buttonAction: this.handleButton2Click
        },
        {
          title: "Facturacion online",
          content: "Pasar a modo de facturación con conexión al SIN",
          buttonText: "Facturación Online",
          buttonAction: this.handleButton3Click
        },
        {
          title: "Facturación offline",
          content: "Pasar a modo de facturación sin conexión al SIN",
          buttonText: "Facturación Offline",
          buttonAction: this.handleButton3AClick
        },
        {
          title: "Verificar Comunicación",
          content: "Verificar conexión con el SFE del SIN",
          buttonText: "Verificar",
          buttonAction: this.handleButton4Click
        },
      ],
      predefinedData: {
        "cabecera": {
          "codigoRecepcion": null,
          "codigoEstado": null,
          "proveedorKey": "T1-N392010028-S0-P0",
          "nitEmisor": 392010028,
          "razonSocialEmisor": "CLINICA MEDICMEL S.R.L.",
          "municipio": "Santa Cruz",
          "telefono": "465456",
          "numeroFactura": null,
          "cuf": null,
          "cufd": "BQT5DS1VuQUE=NzjI2QjU5RjlBNzY=QnwrZDdLWkhYVUFc0NzhFNDIzM0YwR",
          "codigoSucursal": 0,
          "direccion": "AVENIDA DOBLE VIALA GUARDIA NRO.SN ZONA KM 9 DOBLE VIA A LA GUARDIA UV 0 MZA 0",
          "codigoPuntoVenta": 0,
          "fechaEmision": "2023-07-24T12:00:10.236",
          "nombreRazonSocial": "",
          "codigoTipoDocumentoIdentidad": 1,
          "numeroDocumento": "",
          "complemento": "",
          "codigoCliente": "",
          "codigoMetodoPago": 1,
          "numeroTarjeta": "0",
          "montoTotal": null,
          "montoTotalSujetoIva": null,
          "codigoMoneda": 150,
          "tipoCambio": 1,
          "montoTotalMoneda": null,
          "montoGiftCard": 0,
          "descuentoAdicional": null,
          "codigoExcepcion": 0,
          "cafc": null,
          "leyenda": "Ley N° 453: Los servicios deben suministrarse en condiciones de inocuidad, calidad y seguridad.",
          "usuario": "pperez",
          "codigoDocumentoSector": 1
        },
        "detalles": [
          {
            "facturaKey": null,
            "actividadEconomica": 477311,
            "codigoProductoSin": 35210,
            "codigoProducto": "P1",
            "descripcion": "",
            "cantidad": null,
            "unidadMedida": 58,
            "precioUnitario": 2.5,
            "montoDescuento": null,
            "subTotal": null,
            "numeroSerie": "",
            "numeroImei": ""
          }
        ]
      }
    };
  },
  methods: {
    handleButton1Click() {
      const url = 'https://py-kc-rest-v1-xkqvciodha-rj.a.run.app/admin/proveedores/cuis/actualizar?proveedorKey=T1-N392010028-S0-P0';
      const headers = { 'accept': 'application/json' };
      let toastMSG;
      let cuis;
      axios.put(url, null,{headers},)
          .then(response => {
            this.responseData = response.data;
            if (this.responseData.ok == true) {
              cuis = this.responseData.data.cuis.codigo;
              toastMSG = 'El código CUIS está vigente.\n CUIS: ' + cuis;

            } else {
              toastMSG = 'No se pudo renovar el codigo CUIS'
            };
            toast(toastMSG, {
              autoClose: 3500,
            });
          })
          .catch(error => {
            console.error(error);
          });

    },
    handleButton2Click() {
      const url = 'https://py-kc-rest-v1-xkqvciodha-rj.a.run.app/admin/proveedores/cufd/actualizar?proveedorKey=T1-N392010028-S0-P0';
      const headers = { 'accept': 'application/json' };
      let toastMSG;
      let cufd;
      axios.put(url, null,{headers},)
          .then(response => {
            this.responseData = response.data;
            if (this.responseData.ok == true) {
              cufd = this.responseData.data.cufd.codigo;
              toastMSG = 'El código CUFD está vigente.\n CUFD: ' + cufd;

            } else {
              toastMSG = 'No se pudo renovar el codigo CUFD'
            };
            toast(toastMSG, {
              autoClose: 3500,
            });
          })
          .catch(error => {
            console.error(error);
          });

    },
    handleButton3Click() {
      const url = 'https://py-kc-rest-v1-xkqvciodha-rj.a.run.app/oper/facturacion/on_line?proveedorKey=T1-N392010028-S0-P0';
      const headers = { 'accept': 'application/json' };
      let toastMSG;
      axios.put(url, null,{headers},)
          .then(response => {
            this.responseData = response.data;
            if (this.responseData.messages == "Ya estaba en modo online") {
              toastMSG = 'Ya estaba en modo online'
            } else {
              if (this.responseData.ok == true) {
                toastMSG = 'Se pasó a modo online'
              } else {
                toastMSG = 'No se pudo pasar a modo online'
              }
            };
            toast(toastMSG, {
              autoClose: 2000,
            });
          })

          .catch(error => {
            console.error(error);
          });

    },
    handleButton3AClick() {
      const url = 'https://py-kc-rest-v1-xkqvciodha-rj.a.run.app/oper/facturacion/off_line';
      const headers = { 'accept': 'application/json', 'Content-Type': 'application/json' };
      let toastMSG;
      axios.put(url, { proveedorKey: 'T1-N392010028-S0-P0', codigoMotivoEvento: 1, descripcion: 'Internet bajo' }, { headers },)
          .then(response => {
            this.responseData = response.data;
            if (this.responseData.messages == "Ya estaba en modo offline") {
              toastMSG = 'Ya estaba en modo offline'
            } else {
              if (this.responseData.ok == true) {
                toastMSG = 'Se pasó a modo offline por falla de Internet'
              } else {
                toastMSG = 'No se pudo pasar a modo offline'
              }
            };
            toast(toastMSG, {
              autoClose: 2000,
            });
          })

          .catch(error => {
            console.error(error);
          });
    },
    handleButton4Click() {
      const url = 'https://py-kc-rest-v1-xkqvciodha-rj.a.run.app/oper/facturacion?codigoSector=1';
      const headers = { 'accept': 'application/json' };
      let toastMSG;
      axios.get(url, { headers })
          .then(response => {
            this.responseData = response.data;
            this.responseData.ok?  toastMSG = 'Si existe comunicación' :  toastMSG = 'No existe comunicación'
            toast(toastMSG, {
              autoClose: 2000,
            });
          })

          .catch(error => {
            console.error(error);
          });
    },
    anularFac() {
      const url = 'https://py-kc-rest-v1-xkqvciodha-rj.a.run.app/oper/facturacion/anular';
      const headers = { 'accept': 'application/json', 'Content-Type': 'application/json' };
      let toastMSG;
      axios.put(url, { gestion : 2023, periodo: 7, proveedorKey : "T1-N392010028-S0-P0",  codigoSector: 1,  numeroFactura: this.nroanular, codigoMotivo: 1 }, { headers })
          .then(response => {
            this.responseData = response.data;
            this.responseData.ok?  toastMSG = 'Se anuló la factura.' :  toastMSG = 'No se pudo anular la factura.'
            toast(toastMSG, {
              autoClose: 2000,
            });
          })

          .catch(error => {
            console.error(error);
          });
    },
    cleanForm() {
      this.formData.razonsocial = '',
      this.formData.nrodocumento = '',
      this.formData.nrofactura = null,
      this.formData.email = '',
      this.formData.product = '',
      this.formData.cantidad = null,
      this.formData.subtotal = null,
      this.formData.total = null,
      this.formData.descuento = null
    },
    submitForm() {
      const url = 'https://py-kc-rest-v1-xkqvciodha-rj.a.run.app/oper/facturacion/crear?proveedorKey=T1-N392010028-S0-P0&codigoSector=1';
      const headers = { 'accept': 'application/json' };
      let toastMSG;
      axios.put(url, null, { headers })
          .then(response => {
            this.receivedData = response.data;
            this.receivedData.ok?  toastMSG = 'Se recibió la factura.' :  toastMSG = 'No se recibió la factura.'
            toast(toastMSG, {
              autoClose: 2000,
            });
            if (this.responseData.data.cabecera.codigoRecepcion !== 908) {
              console.log(response.data.msg());
              toast("Factura procesada exitósamente!", {
                autoClose: 3000,
              });
            }
          })

          .catch(error => {
            console.error(error);
          });
      this.receivedData.data.cabecera.nombreRazonSocial = this.formData.razonsocial;
      this.receivedData.data.cabecera.nrodocumento = this.formData.nrodocumento;
      this.receivedData.data.cabecera.codigoCliente = this.formData.email;
      this.receivedData.data.cabecera.montoTotal = this.formData.total;
      this.receivedData.data.cabecera.montoTotalSujetoIva = this.formData.total;
      this.receivedData.data.cabecera.montoTotalMoneda = this.formData.total;
      this.receivedData.data.cabecera.descuentoAdicional = this.formData.descuento;
      this.receivedData.data.cabecera.numeroFactura = this.formData.numerofactura;
      this.receivedData.data.detalles.cantidad = this.formData.cantidad;
      this.receivedData.data.cabecera.subtotal = this.formData.subtotal;
      this.receivedData.data.detalles.montoDescuento = this.formData.descuento;
      const url2 = 'https://py-kc-rest-v1-xkqvciodha-rj.a.run.app/oper/facturacion/emitir?codigoSector=1';
      const headers2 = { 'accept': 'application/json', 'Content-Type': 'application/json' };
      let toastMSG2;
      axios.post(url2, this.receivedData, { headers2 })
          .then(responser => {
            this.receivedData2 = responser.data;
            this.receivedData2.ok? toastMSG2 = "Se recibio la factura procesada" : toastMSG2 = "No se recibio la factura."
            toast(toastMSG2, {
              autoClose: 3000,
            });
          });
    }
  }
};
</script>

<style>
.card-container {
  display: flex;
  justify-content: center;
}

.card {
  background-color: #f2f2f2;
  padding: 10px;
  border-radius: 4px;
  margin: 10px;
  text-align: center;
  max-width: 300px; /* Set a maximum width for the card */
}
</style>
<script setup>
</script>