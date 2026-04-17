<script setup>

defineProps(['id', 'descricao', 'status'])
defineEmits(['editar', 'excluir'])
</script>


<template>
  <li>
    {{ descricao }}
    <a href="#" @click.prevent="$emit('excluir', id)">Excluir</a>

  </li>
</template>



<style>

</style>
