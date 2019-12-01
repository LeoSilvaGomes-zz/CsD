<template>
  <div 
    class="container-sidebar"
  >
    <div class="content-sidebar">
      <div class="search-container">
        <font-awesome-icon
          icon="search"
          style="color: #1a1919b6;"
          class="font-awesome-search"
        />
        <input 
          type="text" 
          class="input" 
          v-model="searchDeputado"
          @focus="handleFocus"
        >
      </div>
      <div 
        class="filter-container"
        @click="select()"
      >
        <p style="font-size: 90%;"><b>Filtro</b></p>
        <font-awesome-icon
            :icon="iconFilter"
            style="color: #1a1919b6;"
            class="font-awesome-filter"
        />
      </div>
      <div 
        class="filter-content"
        :style="display"
      >
        <div 
          class="uf-filter"
          @click="selectUfs()"
        >
          <div style="display: flex; justify-content: space-between; width: 100%;">    
            <p style="font-size: 90%;"><b>UF</b></p>
            <font-awesome-icon
                :icon="iconFilterUf"
                style="color:#1a1919b6;"
                class="font-awesome-filter"
            />
          </div>

          <div 
            class="uf-container"
            :style="displayUf"  
          >
            <div
              class="uf-content"
              v-for="uf in brstates"
              :key="uf"
              @click="filterUf(uf)"
            >
              {{ uf }}
            </div>
          </div>
        </div>

        <div 
          class="partido-filter"
          @click="selectPartido()"
        >
          <div style="display: flex; justify-content: space-between; width: 100%;">    
            <p style="font-size: 90%;"><b>Partido</b></p>
            <font-awesome-icon
                :icon="iconFilterPartido"
                style="color:#1a1919b6;"
                class="font-awesome-filter"
            />
          </div>

          <div 
            class="partido-container"
            :style="displayPartido"  
          >
            <div
              class="uf-content"
              v-for="partido in partidos"
              :key="partido.sigla"
              @click="filterPartido(partido)"
            >
              {{ partido.sigla }}
            </div>
          </div>
        </div>
      </div>
      
      <div 
        class="deputados-container"
        :style="height"
      >
        <div 
          v-for="deputado in filteredDeputados"
          :key="deputado.nome"
          @click="selectDeputado(deputado.id)"
        >
          <div
            v-if="filteredPartido == ''"
          >
            <div
              class="deputados-content"
              v-if="filteredUf == ''"
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

            <div
              class="deputados-content"
              v-else-if="deputado.siglaUf == filteredUf"
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

          <div
            v-else-if="deputado.siglaPartido == filteredPartido"
          >
            <div
              class="deputados-content"
              v-if="filteredUf == ''"
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

            <div
              class="deputados-content"
              v-else-if="deputado.siglaUf == filteredUf"
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
      </div>
    </div>
  </div>
</template>





<script>
import axios from 'axios'

export default {
  props: {
    id: {
      default: '',
      type: String,
    }
  },
  data(){
    return {
      isFocused: false,
      deputados: [],
      partidos: ['none'],
      iconFilter: 'caret-right',
      iconFilterUf: 'caret-right',
      iconFilterPartido: 'caret-right',
      display: 'display: none;',
      displayUf: 'display: none;',
      displayPartido: 'display: none;',
      filteredDeputados: [],
      filteredUf: '',
      filteredPartido: '',
      searchDeputado: '',
      height: 'height: 85vh;',
      brstates: ['none', 'AC', 'AL', 'AP', 'AM', 'BA', 'CE', 'DF',
        'ES', 'GO', 'MA', 'MT', 'MS', 'MG', 'PA', 'PB', 'PR',
        'PE', 'PI', 'RJ', 'RN', 'RS', 'RO', 'RR', 'SC', 'SP', 'SE', 'TO'],
    }
  },
  mounted() {
    this.request();
  },
  watch: {
    searchDeputado(value){
      if (this.isFocused && value.length > 0) {
        this.filter(value)
      } else {
        this.filteredDeputados = this.deputados;
      }
    }
  },
  methods: {
    request(){
      axios
        .get("https://dadosabertos.camara.leg.br/api/v2/deputados")
        .then(response => {
          this.deputados = response.data.dados;
          this.filteredDeputados = response.data.dados;
        })
        .catch(error => {
          // eslint-disable-next-line
          console.log(error.response)
        })

      axios
        .get("https://dadosabertos.camara.leg.br/api/v2/partidos")
        .then(response => {
          this.partidos = response.data.dados
        })
        .catch(error => {
          // eslint-disable-next-line
          console.log(error.response)
        })
    },
    select(){
      if(this.iconFilter == 'caret-right'){
        this.iconFilter = 'caret-down'
        this.display = 'display: flex;'
        this.height = 'height: 75vh;'
      }
      else if(this.iconFilter == 'caret-down'){        
        this.iconFilter = 'caret-right'
        this.display = 'display: none;'
        this.filteredUf = ''
        this.filteredPartido = ''
        this.height = 'height: 85vh;'
      }
    },
    selectUfs(){
      if(this.iconFilterUf == 'caret-right'){
        this.iconFilterUf = 'caret-down'
        this.displayUf = 'display: flex;'
        this.height = 'height: 65vh;'
      }
      else if(this.iconFilterUf == 'caret-down'){        
        this.iconFilterUf = 'caret-right'
        this.displayUf = 'display: none;'
        this.height = 'height: 75vh;'
      }
      if(this.iconFilterPartido == 'caret-down' && this.iconFilterUf == 'caret-down'){
        this.height = 'height: 55vh;'
      }
      if(this.iconFilterPartido == 'caret-down' && this.iconFilterUf == 'caret-right'){
        this.height = 'height: 65vh;'
      }
    },
    selectPartido(){
      if(this.iconFilterPartido == 'caret-right'){
        this.iconFilterPartido = 'caret-down'
        this.displayPartido = 'display: flex;'
        this.height = 'height: 65vh;'
      }
      else if(this.iconFilterPartido == 'caret-down'){        
        this.iconFilterPartido = 'caret-right'
        this.displayPartido = 'display: none;'
        this.height = 'height: 75vh;'
      }
      if(this.iconFilterPartido == 'caret-down' && this.iconFilterUf == 'caret-down'){
        this.height = 'height: 55vh;'
      }
      if(this.iconFilterPartido == 'caret-right' && this.iconFilterUf == 'caret-down'){
        this.height = 'height: 65vh;'
      }
    },
    filter: function(search) {
      this.filteredDeputados = this.deputados.filter(deputado => {
        return deputado.nome.toLowerCase().indexOf(search.toLowerCase()) !== -1 ? true : false; 
      })
    },
    handleFocus: function() {
      this.isFocused = !this.isFocused;
    },
    filterUf: function(value){
      if(value == 'none'){
        this.filteredUf = '';
      }
      else{
        this.filteredUf = value;
      }
    },
    filterPartido: function(value){
      if(value == 'none'){
        this.filteredPartido = '';
      }
      else{
        this.filteredPartido = value.sigla;
      }
    }
  }

}
</script>





<style>

.container-sidebar{
  height: 100vh;
  scroll-margin: none;
  display: flow-root;
  width: 25%;
}

.content-sidebar{
  width: 25%;
  background-color: #f8f8f8;
  position: fixed;
}

.info-content{
  font-size: 80%;
  padding: 2% 0;
}

.search-container{
  padding: 0px 15px;
  margin: 15px 10px 5px 10px;
  text-align: left;
  font-size: 80%;
  background-color: white;
  border-radius: 10px;
  color: #1a1919b6;
  display: flex;
  justify-content: space-between;
}

.input {
  border: none;
  width: 90%;
}

.deputados-container {
  margin-top: 15px;
  overflow:auto;
  -ms-overflow-style: none;
  overflow: -moz-scrollbars-none; 
}

.deputados-container::-webkit-scrollbar {
  display: none;
}

.deputados-content{
  padding: 10px 15px;
  margin: 5px 10px;
  text-align: left;
  background-color: white;
  border-radius: 10px;
}

.nome-content {
  font-size: 80%;
  padding: 2% 0 0 0;
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


.filter-content {
  padding: 0px 15px;
  margin: 0px 10px 10px 10px;
  text-align: left;
  font-size: 80%;
  border-radius: 10px;
  color: #1a1919b6;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.font-awesome-filter{
  margin: 9.5px 0px;
  font-size: 130%;
}

.uf-filter{
  text-align: left;
  width: 100%;
  border-radius: 10px;
  color: #1a1919b6;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}


.uf-container{
  height: 10vh;
  overflow: auto;
  font-size: 15px;
  flex-direction: column;
}

.uf-content{
  padding: 5px;
  font-size: 15px;
  justify-content: center;
}

.uf-content:hover{
  background-color: #f0efef81;
}

.partido-filter{
  text-align: left;
  width: 100%;
  border-radius: 10px;
  color: #1a1919b6;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}


.partido-container{
  height: 10vh;
  overflow: auto;
  font-size: 15px;
  flex-direction: column;
}

.partido-content{
  padding: 5px;
  font-size: 15px;
  justify-content: center;
}

.partido-content:hover{
  background-color: #f0efef81;
}

.font-awesome-search{
  margin: 9.5px 0px;
  font-size: 100%;
}

</style>