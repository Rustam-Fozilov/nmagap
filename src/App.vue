<template>
  <the-navbar></the-navbar>
  <div class="container">
    <router-view></router-view>
  </div>
</template>

<script>
import TheNavbar from "@/components/TheNavbar";

export default {
  name: 'App',
  components: {TheNavbar},
}
</script>

<style>
@font-face {
  font-family: Montserrat-Regular;
  src: url("@/assets/fonts/Montserrat-Regular.ttf");
}

@font-face {
  font-family: Montserrat-SemiBold;
  src: url("@/assets/fonts/Montserrat-SemiBold.ttf");
}

html, body {
  margin: 0;
  padding: 0;
  font-size: 18px;
  background-color: #f2f2f5;
}

.container {
  width: 720px;
  margin: 0 auto;
}

.active {
  text-decoration: underline !important;
}

@media only screen and (max-width: 800px) {
  .container {
    width: 90%;
  }
}
</style>
