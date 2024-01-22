<template>
  <Formulario @aoSalvarTarefa="salvarTarefa" :modoEscuro="modoEscuro" />
  <div class="lista">
    <Box v-if="semTarefas && !filtro" :listaEstaVazia="semTarefas">
      Você não está muito produtivo hoje
      <span class="has-text-weight-bold">:(</span>
    </Box>
    <div class="field">
      <p class="control has-icons-left">
        <input
          class="input"
          type="text"
          placeholder="Digite para filtrar"
          v-model="filtro"
        />
        <span class="icon is-small is-left">
          <i class="fas fa-search"></i>
        </span>
      </p>
    </div>
    <Tarefa
      v-for="(tarefa, index) in tarefas"
      :tarefa="tarefa"
      :key="index"
    />
  </div>
</template>

<script lang="ts">
import { computed, defineComponent, ref, watchEffect } from "vue";
import Formulario from '../components/Formulario.vue';
import Tarefa from '../components/Tarefa.vue';
import Box from '../components/Box.vue';
import { useStore } from "@/store";
import ITarefa from "@/interfaces/ITarefa";
import {
  CADASTRAR_TAREFA,
  OBTER_PROJETOS,
  OBTER_TAREFAS,
} from "@/store/tipo-acoes";


export default defineComponent({
  name: 'Tarefas',
  props: {
    modoEscuro: {
      type: Boolean,
      default: false
    }
  },
  components: {
    Formulario,
    Tarefa,
    Box,
  },
  methods: {
    salvarTarefa(tarefa: ITarefa): void {
      this.store.dispatch(CADASTRAR_TAREFA, tarefa);
    },
  },
  computed: {
    semTarefas(): boolean {
      return this.tarefas.length == 0;
    },
  },
  setup() {
    const store = useStore();
    store.dispatch(OBTER_TAREFAS);
    store.dispatch(OBTER_PROJETOS);
    const filtro = ref("");

    watchEffect(() => {
      store.dispatch(OBTER_TAREFAS, filtro.value)
    })

    return {
      tarefas: computed(() => store.state.tarefa.tarefas),
      store,
      filtro
    };
  },
});
</script>


<style scoped>
.lista {
  color: #000000;
}
input::placeholder {
  color: #000000;
}
</style>
