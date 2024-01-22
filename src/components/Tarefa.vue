<template>
  <Box>
    <div class="columns " @click="tarefaClicada">
      <div class="column is-4">
        {{ tarefa.descricao || 'Tarefa sem descrição' }}
      </div>
      <div class="column is-3">
        {{ tarefa.projeto?.nome || 'N/D' }}
      </div>
      <div class="column ">
        <Cronometro :tempoEmSegundos="tarefa.duracaoEmSegundos"/>
      </div>
      <button class="button" @click="abrirModal">
          <span class="icon is-small">
          <i class="fas fa-pencil-alt"></i>
          </span>
      </button>
      <button class="button ml-2 is-danger" @click.stop="excluirTarefa">
        <span class="icon is-small">
          <i class="fas fa-trash"></i>
        </span>
      </button>
    </div>
    <Modal :mostrar="mostrarModal">
      <template v-slot:cabecalho>
        <p class="modal-card-title">Editando uma tarefa</p>
        <button @click="fecharModal" class="delete" aria-label="close"></button>
      </template>
      <template v-slot:corpo>
        <div class="field">
          <label for="descricaoDaTarefa" class="label"> Descrição </label>
          <input
            type="text"
            class="input"
            v-model="tarefaSelecionada.descricao"
            id="descricaoDaTarefa"
          />
        </div>
      </template>
      <template v-slot:rodape>
        <button @click="alterarTarefa" class="button is-success">
          Salvar alterações
        </button>
        <button @click="fecharModal" class="button">Cancelar</button>
      </template>
    </Modal>
  </Box>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import Cronometro from '../components/Cronometro.vue';
import Box from '../components/Box.vue';
import Modal from "../components/Modal.vue";
import { useStore } from "@/store";
import { ALTERAR_TAREFA, REMOVER_TAREFA } from "@/store/tipo-acoes";
import ITarefa from "@/interfaces/ITarefa";
import { TipoNotificacao } from "@/interfaces/INotificacao";
import useNotificador from '@/hooks/notificador';



export default defineComponent({
  name: 'Tarefa',
  props: {
    tarefa: {
      type: Object as () => ITarefa,
      required: true
    }
  },
  components: {
    Cronometro,
    Box,
    Modal
  },
  data() {
    return {
      mostrarModal: false,
      tarefaSelecionada: null as ITarefa | null,
    };
  },
  methods: {
    tarefaClicada() {
      this.$emit('aoTarefaClicada', this.tarefa);
    },
    abrirModal() {
      this.tarefaSelecionada = Object.assign({}, this.tarefa);
      this.mostrarModal = true;
    },
    fecharModal() {
      this.tarefaSelecionada = null;
      this.mostrarModal = false;
    },
    alterarTarefa() {
      this.store.dispatch(ALTERAR_TAREFA, this.tarefaSelecionada)
        .then(() => this.fecharModal())
        this.notificar(TipoNotificacao.SUCESSO, 'Sucesso!', 'Tarefa editada com sucesso!')
    },
    excluirTarefa() {
      this.store.dispatch(REMOVER_TAREFA,  this.tarefa)
      this.notificar(TipoNotificacao.SUCESSO, 'Sucesso!', 'Tarefa excluida com sucesso!')
    }
  },
  setup() {
    const store = useStore();
    const { notificar } = useNotificador()
    return {
      store,
      notificar
    };
  },
});
</script>
