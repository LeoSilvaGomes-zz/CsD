<template>
  <div class="container">
    <Sidebar 
      @action="idDeputado" 
      :style="telaSidebar"
    />
    <DeputadoView 
      :id="idDeputados"
      :style="telaDashboard"
      @action="idDeputado"
    />
    <LandingPage
      v-if="idDeputados == ''"
      :style="telaDashboard"
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
      telaDashboard: '',
      idDeputados: ''
    };
  },
  created() {
    window.addEventListener('resize', this.handleResize)
    this.handleResize();
    if(this.window.width < 1000){
      this.telaDashboard = 'display:none;'
      this.telaSidebar = ''
    }
    else {
      this.telaDashboard = ''
      this.telaSidebar = ''
    }
  },
  destroyed() {
    window.removeEventListener('resize', this.handleResize)
  },
  methods: {
    handleResize() {
      this.window.width = window.innerWidth;
      this.window.height = window.innerHeight;
    },
    idDeputado(value) {
      this.idDeputados = String(value);
      if(this.idDeputados == '' && this.window.width < 1000){
        this.telaSidebar = ''
        this.telaDashboard = 'display:none;'
      }
      else if (this.idDeputados != '' && this.window.width < 1000){
        this.telaDashboard = ''
        this.telaSidebar = 'display:none;'
        console.log("leo")
      }
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