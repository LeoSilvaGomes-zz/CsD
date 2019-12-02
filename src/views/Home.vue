<template>
  <div class="container">
    <Sidebar 
      @action="idDeputado"
    />
    <DeputadoView 
      :id="idDeputados"
    />
  </div>
</template>

<script>
import axios from 'axios'
import Sidebar from '../components/Sidebar'
import DeputadoView from '../components/DeputadoView'

export default {
  components:{
    Sidebar,
    DeputadoView,
  },
  created() {
    this.request();
  },
  data() {
    return {
      idDeputados: '',
    }
  },
  methods: {
    idDeputado (value) {
      console.log(String(value))
      this.idDeputados = String(value)
    },
    request(){
      axios
        .get("https://dadosabertos.camara.leg.br/api/v2/deputados")
        .then(response => {
          console.log(response.data)
        })
        .catch(error => {
          console.log(error.response)
        })
    }
  }

}
</script>

<style>

.container {
  display: flex;
}

</style>