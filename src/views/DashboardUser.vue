<template>
  <div>
      <section id="dashboard">
          <h2 class="text-center text-gray h4 p-5">Bienvenido. Usuario <br> Escoge tus prendas</h2>
      </section>

      <div class="container">
          <div class="row">
              <div class="col-md-6 col-sm-12 col-lg-6 col-xl-6 col-xxl-6 mt-3" v-for="cart in cartProducts" :key="cart.id">
                <div class="card shadow-lg">
                    <img :src="cart.imagen" :alt="cart.imagen" class="card-img-top img-fluid">

                    <div class="card-body">
                        <h3 class="card-title text-muted">
                            {{ cart.nombre }}
                        </h3>

                        <h4 class="card-text text-muted mt-4 text-uppercase">
                            {{ cart.nombreTalla }}
                        </h4>

                        <div class="d-flex justify-content-around align-items-center mt-4">
                            <button class="btn btn-dark">
                                XS
                            </button>

                            <button class="btn btn-light">
                                S
                            </button>

                            <button class="btn btn-light">
                                M
                            </button>

                            <button class="btn btn-light">
                                L
                            </button>

                            <button class="btn btn-light">
                                XL
                            </button>
                        </div>
                    </div>
                </div>
              </div>
          </div>
      </div>

      <div class="text-center mt-6">
          <p class="quantity text-muted">{{ countProductsCart }} / {{ countProductsCart }}</p>

          <button class="btn btn-outline-dark" data-bs-toggle="modal" data-bs-target="#search">
              Escanea tus productos
          </button>
      </div>

      <div class="text-center mt-4">
          <router-link to="/probador" class="btn btn-outline-dark btn-probador">
              Ir a probador
          </router-link>
      </div>

      <div class="text-center mt-4">
          <button class="btn btn-outline-dark text-uppercase">Limpiar carrito</button>
      </div>

      <div class="text-center mt-7">
          <h2 class="text-muted mouse" data-bs-toggle="modal" data-bs-target="#delete-session">Cerrar Sesión</h2>
      </div>

      <footer id="footer">

      </footer>

      <div
      class="modal fade"
      id="search"
      tabindex="-1"
      aria-labelledby="exampleModalLabel"
      aria-hidden="true"
    >
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="exampleModalLabel">Buscar producto</h5>
            <button
              type="button"
              class="btn-close"
              data-bs-dismiss="modal"
              aria-label="Close"
            ></button>
          </div>
          <div class="modal-body">
              <form @submit.prevent="searchProduct">
                  <input type="text" class="form-control" placeholder="Ingresar Código SKU..." v-model="sku">

                  <input type="submit" value="Agregar a carrito" class="btn btn-dark mt-3">
              </form>
          </div>
        </div>
      </div>
    </div>

      <DeleteSession @delete-session="logout" />
  </div>
</template>

<script>
import DeleteSession from '../components/modal/DeleteSession.vue'
import axios from 'axios'
import Swal from 'sweetalert2'

export default {
    name: 'Dashboard',
    components: {
        DeleteSession,
    },

    data() {
        return {
            IdRegistro: '',
            cartProducts: [],
            sku: '',
        }
    },

    computed: {
        countProductsCart() {
            return this.cartProducts && this.cartProducts.length;
        }
    },

    methods: {
        logout() {

            axios.post('https://examen.pitayasoft.mx/api/User/TerminarRegistro', { 'IdRegistro': this.$localStorage.get('id') })
                .then(response => {
                    console.log(response)

                    this.$router.push('/');
                })
                .catch(error => {
                    console.error(error)
                })
        },

        searchProduct() {
          const searchSku = {
              sku: this.sku,
          }

          axios.post('https://examen.pitayasoft.mx/api/User/BuscarProducto', searchSku)
            .then(response => {
                console.log(response.data)

                Swal.fire({
                    position: 'top-end',
                    icon: 'success',
                    title: 'El producto se agrego correctamente!',
                    showConfirmButton: false,
                    timer: 1500
                })

                this.cartProducts = response.data;

                this.sku = '';
            })
            .catch(error => {
                console.error(error)
            })
      }
        
    }
}
</script>

<style scoped>
    #dashboard {
        background-color: #1c1c1c;
        width: 100%;
        min-height: 20vh;
    }

    .text-gray {
        color: #807f7f;
    }

    .quantity {
        font-size: 2rem;
    }

    .mt-6 {
        margin-top: 6rem !important;
    }

    .mt-7 {
        margin-top: 10rem !important;
    }

    .btn {
        border-radius: 10px !important;
    }

    #footer {
       height:50px;
        line-height:50px;
        background: #1c1c1c;
        color:white;
        text-align:center;
        position:absolute;
        bottom:0;
        left:0;
        width:100%;
    }

    .mouse {
        cursor: pointer;
    }

    .btn-probador {
        padding: .375rem 4rem;
    }
</style>