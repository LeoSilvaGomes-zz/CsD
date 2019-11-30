<template>
  <div 
    class="container-sidebar"
  >
    <div class="search-container">
      <input type="text" class="input" >
    </div>
    <div class="deputados-container">
      <div 
        class="deputados-content"
        v-for= "deputado in deputados"
        :key="deputado.nome"
      >
        <div
          class="nome-content"
        >
          <b>Nome:</b> {{ deputado.nome }}
        </div>
        <div
          class="info-content"
        >
          <b>Partido:</b> {{ deputado.siglaPartido }}
          <b> UF:</b> {{ deputado.siglaUf }}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data(){
    return {
      deputados: [],
    }
  },
  created() {
    this.request();
  },
  methods: {
    request(){
      axios
        .get("https://dadosabertos.camara.leg.br/api/v2/deputados")
        .then(response => {
          this.deputados = response.data.dados;
        })
        .catch(error => {
          console.log(error.response)
        })
    }
  }

}
</script>

<style>

.nome-content {
  font-size: 90%;
  padding: 2% 0;
}

.info-content{
  font-size: 80%;
  padding: 2% 0;
}

.deputados-content{
  padding: 10px 15px;
  margin: 10px 10px;
  text-align: left;
  background-color: white;
  border-radius: 10px;
}

.deputados-container {
  overflow: auto;
  height: 80vh;
}

.container-sidebar{
  width: 25%;
  scroll-margin: none;
  background-color: #f8f8f8;
}


</style>