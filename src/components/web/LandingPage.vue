<template>
  <div class="content-landing">
    <div class="content-title">
      <p style="margin-bottom: 3%;">Conheça</p>seus Deputados!
    </div>
    <div class="container-landing">
      <div class="content-animation"></div>
      <div class="content-text">
        <p>Esse é um portal transparência sobre as atividades de deputados! Aqui você pode ficar sabendo sobre despesas, orgãos que faz parte, eventos que participou ou irá participar, discursos relevantes e frentes que tem levantado</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {};
</script>

<style>

.container-landing {
    display: flex;
    flex-direction: row;
    margin-top: 20%;
}

.content-animation {
    width: 140%;
}

.content-text {
    width: 100%;
    text-align: justify;
    font-size: 80%;
    padding-right: 5%;
}

.content-landing {
  width: 100%;
  padding-right: 10%;
  padding-top: 3%;
}

.content-title {
  text-align: end;
  color: #0c5e3d;
  font-size: 400%;
  font-weight: bold;
}
</style>