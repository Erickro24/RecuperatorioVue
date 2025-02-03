
<template>
    <div>
      <input v-model="search" placeholder="Buscar empleado..." class="search-bar" />
      <select v-model="filtroTipo" class="filter-select">
        <option value="">Todos</option>
        <option value="Automovil">Automovil</option>
        <option value="Moto">Moto</option>
      </select>
  
      <table class="employee-table">
        <thead>
          <tr>
            <th>Nombre</th>
            <th>Área</th>
            <th>Vehículo</th>
            <th>Placa</th>
            <th>Color</th>
            <th>Acciones</th>
          </tr>
        </thead>
        <tbody>
          <EmpleadoItem 
            v-for="empleado in empleadosFiltrados" 
            :key="empleado.id" 
            :empleado="empleado" 
            @editar="abrirEdicion"
          />
        </tbody>
      </table>
  
      <!-- Modal para edición de empleado -->
      <div v-if="empleadoSeleccionado" class="modal">
        <div class="modal-content">
          <h3>Editar Empleado</h3>
          <label>Nombre:</label>
          <input v-model="empleadoSeleccionado.nombre" type="text" />
  
          <label>Área:</label>
          <input v-model="empleadoSeleccionado.area" type="text" />
  
          <label>Vehículo:</label>
          <input v-model="empleadoSeleccionado.vehiculo" type="text" />
  
          <label>Placa:</label>
          <input v-model="empleadoSeleccionado.placa" type="text" />
  
          <label>Color:</label>
          <input v-model="empleadoSeleccionado.color" type="text" />
  
          <button @click="guardarEdicion" class="save-button">Guardar</button>
          <button @click="cerrarEdicion" class="cancel-button">Cancelar</button>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import { ref, computed, onMounted } from 'vue';
  import axios from 'axios';
  import EmpleadoItem from './EmpleadoItem.vue';
  
  export default {
    components: { EmpleadoItem },
    setup() {
      const empleados = ref([]);
      const search = ref('');
      const filtroTipo = ref('');
      const empleadoSeleccionado = ref(null);
  
      // Cargar empleados desde db.json
      onMounted(async () => {
        try {
          const response = await axios.get('http://localhost:3002/empleados');
          empleados.value = response.data;
        } catch (error) {
          console.error('Error al obtener los empleados:', error);
        }
      });
  
      const empleadosFiltrados = computed(() => {
        return empleados.value.filter(e =>
          e.nombre.toLowerCase().includes(search.value.toLowerCase()) &&
          (filtroTipo.value === '' || e.vehiculo === filtroTipo.value)
        );
      });
  
      const abrirEdicion = (empleado) => {
        empleadoSeleccionado.value = { ...empleado }; // Clonamos para evitar cambios en vivo
      };
  
      const cerrarEdicion = () => {
        empleadoSeleccionado.value = null;
      };
  
      const guardarEdicion = async () => {
        if (empleadoSeleccionado.value) {
          try {
            await axios.put(`http://localhost:3002/empleados/${empleadoSeleccionado.value.id}`, empleadoSeleccionado.value);
            const index = empleados.value.findIndex(e => e.id === empleadoSeleccionado.value.id);
            empleados.value[index] = { ...empleadoSeleccionado.value };
            cerrarEdicion();
          } catch (error) {
            console.error('Error al actualizar el empleado:', error);
          }
        }
      };
  
      return {
        empleados,
        search,
        filtroTipo,
        empleadosFiltrados,
        empleadoSeleccionado,
        abrirEdicion,
        cerrarEdicion,
        guardarEdicion
      };
    }
  };
  </script>
  
 