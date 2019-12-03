<template>
  <div class="container">
    <Sidebar 
      @action="idDeputado" 
      :style="telaSidebar"
    />
    <DeputadoView 
      :id="idDeputados"
    />
    <LandingPage
      v-if="idDeputados == ''"
    />
  </div>
</template>

<script>
import axios from "axios";
import Sidebar from "../components/web/Sidebar";
import DeputadoView from "../components/web/DeputadoView";
import LandingPage from "../components/web/LandingPage";

export default {
  components: {
    Sidebar,
    LandingPage,
    DeputadoView
  },
  created() {
    this.request();
  },
  data() {
    return {
      window: {
        width: 0,
        height: 0
      },
      telaSidebar: '',
      idDeputados: ''
    };
  },
  created() {
    window.addEventListener('resize', this.handleResize)
    this.handleResize();
  },
  destroyed() {
    window.removeEventListener('resize', this.handleResize)
  },
  methods: {
    handleResize() {
      this.window.width = window.innerWidth;
    },
    idDeputado(value) {
      if(value){
        console.log(value)
      }
      else {
        console.log("leo")
      }
      this.idDeputados = String(value);
    },
    request() {
      axios
        .get("https://dadosabertos.camara.leg.br/api/v2/deputados")
        .then(response => {
          console.log(response.data);
        })
        .catch(error => {
          console.log(error.response);
        });
    }
  }
};
</script>

<style>
.container {
  display: flex;
}



</style>