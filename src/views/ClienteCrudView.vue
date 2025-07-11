<script setup lang="ts">
import Button from 'primevue/button'
import { ref } from 'vue'
import ClienteCrudList from '../components/clientes_crud/ClientesCrudList.vue'
import ClienteCrudSave from '../components/clientes_crud/ClienteCrudSave.vue'
const mostrarDialog = ref<boolean>(false)
const ClienteCrudListRef = ref<typeof ClienteCrudList | null>(null)
const clienteEdit = ref<any>(null)

function handleCreate() {
  clienteEdit.value = null
  mostrarDialog.value = true
}

function handleEdit(cliente: any) {
  clienteEdit.value = cliente
  mostrarDialog.value = true
}

function handleCloseDialog() {
  mostrarDialog.value = false
}

function handleGuardar() {
  ClienteCrudListRef.value?.obtenerLista()
}
</script>

<template>
  <div class="page-container">
    <div class="content-wrapper">
      <div class="header-section">
        <div class="header-content">
          <h1 class="page-title">
            <i class="pi pi-users" style="margin-right: 0.5rem;"></i>
            Gestión de Clientes
          </h1>
          <Button label="Agregar Cliente" icon="pi pi-plus" @click="handleCreate" class="create-button"
            severity="success" />
        </div>
      </div>

      <div class="list-section">
        <ClienteCrudList ref="ClienteCrudListRef" @edit="handleEdit" />
      </div>

      <ClienteCrudSave :mostrar="mostrarDialog" :cliente="clienteEdit" :modoEdicion="!!clienteEdit"
        @guardar="handleGuardar" @close="handleCloseDialog" />
    </div>
  </div>
</template>

<style scoped>
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
</style>
