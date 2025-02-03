<template>
  <div>
    <h1>Estacionamiento de Empleados</h1>
    <EmpleadoList />
  </div>
</template>

<script>
import EmpleadoList from './components/EmpleadoList.vue';
import './assets/css/styles.css';

export default {
  components: {
    EmpleadoList
  }
};
</script>