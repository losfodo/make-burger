<template>
  <div>
    <div id="nav">
      <router-link id="logo-url" to="/">
        <img id="logo" :src="logo" :alt="alt"><!--logo via props-->
      </router-link>
      <router-link to="/">Fazer Pedido</router-link><!--rotas que ficam no top bar-->
      <router-link to="/pedidos">Pedidos</router-link>
    </div>
  </div>
</template>

<script>

export default {
  name: "Navbar",
  props: ["logo", "alt"] //recebe por props a logo e :alt="app_name" lá em src/App.vue
}
</script>

<style scoped>
  #nav {
    background-color: #222; /*cor de fundo preta*/
    border-bottom: 4px solid #111; /*borda solida grossura preta*/
    padding: 15px 50px;/*altura padding 15, distanciamento dos itens 50*/
    display: flex;/*modo flex formato primeiro a setar*/
    justify-content: flex-end;/*deixa a direita tudo */
    align-items: center;/*alinha no centro pela vertical*/
  }

  #nav #logo-url { /**logo id para ajustar apenas logo em especifico */
    margin: auto;
    margin-left: 0;
  }

  #logo {
    width: 40px;
    height: 40px;
  }

  #nav a {
    color: #fcba03; /*cor dos botoes*/
    text-decoration: none;/*linha padrão remove*/
    margin: 12px;
    transition: .5s;/*transação de meio segundo o nav a:hover*/
  }

  #nav a:hover {
    color: #FFF;/*deixando branco ao passar o mouse*/
  }
</style>