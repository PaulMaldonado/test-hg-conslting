<template>
  <div>
      <header class="wrapper">

      </header>

      <div class="container mt-4">
          <div class="row">
              <div class="col-md-7 col-sm-12 col-lg-7 col-xl-7 col-xxl-7 mx-auto">
                  <form @submit.prevent="login">
                      <div class="row">
                          <div class="col-md-12 col-sm-12 col-lg-12 col-xl-12 col-xxl-12 mt-4">
                              <div class="form-group">
                                  <p class="text-center text-muted text-name">Nombre</p>
                                  <input type="text" class="form-control" v-model="nombre" required>
                              </div>
                          </div>
                      </div>

                      <div class="row">
                          <div class="col-md-12 col-sm-12 col-lg-12 col-xl-12 col-xxl-12 mt-4">
                              <div class="form-group">
                                  <p class="text-center text-muted text-email">Correo electrónico</p>
                                  <input type="email" class="form-control" v-model="email" required>
                              </div>
                          </div>
                      </div>

                      <div class="d-grid gap-2">
                          <input type="submit" value="Ingresar" class="btn btn-dark mt-6">
                      </div>
                  </form>
              </div>
          </div>
      </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
    name: 'Login',

    data() {
        return {
            nombre: '',
            email: ''
        }
    },

    methods: {
        login() {

            const user = {
                nombre: this.nombre,
                email: this.email
            }

            axios.post('https://examen.pitayasoft.mx/api/User/Registro', user)
                .then(response => {
                    console.log(response.data)

                    this.$localStorage.set('id', response.data.id)

                    this.$router.push('/dashboard');
                })
                .catch(error => {
                    console.error(error)
                })
        }
    }
}
</script>

<style scoped>
    .wrapper {
        background-color: #1c1c1c;
        width: 100%;
        min-height: 20vh;
    }

    .form-control {
        border-radius: 10px !important;
        font-family: 'Montserrat';
        font-style: normal;
        font-weight: 300;
    }

    .btn-dark {
        font-family: 'Montserrat';
        font-style: normal;
        font-weight: 500;
        font-size: 22px;
        line-height: 27px;
        letter-spacing: 0.1em;
    }

    .btn {
        border-radius: 10px !important;
    }

    .text-name {
        font-family: 'Montserrat';
        font-style: normal;
        font-weight: 300;
        font-size: 22px;
        line-height: 27px;
        letter-spacing: 0.1em;
        color: #000000;
    }

    .text-email {
        font-family: 'Montserrat';
        font-style: normal;
        font-weight: 400;
        font-size: 22px;
        line-height: 27px;
        letter-spacing: 0.1em;
        color: #000000;

    }

    .mt-6 {
        margin-top: 4rem !important;
        margin-bottom: 4rem !important;
    }
</style>