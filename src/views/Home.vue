<template>
  <div>
    <Banner /><!--titulo principal-->
    <div class="main-container"><!--main-container css editado no app.vue-->
      <h1>Monte o seu burger:</h1>
      <BurgerForm />
    </div>
  </div>
</template>

<script>
import Banner from '../components/Banner'
import BurgerForm from '../components/BurgerForm'

export default {
  name: 'Home',
  components: {//importa componentiza e coloca no html
    Banner,
    BurgerForm
  }
}
</script>