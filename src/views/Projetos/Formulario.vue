<template>
  <section>
    <form @submit.prevent="salvar">
      <div class="field">
        <label for="nomeDoProjeto" :class="{ 'label-modo-escuro': modoEscuro }"> Nome do Projeto </label>
        <input
          type="text"
          class="input"
          v-model="nomeDoProjeto"
          id="nomeDoProjet"
        />
      </div>
      <div class="field">
        <button class="button is-primary" type="submit">Salvar</button>
        <button class="button is-danger ml-2" @click="cancelar" >Cancelar</button>
      </div>
    </form>
   
  </section>
</template>

<script lang="ts">
import { useStore } from "@/store";
import { defineComponent, ref, watch } from "vue";

import { TipoNotificacao } from "@/interfaces/INotificacao";
import useNotificador from '@/hooks/notificador'
import { ALTERAR_PROJETO, CADASTRAR_PROJETO } from "@/store/tipo-acoes";
import { useRouter } from "vue-router";

export default defineComponent({
  name: "Formulario",
  props: {
    id: {
      type: String
    },
    modoEscuro: {
      type: Boolean,
      default: false
    }
  },
  setup(props) {

    const router = useRouter()
    const store = useStore();
    const { notificar } = useNotificador();

    const nomeDoProjeto = ref("")

    if (props.id) {
        const projeto = store.state.projeto.projetos.find(
          (proj) => proj.id == props.id
        );
        nomeDoProjeto.value = projeto?.nome || "";
    }

    const cadastradoComSucesso  = () => {
        nomeDoProjeto.value = "";
        notificar(TipoNotificacao.SUCESSO, 'Sucesso!', 'Projeto adicionado com sucesso!');      
        router.push("/projetos");
    }
    const editadoComSucesso  = () => {
        nomeDoProjeto.value = "";
        notificar(TipoNotificacao.SUCESSO, 'Sucesso!', 'Projeto editado com sucesso!');    
        router.push("/projetos");
    }
   
    const salvar = () => {
        if ( nomeDoProjeto.value === "") {
            notificar(TipoNotificacao.ATENCAO, 'Erro!', 'Por favor, insira um nome no projeto!')
        } else {
            if (props.id) {
              store
                .dispatch(ALTERAR_PROJETO, {
                  id: props.id,
                  nome: nomeDoProjeto.value,
                })
                .then(() => editadoComSucesso());
            }
          else {
            store
              .dispatch(CADASTRAR_PROJETO, nomeDoProjeto.value)
              .then(() => cadastradoComSucesso());
          }
        }
    }
    const cancelar = () => {
      router.push("/projetos");
    }

    watch(() => props.modoEscuro, (novoValor) => {
        const label = document.querySelector('#nomeDoProjeto')
        if (label) {
          if (novoValor) {
            label.classList.add('label-modo-escuro')
          } else {
            label.classList.remove('label-modo-escuro')
          }
        }
    })

    return {
      nomeDoProjeto,
      salvar,
      cancelar
    };
  }, 
});
</script>

<style scoped>
.label-modo-escuro {
  color: #fff;
}
</style>