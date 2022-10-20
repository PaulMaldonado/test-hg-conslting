<template>
  <div>
      <header class="wrapper">

      </header>

      <div class="container mt-4">
          <div class="row">
              <div class="col-md-7 col-sm-12 col-lg-7 col-xl-7 col-xxl-7 mx-auto">
                  <form @submit.prevent="login">
                      <div class="row">
                          <div class="col-md-12 col-sm-12 col-lg-12 col-xl-12 col-xxl-12 mt-3">
                              <div class="form-group">
                                  <label class="mx-auto">Nombre</label>
                                  <input type="text" class="form-control" v-model="nombre" required>
                              </div>
                          </div>
                      </div>

                      <div class="row">
                          <div class="col-md-12 col-sm-12 col-lg-12 col-xl-12 col-xxl-12 mt-3">
                              <div class="form-group">
                                  <label class="mx-auto">Correo electrónico</label>
                                  <input type="email" class="form-control" v-model="email" required>
                              </div>
                          </div>
                      </div>

                      <div class="d-grid gap-2">
                          <input type="submit" value="Ingresar" class="btn btn-dark mt-4">
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
    }

    .btn {
        border-radius: 10px !important;
    }
</style>