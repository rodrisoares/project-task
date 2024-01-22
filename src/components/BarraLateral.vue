<template>
  <header>
    <nav class="panel">
      <ul class="nav-list">
        <li>
          <router-link to="/" class="link">
            <i class="fas fa-tasks"></i>
            Tarefas
          </router-link>
        </li>
        <li>
          <router-link to="/projetos" class="link">
            <i class="fas fa-project-diagram"></i>
            Projetos
          </router-link>
        </li>
      </ul>
    </nav>
    <div class="header-container">
      <div class="txtModo">
        
        <input type="checkbox" class="checkbox" id="chk" @click="alterarTema" />
        <label class="label" for="chk">
          <i class="fas fa-moon"></i>
          <i class="fas fa-sun"></i>
          <div class="ball"></div>
        </label>
        <p>{{ textoBotao }}</p> 
      </div>
    </div>
  </header>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'BarraLateral',
  emits: ['aoTemaAlterado'],
  data () {
    return {
      modoEscuroAtivo: false
    }
  },
  computed: {
    textoBotao () {
      if (this.modoEscuroAtivo) {
        return 'Modo Escuro'
      }
      return 'Modo Claro'
    }
  },
  methods: {
    alterarTema () {
      this.modoEscuroAtivo = !this.modoEscuroAtivo
      this.$emit('aoTemaAlterado', this.modoEscuroAtivo)
    }
  }
})
</script>

<style scoped>
header {
  padding: 1rem;
  background: #0d3b66;
  width: 100%;
  height: 200vh;
  display: flex;              
  flex-direction: column;  
}
.header-container {
  display: flex;
  align-items: center;
  position: absolute;
  top: 100px;  
}
.txtModo p {
  margin-bottom: 10px; 
  margin-left: -8px;
}
.txtModo {
  color: #fff;
  padding: 20px;
}
.checkbox {
  opacity: 0;
  position: absolute;
}
.nav-list {
  display: flex;
  justify-content: space-around;
}
.label {
  background-color: #111;
  border-radius: 50px;
  cursor: pointer;
  display: flex;
  justify-content: space-between;
  align-items: center;

  padding: 5px;
  position: relative;
  height: 26px;
  width: 50px;
  transform: scale(1.5);

}
.label .ball {
  background-color: #fff;
  border-radius: 50%;
  position: absolute;
  top: 2px;
  left: 2px;
  height: 22px;
  width: 22px;
  transform: translateX(0px);
  transition: transform 0.2s linear;
}

.checkbox:checked + .label .ball {
  transform: translateX(24px);
}

.fa-moon {
  color: #f1c40f;
}

.fa-sun {
  color: #f39c12;
}
.panel {
  position: relative;
  left: 10px;
}
.panel li {
  margin: 8px 0;
}
.link {
  color: #fff;
}
.link:hover {
  color: #c9a30c;
}

@media only screen and (max-width: 768px) {
  header {
    padding: 2.5rem;
    height: auto;
  }
  .txtModo{
    color: #fff;
    position: relative;
  }
  .panel li {
    margin: 8px 0;
    left: 10px;
    position: relative;
    top: -60px;
  }
}
</style>
