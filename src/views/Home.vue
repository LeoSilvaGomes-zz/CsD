<template>
  <div class="container">
    <Sidebar />
  </div>
</template>

<script>
import axios from 'axios'
import Sidebar from '../components/Sidebar'

export default {
  components:{
    Sidebar,
  },
  created() {
    this.request();
  },
  methods: {
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

</style>