<script setup lang="ts">
import { ref, onMounted } from 'vue'
import { useToast } from 'primevue/usetoast';
import http from '../plugins/axios'
import OdontologoList from '../components/perfil_odontologo/OdontologoList.vue'
import OdontologoSave from '../components/perfil_odontologo/OdontologoSave.vue'
import CambiarPasswordDialog from '../components/perfil_odontologo/CambiarPasswordDialog.vue'
import Dialog from 'primevue/dialog'
import type { Odontologo } from '../models/Odontologo'

const odontologo = ref<Odontologo | null>(null)
const mostrarDialog = ref(false)
const mostrarDialogPassword = ref(false)
const toast = useToast();

async function cargarOdontologoAutenticado() {
  try {
    odontologo.value = await http.get('odontologos/mi-perfil').then(response => response.data);
    toast.add({ severity: 'success', summary: 'Éxito', detail: 'Datos del odontólogo cargados correctamente.', life: 3000 });
  } catch (error) {
    console.error('Error al cargar los datos del odontólogo:', error);
    toast.add({ severity: 'error', summary: 'Error', detail: 'No se pudieron cargar los datos del odontólogo.', life: 3000 });
  }
}

async function cambiarPassword(passwordActual: string, nuevaPassword: string) {
  try {
    const response = await http.post('odontologos/cambiar-password', { 
      passwordActual, 
      nuevaPassword,
    });
    mostrarDialogPassword.value = false;
    toast.add({ severity: 'success', summary: 'Éxito', detail: 'La contraseña fue cambiada exitosamente.' , life: 3000 });
  } catch (error) {
    console.error('Error al cambiar la contraseña:', error);
    toast.add({ severity: 'error', summary: 'Error', detail: 'No se pudo cambiar la contraseña.', life: 3000 });
  }
}

async function guardarCambios(odontologoActualizado: Odontologo) {
  try {
    await http.patch(`odontologos/${odontologoActualizado.id}`, odontologoActualizado);
    mostrarDialog.value = false;
    toast.add({ severity: 'success', summary: 'Éxito', detail: 'Datos actualizados correctamente.', life: 3000 });
  } catch (error) {
    console.error('Error al guardar los cambios:', error);
    toast.add({ severity: 'error', summary: 'Error', detail: 'No se pudieron guardar los cambios.', life: 3000 });
  }
}

onMounted(() => {
  cargarOdontologoAutenticado();
});
</script>

<template>
  <div class="slider-background">
    <div class="content-container">
      <h1 class="title">Mi Perfil</h1>
      <div v-if="odontologo">
        <!-- Muestra los datos del odontologo -->
        <OdontologoList :odontologo="odontologo" @editar="mostrarDialog = true"
          @cambiarPassword="mostrarDialogPassword = true" />

        <!-- Diálogo para editar los datos -->
        <Dialog v-model:visible="mostrarDialog" header="Editar Perfil" :style="{ width: '30rem' }" modal>
          <OdontologoSave :odontologo="{ ...odontologo }" :modoEdicion="true" @guardar="guardarCambios" />
        </Dialog>

        <!-- Diálogo para cambiar la contraseña -->
        <Dialog v-model:visible="mostrarDialogPassword" header="Cambiar Contraseña" :style="{ width: '30rem' }" modal>
          <CambiarPasswordDialog @guardar="cambiarPassword" @cerrar="mostrarDialogPassword = false" />
        </Dialog>
      </div>
      <p v-else>Cargando datos del odontologo...</p>
    </div>
  </div>
</template>
<style scoped>
.slider-background {
  background-color: #2700c1;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: flex-start;
  padding: 2rem;
}

.content-container {
  background: rgba(255, 255, 255, 0.9);
  border-radius: 8px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
  padding: 2rem;
  width: 100%;
  max-width: 1000px;
  margin-top: 200px;
}

.title {
  font-size: 2rem;
  text-align: center;
  margin-bottom: 1.5rem;
}
</style>
