<template>
  <div>
    <Navbar :logo="logo_src" :alt="app_name" /><!--barra de navegação importada, envia logo via props-->
    <div><!--Navbar onde vai ter botoes de clicks com router-view encaminhados-->
      <router-view></router-view><!--rotas app principal do router/index.js do router.vue-->
    </div>
    <Footer /><!--parte que fica fora do router view ela se mantem não importa a mudança de rotas-->
  </div>
</template>
<script>
import Navbar from './components/Navbar'
import Footer from './components/Footer'

export default {
  data: function() {
    return {
      logo_src: "/img/logo.png", //passa o caminho da imagem.png
      app_name: "Make Your Burger"
    }
  },
  components: {
    Navbar,
    Footer
  }
}
</script>

<style>
  * {/*projeto num todo*/
    font-family: Helvetica;
    padding: 0;
    margin: 0;
    box-sizing: border-box;/*não passar da largura maxima */
  }

  .main-container {/*onde tiver class com esse nomeamento pegaria em geral*/
    margin: 50px;
    min-height: 250px;/*altira minima joga footer para baixo*/
  }

  h1 {
    text-align: center;
    font-size: 42px;
    margin-bottom: 30px;
    color: #222;
  }
</style>
