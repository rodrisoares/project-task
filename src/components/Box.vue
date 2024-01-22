<template>
  <div class="box has-text-weight-bold" :style="{ backgroundColor: listaEstaVazia ? '#FF0000' : '#84fa84'}">
    <slot></slot>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'Box',
  props: ['listaEstaVazia'],
})
</script>
<style scoped>
.box {
  color: #000000;
}
</style>
