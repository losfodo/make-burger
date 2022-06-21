<template>
  <Message :msg="msg" v-show="msg" /><!--importa mensagem de sucesso e envia msg com mensagem -->
  <div>   
    <form id="burger-form" method="POST" @submit="createBurger"><!--gerar o evento submit de um formulario-->
      <div class="input-container">
        <label for="nome">Nome do cliente:</label>
        <input type="text" id="nome" name="nome" v-model="nome" placeholder="Digite o seu nome">
      </div>
      <div class="input-container">
        <label for="pao">Escolha o pão:</label>
        <select name="pao" id="pao" v-model="pao">
          <option value="">Selecione o seu pão</option>
          <option v-for="pao in paes" :key="pao.id" :value="pao.tipo">{{ pao.tipo }}</option><!--coloca logica certinha v-select option-->
        </select>
      </div>
      <div class="input-container">
        <label for="carne">Escolha a carne do seu Burger:</label>
        <select name="carne" id="carne" v-model="carne">
          <option value="">Selecione o tipo de carne</option>
          <option v-for="carne in carnes" :key="carne.id" :value="carne.tipo">{{ carne.tipo }}</option><!--tipo sempre o valor e id seria id unico-->
        </select>
      </div>
      <div id="opcionais-container" class="input-container">
        <label id="opcionais-title" for="opcionais">Selecione os opcionais:</label>
        <div class="checkbox-container" v-for="opcional in opcionaisdata" :key="opcional.id">
          <input type="checkbox" name="opcionais" v-model="opcionais" :value="opcional.tipo">
          <span>{{ opcional.tipo }}</span>
        </div>
      </div>
      <div class="input-container">
        <input class="submit-btn" type="submit" value="Criar meu Burger!">
      </div>
    </form>
  </div>
</template>

<script>
import Message from './Message'

export default {
  name: "BurgerForm",
  data() {
    return {
      paes: null,// dados iniciais
      carnes: null,
      opcionaisdata: null,
      nome: null,
      pao: null,
      carne: null,
      opcionais: [],//array tem mais de um
      //status: "Solicitado", //status inicia solicitado
      msg: null
    }
  },
  methods: {
    async getIngredientes() {
      const req = await fetch('http://localhost:3000/ingredientes')//A interface Fetch API permite que o navegador da Web faça solicitações HTTP para servidores web.
      const data = await req.json()//pegar os dados pelo formato dele nmo caso json
      //console.log('data dados dos ingredientes se estiver no created ou mounted', data)
      this.paes = data.paes
      this.carnes = data.carnes//pegando dados de cada um separadamente
      this.opcionaisdata = data.opcionais // data return opcionaisdata = data.opcionais de db.json
    },
    async createBurger(e) {// metodo função que tera metodo post que sera enviado para o backend os dados do pedido

      e.preventDefault()// para o evento submit pare de ser executado
      console.log('criou o hamburger mostra no console funcionamento do submit')
      const data = {// const como todos os dados que quer enviar para metodo post backend do pedido
        nome: this.nome,
        carne: this.carne,
        pao: this.pao,
        opcionais: Array.from(this.opcionais),// como é um tem q usar array.from pra funcionar
        status: "Solicitado"
      }
      console.log('mostrar os dados que serão enviados', data)
      const dataJson = JSON.stringify(data)// em formato json coloca stringify: transformando dado em texto a data  

      const req = await fetch("http://localhost:3000/burgers", {//fetch:usa api do js puro /burgers array que sera armazenado os dados gerais
        method: "POST", //metodo salva os dados no backend
        headers: { "Content-Type" : "application/json" },// headers avisa que esta se comunicando com o json
        body: dataJson // envia os dados para corpo da requisição com body recebe const dataJson
      });

      const res = await req.json()

      console.log('ver a resposta do dado post enviado', res)

      this.msg = `Pedido N: ${res.id} realizado com sucesso!`

      // clear message
      setTimeout(() => this.msg = "", 3000)// mensagem desaparece dps de 3s

      // limpar campos
      this.nome = ""
      this.carne = ""
      this.pao = ""
      this.opcionais = []
      
    }
  },
  mounted () {//deixar ja montado os dados pego
    this.getIngredientes()
  },
  components: {
    Message
  }
}
</script>

<style scoped>
  #burger-form { /*formulario geral id*/
    max-width: 400px;
    margin: 0 auto;
  }

  .input-container {/*class*/
    display: flex;
    flex-direction: column;
    margin-bottom: 20px;
  }

  label {/*pega todas a label para mudar css*/
    font-weight: bold;
    margin-bottom: 15px;
    color: #222;;
    padding: 5px 10px;
    border-left: 4px solid #fcba03; /*borda a esquerda laranja amarelada dos campos de label*/
  }

  input, select {
    padding: 5px 10px;
    width: 300px;
  }

  #opcionais-container {
    flex-direction: row;
    flex-wrap: wrap;/*opçoes de checkbox ficar um abaixo do outro*/
  }

  #opcionais-title {
    width: 100%;
  }

  .checkbox-container {
    display: flex;
    align-items: flex-start;/*depois q passa o 100% da largura checkbox vai para baixo a esquerda*/
    width: 50%;
    margin-bottom: 20px;
  }

  .checkbox-container span,
  .checkbox-container input {
    width: auto;
  }

  .checkbox-container span {
    margin-left: 6px;
    font-weight: bold;
  }

  .submit-btn {
    background-color: #222;
    color:#fcba03;
    font-weight: bold;
    border: 2px solid #222;
    padding: 10px;
    font-size: 16px;
    margin: 0 auto;
    cursor: pointer;/*tipo do cursor do mouse*/
    transition: .5s;
  }

  .submit-btn:hover {
    background-color: transparent;
    color: #222;
  }
</style>