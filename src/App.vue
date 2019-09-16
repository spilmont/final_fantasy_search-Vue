<template>
  <div id="app">
    <header><h1>Final Fantasy XIV search</h1></header>
<personnage></personnage>
  </div>
</template>

<script>

import Personnage from "./components/Personnage/Personnage"

export default {
  name: 'app',
  components: {
  Personnage
  }
}
</script>

<style>
  header{
    font-size: 28px;
    text-align: center;
  }
  body{
    background-image: url("./assets/textures/background.png");
    color: wheat ;
  }




</style>
