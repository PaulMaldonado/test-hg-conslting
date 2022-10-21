<template>
  <div>
      <section id="dashboard">
          <h2 class="text-center text-gray h4 p-5">Bienvenido. Usuario <br> Escoge tus prendas</h2>
      </section>

      <div class="container">
          <div class="row">
              <div class="col-md-6 col-sm-12 col-lg-6 col-xl-6 col-xxl-6 mt-3 mx-auto" v-for="cart in cartProducts" :key="cart.id">
                <div class="card shadow-lg p-2">
                    <div class="d-flex justify-content-end">
                        <div class="close-product d-flex justify-content-center align-items-center">
                            <i class="fa-solid fa-xmark" @click.prevent="deleteProduct(cart.idProducto)"></i>
                        </div>
                    </div>
                    

                    <div id="carouselExampleControls" class="carousel slide" data-bs-ride="carousel">
                        <div class="carousel-inner">
                            <div class="carousel-item active">
                                <img :src="cart.imagen" class="d-block w-100" :alt="cart.imagen">
                            </div>
                            <div class="carousel-item">
                                <img :src="cart.imagen1" class="d-block w-100" :alt="cart.imagen1">
                            </div>
                            <div class="carousel-item">
                                <img :src="cart.imagen2" class="d-block w-100" :alt="cart.imagen2">
                            </div>
                            <div class="carousel-item">
                                <img :src="cart.imagen3" class="d-block w-100" :alt="cart.imagen3">
                            </div>
                        </div>
                        <button class="carousel-control-prev" type="button" data-bs-target="#carouselExampleControls" data-bs-slide="prev">
                            <span class="carousel-control-prev-icon" aria-hidden="true"></span>
                            <span class="visually-hidden">Previous</span>
                        </button>
                        <button class="carousel-control-next" type="button" data-bs-target="#carouselExampleControls" data-bs-slide="next">
                            <span class="carousel-control-next-icon" aria-hidden="true"></span>
                            <span class="visually-hidden">Next</span>
                        </button>
                    </div>

                    <div class="card-body">
                        <h3 class="card-title">
                            {{ cart.nombre }}
                        </h3>

                        <h4 class="card-text mt-4 text-uppercase">
                            Talla
                        </h4>

                        <form @click.prevent="addProduct">
                             <div class="d-flex justify-content-start align-items-center mt-4" v-if="cart.idTalla">
                                <button class="btn btn-dark talla">
                                    {{ cart.nombreTalla }}
                                </button>
                            </div>
                        </form>
                       
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
          <h2 class="mouse logout" data-bs-toggle="modal" data-bs-target="#delete-session">Cerrar Sesión</h2>
      </div>

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

                  <input type="submit" value="Buscar producto" class="btn btn-dark mt-3 btn-add">
              </form>
          </div>
        </div>
      </div>
    </div>

      <DeleteSession @delete-session="logout" />

      <footer id="footer">

      </footer>
  </div>
</template>

<script>
import DeleteSession from '../components/modal/DeleteSession.vue'
import axios from 'axios'

export default {
    name: 'Dashboard',
    components: {
        DeleteSession,
    },

    data() {
        return {
            IdRegistro: '',
            Idregistro: '',
            Idproducto: '',
            cartProducts: [],
            sku: '',
            Idtalla: '',
            productAdd: [],
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

                this.cartProducts = response.data;

                this.sku = '';
            })
            .catch(error => {
                console.error(error)
            })
      },

      addProduct() {
          this.$localStorage.set(this.Idtalla);
      },

      deleteProduct(Idproducto) {
          //console.log(Idproducto)
          //console.log(this.$localStorage.get('id'))
          axios.post('https://examen.pitayasoft.mx/api/User/QuitarProducto',
            {   'Idregistro': this.$localStorage.get('id'),
                'Idproducto' : Idproducto 
            }) 
            .then(response => {
                console.log(response.data)
                console.log(Idproducto)
                console.log(this.$localStorage.get('id'))
            })
            .catch(error => {
                console.error(error)
                console.log(Idproducto)
                console.log(this.$localStorage.get('id'))
            })
      }
    }
}
</script>

<style scoped>
    #dashboard {
        background-color: #1c1c1c;
        width: 100%;
        height: 15vh;
        left: 77px;
        top: 50px;
        font-family: 'Montserrat';
        font-style: normal;
        font-weight: 300;
        font-size: 18px;
        line-height: 22px;
        text-align: center;
        letter-spacing: 0.3em;
        color: #FFFFFF;
    }

    .text-gray {
       color: #FFFFFF;
    }

    .quantity { 
        font-family: 'Montserrat';
        font-style: normal;
        font-weight: 300;
        font-size: 22px;
        line-height: 27px;
        letter-spacing: 0.3em;
        color: #000000;
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
        position: absolute;
        height: 10vh;
        top: 895px;
        background: #1C1B1B;
        height: 50px;
        line-height: 50px;
        left: 0;
        width:100%;
    }

    .mouse {
        cursor: pointer;
    }

    .btn-probador {
        padding: .375rem 4rem;
    }

    .logout {
        font-family: 'Montserrat';
        font-style: normal;
        font-weight: 300;
        font-size: 18px;
        line-height: 22px;
        letter-spacing: 0.3em;
        color: #1C1B1B;
    }

    .btn-outline-dark {
        left: 68px;
        top: 275px;
        font-family: 'Montserrat';
        font-style: normal;
        font-weight: 400;
        font-size: 22px;
        line-height: 27px;
        letter-spacing: 0.1em;
        color: #000000;
    }

    .btn-outline-dark:hover {
        color: white;
    }

    .talla {
        box-sizing: border-box;
        left: 86px;
        top: 559px;
        border: 0.5px solid #1C1B1B;
    }

    .img-product {
        width: 253px;
        height: 277px;
        left: 87px;
        top: 158px;
    }

    .card-title {
        width: 260px;
        left: 95px;
        top: 438px;
        font-family: 'Montserrat';
        font-style: normal;
        font-weight: 300;
        font-size: 16px;
        line-height: 20px;
        letter-spacing: 0.1em;
        color: #000000;
    }

    .card-text {
        left: 86px;
        top: 524px;
        font-family: 'Montserrat';
        font-style: normal;
        font-weight: 300;
        font-size: 16px;
        line-height: 20px;
        letter-spacing: 0.2em;
        color: #000000;
    }

    .close-product {
        box-sizing: border-box;
        width: 26px;
        height: 26px;
        top: 170px;
        background: #FFFFFF;
        border: 0.5px solid #000000;
        border-radius: 20px !important;
    }

    .form-control {
        border-radius: 10px !important;
        font-family: 'Montserrat';
        font-style: normal;
        font-weight: 300;
    }

    .btn-add {
        font-family: 'Montserrat';
        font-style: normal;
        font-weight: 300;
    }

    .modal-title {
        left: 162px;
        top: 245px;
        font-family: 'Montserrat';
        font-style: normal;
        font-weight: 300;
        font-size: 22px;
        line-height: 27px;
        letter-spacing: 0.1em;
        color: #000000;
    }

    .modal-dialog {
        border: 1px solid #1C1B1B !important;
    }

    .modal {
        top: 20rem !important;
    }
</style>