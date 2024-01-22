<template>
  <section>
    <i class="far fa-clock" :class="{ 'modo-escuro': modoEscuro, 'display': !timerParado, 'display-parado': timerParado }">
    </i>
    {{ tempoDecorrido }}
  </section>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'Cronometro',
  props: {
    tempoEmSegundos: {
      type: Number,
      default: 0
    }
  },
  data() {
    return {
      timerParado: true,
      modoEscuro: false
    }
  },
  computed: {
    tempoDecorrido () : string {
      return new Date(this.tempoEmSegundos * 1000).toISOString().substr(11,8)
    }
  },
  watch: {
    timerParado (value) {
      if (value) {
        this.modoEscuro = true
      } else {
        this.modoEscuro = false
      }
    }
  }
})
</script>

<style scoped>
.display {
  color: var(--texto-primario);
}

.display-parado {
  color: var(--texto-secundario);
}

.modo-escuro {
  color: var(--texto-secundario);
}
</style>