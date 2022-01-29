<template>
  <div>
    <section class="section">
      <div class="container">
        <div class="columns">
          <div class="column is-3 lista-de-conversas">
            <div class="barra-superior" />
            <div class="item" 
              v-bind:key="index" 
              v-for="(conversa, index) in conversas"
              @click="indiceAtivo = index"
            >
              <div class="title is-6">
                {{conversa.usuario}}
              </div>
              <div class="subtitle is-6">
                Última mensagem...
              </div>
            </div>
          </div>
          <div class="column conversa-ativa">
            <div class="barra-superior">
              <span>{{conversas[indiceAtivo].usuario}}</span>
            </div>

            <div class="lista-mensagem">
              <Message 
                v-bind:key="index"
                v-for="(mensagem, index) in conversas[indiceAtivo].mensagens"
                :conteudo="mensagem.conteudo" 
                :verde="mensagem.verde"
              />

              <div class="barra-inferior">
                <input v-model="conteudoNovaMensagem" v-on:keyup.enter="enviarMensagem" type="text" class="input" placeholder="Insira uma mensagem">
              </div>
            </div>

          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import { conversasIniciais } from './data.js';
import Message from './message.vue';

export default {
  data: function() {
    return { // onde ficara todas as variáveis desse arquivo
      conversas: conversasIniciais,
      indiceAtivo: 0,
      conteudoNovaMensagem: ""
    }
  },
  components: { // Onde será listados todos os componentes desse arquivo
    Message
  },
  methods: { // Onde ficará todos os metodos desse arquivo
    enviarMensagem: function() {
      let horarioAtual = new Date().getHours() + ":" + new Date().getMinutes();
      
      let novaMensagem = {
        horario: horarioAtual,
        conteudo: this.conteudoNovaMensagem,
        verde: true
      };
      
      this.conversas[this.indiceAtivo]
        .mensagens
        .push(novaMensagem);

      this.conteudoNovaMensagem = "";
    }
  }
}
</script>

<style >

.barra-inferior {
  bottom: 0;
  width: 76.5%;
  padding: 10px;
  position: absolute;
  background-color: #f0f0f0;
}

.barra-inferior input {
  border: none;
  padding: 10px;
  margin: 0 10px;
  width: 90%;
  border-radius: 15px;
  font-size: 16px;
}

.lista-mensagem {
  height: 85%;
  display: flex;
  justify-content: flex-end;
  flex-direction: column;
}

.columns {
  min-height: 750px;
  box-shadow: 0 3rem 3rem -1rem rgba(10,10,10,.2);
}

.column {
  padding: 0;
}

.lista-de-conversas {
  background: white;
}

.conversa-ativa {
  background-color: #E5DDD5;
}

.barra-superior {
  margin: 0;
  height: 50px;
  background-color: #E5DDD5;
  border-right: 1px solid #E1E1E1;
  border-bottom: 1px solid rgba(200,200,200);
}

.barra-superior span {
  line-height: 50px;
  margin-left: 25px;
  font-weight: 500;
}

.item {
  border-bottom: 1px solid #F2F2F2;
  padding: 15px 30px;
  margin-bottom: 0 !important;
}

.subtitle { 
  color: gray;
}

.item:hover {
  background-color: #F5F5F5;
  cursor: pointer;
}
</style>