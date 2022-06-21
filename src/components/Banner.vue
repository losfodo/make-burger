<template>
  <div id="main-banner">
    <h1>Make Your Burger</h1><!--titulo-->
  </div>
</template>

<script>
  export default {
    name: "Banner"
  }
</script>

<style scoped>

  #main-banner {
    background-image: url('/img/burger.jpg');/*coloca uma imagem de fundo*/
    background-position: 0 -250px;/*desloca para eixo y*/
    background-size: cover;/*imagem vai aparecer inteira o melhor possivel dela ajustada na tela*/
    height: 500px;
    display: flex;/*posicionamento flex modo edição muda, esquerda para direita*/
    align-items: center;
    justify-content: flex-start;
  }

  #main-banner h1 {
    color: #FFF;
    text-align: center;
    font-size: 60px;
    background-color: #222;/*fundo do banner*/
    padding: 20px 40px;/*não ficar grudado a esquerda da tela*/
  }

</style>