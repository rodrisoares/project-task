<template>
  <div class="projetos">
    <h1 :class="{ 'title' : true ,'modo-escuro-titulo': modoEscuro }">Projetos</h1>
    <router-view :modoEscuro="modoEscuro"/>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'Projetos',
  props: {
    modoEscuro: {
      type: Boolean,
      default: false
    }
  }
})

</script>

<style scoped>
.projetos {
  padding: 1.25rem;
}
.modo-escuro-titulo {
  color: #fff;
}
</style>