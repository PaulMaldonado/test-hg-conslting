<template>
  <div>
    <div class="modal fade" id="delete-session" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
        <div class="modal-dialog">
            <div class="modal-content">
            <div class="modal-body">
               <h2 class="text-center delete-session">¿Desea cerrar <br> la sesión?</h2>
            </div>
            <div class="modal-footer d-flex justify-content-between align-items-center">
                <button type="submit" class="btn btn-light" @click.prevent="$emit('delete-session')">Si</button>
                <button type="button" class="btn btn-dark" data-bs-dismiss="modal">No</button>
            </div>
            </div>
        </div>
    </div>
  </div>
</template>

<script>
export default {
    name: 'DeleteSession',
    
}
</script>

<style scoped>
  .delete-session {
    font-family: 'Montserrat';
    font-style: normal;
    font-weight: 300;
    font-size: 18px;
    line-height: 22px;
    text-align: center;
    letter-spacing: 0.3em;
    color: #1C1B1B;
  }

  .btn-dark {
    width: 70px;
    height: 38px;
    left: 246px;
    top: 399px;
    background: #1C1B1B;
    border-radius: 15px;
  }

  .btn-light {
    box-sizing: border-box;
    width: 70px;
    height: 38px;
    left: 62px;
    top: 282px;
    background: #FFFFFF;
    border: 1px solid #1C1B1B;
  }

  .modal {
    top: 18rem !important;
  }

  .modal-dialog {
      border: 1px solid #1C1B1B !important;
  }
</style>