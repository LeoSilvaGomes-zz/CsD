<template>
  <div class="dashboard-page">
    <div class="content-dashboard">
      <div class="content-title">
        <p style="margin-bottom: 3%; margin-top: 0px; ">Conheça</p>seus Deputados!
      </div>
      <div class="container-dashboard">
        <div class="content-button" @click="action">Começar</div>
        <div class="content-animation"></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  methods: {
    action() {
      this.$router.push({ name: "dashboard" });
    }
  }
};
</script>

<style>
.dashboard-page {
  height: 100vh;
  background-image: linear-gradient(180deg, #2f7958, #0c5e3d);
}

.container-dashboard {
  display: flex;
  flex-direction: row;
  margin-bottom: 5%;
}

.content-animation {
  width: 140%;
}

.content-button {
  cursor: pointer;
  text-align: justify;
  padding: 8px 12px;
  margin-left: 5%;
  border-radius: 10px;
  background-color: white;
  color: #0c5e3d;
  font-size: 145%;
  font-weight: bold;
}

.content-button:hover {
  background-color: #f0efef;
}

.content-dashboard {
  width: 100%;
  height:100% ;
  display: flex;
  flex-direction: column;
  justify-content: space-around;
}

.content-title {
  padding-left: 5%;
  text-align: initial;
  color: white;
  font-size: 400%;
  font-weight: bold;
}




/*--------------------------------------*/


@media screen and (max-width: 1000px){
  .content-title{
    font-size: 300%;
  }
}  


</style>