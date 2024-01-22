<template>
  <div class="box">
    <div class="title">
      <h1 :class="{ 'modo-escuro-titulo': modoEscuro }">Tarefas</h1>
    </div>
    <div class="columns">
      <div class="column is-5" role="form" aria-label="Formulário para iniciar uma nova tarefa">
        <input
          class="input"
          type="text"
          placeholder="Qual tarefa você deseja iniciar?"
          v-model="descricao"
        />
      </div>
      <div class="column is-3">
        <div class="select">
          <select v-model="idProjeto">
            <option value="">Selecione o projeto</option>
            <option
              :value="projeto.id"
              v-for="projeto in projetos"
              :key="projeto.id"
            >
              {{ projeto.nome }}
            </option>
          </select>
        </div>
      </div>
      <div class="column">
        <Temporizador @aoFinalizarTarefa="salvarTarefa"/>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { computed, defineComponent, ref } from "vue";
import Temporizador from "./Temporizador.vue";
import { useStore } from 'vuex'
import { key } from '@/store'
import useNotificador from "@/hooks/notificador";
import { TipoNotificacao } from "@/interfaces/INotificacao";

export default defineComponent({
  name: "Formulario",
  emits: ['aoSalvarTarefa'],
  components: {
    Temporizador,
  },
  props: {
    modoEscuro: {
      type: Boolean,
      default: false
    }
  },
  setup (props, { emit }) {
    
    const store = useStore(key)

    const descricao = ref("")
    const idProjeto = ref("")
    const { notificar } = useNotificador();
    const projetos = computed(() => store.state.projeto.projetos)

    const salvarTarefa = (tempoEmSegundos: number) : void => {  
      notificar (
          TipoNotificacao.SUCESSO,
          'Sucesso!',
          "Tarefa adicionada com sucesso!"
      );  
      emit('aoSalvarTarefa', {
          duracaoEmSegundos: tempoEmSegundos,
          descricao: descricao.value,
          projeto: projetos.value.find(proj => proj.id == idProjeto.value)
      })
      descricao.value = ''
    }

    return {
      descricao,
      idProjeto,
      projetos,
      salvarTarefa
    }
  }

});
</script>
<style scoped>
.box {
  background-color: var(--bg-primario);
  color: var(--texto-primario);
}

input::placeholder {
  color: #000;
}
.modo-escuro-titulo {
  color: #fff;
}
</style>