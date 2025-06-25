<script setup lang="ts">
import Button from 'primevue/button'
import { ref } from 'vue'
import OdontologoList from '../components/odontologos/OdontologoList.vue'
import OdontologoSave from '../components/odontologos/OdontologoSave.vue'
import OdontologoServicioList from '../components/odontologo_servicios/OdontologoServicioList.vue'
import OdontologoServicioSave from '../components/odontologo_servicios/OdontologoServicioSave.vue'
import ServicioSave from '../components/servicios/ServicioSave.vue'
import { useAuthStore } from '@/stores/index'

const authStore = useAuthStore()

const mostrarDialog = ref(false)
const OdontologoListRef = ref<typeof OdontologoList | null>(null)
const odontologoEdit = ref<any>(null)

// Para gestión de servicios
const mostrarServiciosDialog = ref(false)
const mostrarAsignarDialog = ref(false)
const mostrarCrearServicioDialog = ref(false)
const odontologoSeleccionado = ref<any>(null)

function handleCreate() {
  odontologoEdit.value = null
  mostrarDialog.value = true
}

function handleEdit(odontologo: any) {
  odontologoEdit.value = odontologo
  mostrarDialog.value = true
}

function handleCloseDialog() {
  mostrarDialog.value = false
}

function handleGuardar() {
  OdontologoListRef.value?.obtenerLista()
}

function goToServicios() {
  window.location.href = '/servicios'
}

// Servicios
function verServicios(odontologo: any) {
  odontologoSeleccionado.value = odontologo
  mostrarServiciosDialog.value = true
}
function cerrarServiciosDialog() {
  mostrarServiciosDialog.value = false
}
function abrirAsignarServicios(odontologo: any) {
  odontologoSeleccionado.value = odontologo
  mostrarAsignarDialog.value = true
}
function cerrarAsignarDialog() {
  mostrarAsignarDialog.value = false
}
function abrirCrearServicio() {
  mostrarCrearServicioDialog.value = true
}
function cerrarCrearServicio() {
  mostrarCrearServicioDialog.value = false
}
function handleGuardarAsignar() {
  cerrarAsignarDialog()
  OdontologoListRef.value?.obtenerLista()
}
function handleGuardarCrearServicio() {
  cerrarCrearServicio()
  // Opcional: refrescar lista si lo necesitas
}
</script>

<template>
  <div class="page-container">
    <div class="content-wrapper">
      <div class="header-section">
        <div class="header-content">
          <h1 class="page-title">
            <i class="pi pi-users" style="margin-right: 0.5rem;"></i>
            Gestión de Odontólogos
          </h1>
          <div style="display: flex; gap: 1rem;">
            <Button label="Agregar Odontólogo" icon="pi pi-plus" @click="handleCreate" class="create-button"
              severity="success" />
            <Button label="Ir a Servicios" icon="pi pi-cog" class="manage-button" @click="goToServicios" />
          </div>
        </div>
      </div>

      <div class="list-section">
        <OdontologoList ref="OdontologoListRef" :usuario-logeado="authStore.user" @edit="handleEdit"
          @ver-servicios="verServicios" @asignar-servicios="abrirAsignarServicios"
          @crear-servicio="abrirCrearServicio" />
      </div>

      <OdontologoSave :mostrar="mostrarDialog" :odontologo="odontologoEdit" :modoEdicion="!!odontologoEdit"
        @guardar="handleGuardar" @close="handleCloseDialog" />

      <!-- Diálogo para ver servicios asignados -->
      <OdontologoServicioList :mostrar="mostrarServiciosDialog" @close="cerrarServiciosDialog" />

      <!-- Diálogo para asignar servicios -->
      <OdontologoServicioSave 
        :mostrar="mostrarAsignarDialog" 
        :odontologo="odontologoSeleccionado"
        @guardar="handleGuardarAsignar" 
        @close="cerrarAsignarDialog" 
      />

      <!-- Diálogo para crear un nuevo servicio -->
      <ServicioSave v-if="mostrarCrearServicioDialog" :mostrar="mostrarCrearServicioDialog"
        @guardar="handleGuardarCrearServicio" @close="cerrarCrearServicio" />
    </div>
  </div>
</template>

<style scoped>
/* ...tu CSS actual sin cambios... */
.page-container {
  background: linear-gradient(rgba(36, 0, 144, 0.1), rgba(36, 0, 144, 0.2)),
    url('@/assets/images/slider/slider-1.png');
  background-size: cover;
  background-position: center;
  min-height: 100vh;
  padding: 2rem 1rem;
}

.content-wrapper {
  max-width: 1400px;
  margin: 50vh auto;
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
}

.header-section {
  background-color: white;
  border-radius: 12px;
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
  padding: 1.5rem;
  margin-bottom: 1rem;
}

.header-content {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.page-title {
  font-size: 1.75rem;
  font-weight: 600;
  color: #240090;
  margin: 0;
  display: flex;
  align-items: center;
}

.create-button {
  padding: 0.75rem 1.5rem;
  font-weight: 500;
  transition: all 0.2s ease;
}

.create-button:hover {
  transform: translateY(-1px);
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
}

.list-section {
  flex-grow: 1;
}

/* Responsive styles */
@media (max-width: 768px) {
  .page-container {
    padding: 1rem 0.5rem;
  }

  .header-section {
    border-radius: 0;
    margin: -1rem -0.5rem 1rem;
  }

  .header-content {
    flex-direction: column;
    gap: 1rem;
    text-align: center;
  }

  .page-title {
    font-size: 1.5rem;
    justify-content: center;
  }

  .create-button {
    width: 100%;
  }
}

/* Animations */
@keyframes slideDown {
  from {
    opacity: 0;
    transform: translateY(-20px);
  }

  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.header-section {
  animation: slideDown 0.3s ease-out;
}

.manage-button {
  background-color: #0056b3;
  color: #ffffff;
  border: none;
  padding: 0.75rem 1.5rem;
  border-radius: 5px;
  font-size: 1rem;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.manage-button:hover {
  background-color: #004494;
}
</style>