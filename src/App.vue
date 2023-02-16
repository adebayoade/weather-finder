<script setup>
import { RouterLink, RouterView } from 'vue-router';
import Form from './components/Form.vue';
</script>

<template>
  <div class="container" style="max-width: 1000px">
    <h1 class="mt-5 col-sm-mt-5 text-white">Weather Today</h1>
    <RouterView />

    <p class="mt-5 footer text-center text-white">
      Designed by
      <a class="text-white text-decoration-none" href="http://adebayoade.com">Adebayo Ade</a>
    </p>
  </div>
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;500;700&display=swap');

body {
  /* background: grey; */
  font-family: 'Roboto', sans-serif;
  background-image: url('@/assets/background_weather.jpg');
  background-repeat: no-repeat;
  background-attachment: fixed;
  background-size: cover;


  /* background: linear-gradient(rgba(255, 255, 255, 0), rgba(255, 255, 255, 0)),
    url('@/assets/background_weather.jpg') center center/cover; */
}

@media screen {

}
/* .footer {
  bottom: 0;
  position: absolute;
} */
.footer a:hover {
  text-decoration: underline !important;
}
</style>
