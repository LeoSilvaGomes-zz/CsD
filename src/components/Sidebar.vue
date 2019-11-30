<template>
  <div 
    class="container-sidebar"
  >
    <div class="search-container">
      <input type="text" class="input" >
    </div>
    <div 
      class="filter-container"
      @click="select()"
    >
      <p style="font-size: 90%;"><b>Filtro</b></p>
      <font-awesome-icon
          :icon="iconFilter"
          style="color: #1a1919b6;"
          class="font-awesome"
      />
    </div>
    <div 
      class="filter-content"
      :style="display"
    >
      <p style="font-size: 90%;"><b>Filtro</b></p>
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
          <b style="font-size: 90%; color: #1a1919b6;">Nome:</b> {{ deputado.nome }}
        </div>
        <div
          class="info-content"
        >
          <b style="font-size: 90%; color: #1a1919b6;">Partido:</b> {{ deputado.siglaPartido }}
          <b style="font-size: 90%; color: #1a1919b6;"> UF:</b> {{ deputado.siglaUf }}
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
      iconFilter: 'caret-right',
      display: 'display: none;',
    }
  },
  mounted() {
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
    },
    select(){
      if(this.iconFilter == 'caret-right'){
        this.iconFilter = 'caret-down'
        this.display = 'display: flex;'
      }
      else if(this.iconFilter == 'caret-down'){        
        this.iconFilter = 'caret-right'
        this.display = 'display: none;'
      }
    }
  }

}
</script>

<style>

.nome-content {
  font-size: 80%;
  padding: 2% 0 0 0;
}

.info-content{
  font-size: 80%;
  padding: 2% 0;
}

.deputados-content{
  padding: 10px 15px;
  margin: 5px 10px;
  text-align: left;
  background-color: white;
  border-radius: 10px;
}

.deputados-container {
  overflow: auto;
  height: 80vh;
}

.filter-container {
  padding: 0px 15px;
  margin: 10px 10px 5px 10px;
  text-align: left;
  font-size: 80%;
  background-color: white;
  border-radius: 10px;
  color: #1a1919b6;
  display: flex;
  justify-content: space-between;
}

.container-sidebar{
  width: 25%;
  scroll-margin: none;
  background-color: #f8f8f8;
  padding: 20px 0px;
}

.filter-content {
  padding: 0px 15px;
  margin: 0px 10px 10px 10px;
  text-align: left;
  font-size: 80%;
  border-radius: 10px;
  color: #1a1919b6;
  display: flex;
  justify-content: space-between;
}

.font-awesome{
  margin: 9.5px 0px;
  font-size: 130%;
}

</style>