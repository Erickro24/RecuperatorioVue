
<template>
    <tr>
      <td>{{ empleado.nombre }}</td>
      <td>{{ empleado.area }}</td>
      <td>{{ empleado.vehiculo }}</td>
      <td>{{ empleado.placa }}</td>
      <td>{{ empleado.color }}</td>
      <td>
        <button @click="$emit('editar', empleado)" class="edit-button">Editar</button>
      </td>
    </tr>
  </template>
  
  <script>
  export default {
    props: ['empleado']
  };
  </script>
  
