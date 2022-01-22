<template>
  <div id="app">

    <div class="urna">

      <Tela 
        :tela="tela" 
        :numeroVoto="numeroVoto"
        :qtdNumeros="qtdNumeros"
        :candidato="candidato" 
      />
      <Teclado 
        :adicionarNumero="adicionarNumero"
        :corrigir="corrigir"
        :confirmar="confirmar"
      />

    </div>

  </div>
</template>

<script>
import '@/css/global.css';
import Teclado from '@/components/Teclado.vue';
import Tela from '@/components/Tela.vue';

export default {
  name: 'App',
  components: {
    Teclado,
    Tela
  },
  methods: {
    
    adicionarNumero(numero){
      // VERIFICA LIMITE DE NÚMEROS DIGITADOS
      if(this.numeroVoto.length == this.qtdNumeros){
        return false;
      }

      // ADICIONA O NÚMERO SELECIONADO
      this.numeroVoto += ''+ numero;

      // VERIFICA CANDIDATO VOTADO
      this.verificarCadidato();
    },

    verificarCadidato(){
      // VOTO INCOMPLETO
      if(this.numeroVoto.length < this.qtdNumeros){
        return false;
      }

      // VERIFICA CANDIDATO EXISTENTE
      if(this.candidatos[this.tela][this.numeroVoto]){
        this.candidato = this.candidatos[this.tela][this.numeroVoto];
        return true;
      }

      // VOTO NULO
      this.candidato = {
        nome: 'Voto Nulo',
        partido: 'Voto Nulo',
        imagem: ''

      }
    },

    corrigir(){
      this.limpar();
    },

    limpar(){
      this.candidato = {}
      this.numeroVoto = ''
    },

    confirmar() {
      // VERIFICA LIMITE DE NÚMEROS DIGITADOS
      if(this.numeroVoto.length == this.qtdNumeros){
        return false;
      }

      return this.avancarTela();
    },

    avancarTela() {
      if(this.tela == 'prefeito') {
        this.tela = 'vereador';
        this.qtdNumeros = 5;
        return this.limpar();
      }
    }

  },
  data() {
    return {
      tela: 'prefeito',
      numeroVoto: '',
      qtdNumeros: 2,
      candidato: {},
      candidatos: {
        "prefeito":{
          "01":{
            "nome": "Ash",
            "partido": "Pokemon",
            "imagem": "https://raw.githubusercontent.com/william-costa/wdev-urna-eletronica-resources/master/images/ash.png"
          },
          "08":{
            "nome": "Vegeta",
            "partido": "Dragon Ball",
            "imagem": "https://raw.githubusercontent.com/william-costa/wdev-urna-eletronica-resources/master/images/vegeta.png"
          }
        },

        "vereador":{
          "01234":{
            "nome": "Pikachu",
            "partido": "Pokemon",
            "imagem": "https://raw.githubusercontent.com/william-costa/wdev-urna-eletronica-resources/master/images/pikachu.png"
          },
          "08001":{
            "nome": "Goku",
            "partido": "Dragon Ball",
            "imagem": "https://raw.githubusercontent.com/william-costa/wdev-urna-eletronica-resources/master/images/goku.png"
          }
        }
      }

    }
  }
}


</script>

<style>
#app {
  background-color: var(--background-color);
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}

.urna {
  width: 1000px;
  height: 500px;
  background-color: var(--ballot-box-background-color);
  padding: 30px;
  border-radius: 5px;
  display: flex;
  justify-content: space-between;
}
</style>
