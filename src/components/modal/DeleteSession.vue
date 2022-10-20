<template>
  <div>
    <div class="modal fade" id="delete-session" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
        <div class="modal-dialog">
            <div class="modal-content">
            <div class="modal-body">
               <h2 class="text-center text-muted">¿Desea cerrar <br> la sesión?</h2>
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

</style>