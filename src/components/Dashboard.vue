<template>
  <div id="burger-table" v-if="burgers">
    <Message :msg="msg" v-show="msg" />
    <div>
      <div id="burger-table-heading">
        <div class="order-id">#:</div>
        <div>Cliente:</div>
        <div>Pão:</div>
        <div>Carne:</div>
        <div>Opcionais:</div>
        <div>Ações:</div>
      </div>
    </div>
    <div id="burger-table-rows">
      <div class="burger-table-row" v-for="burger in burgers" :key="burger.id"><!--setar um array de dados e a key chave unica-->
        <div class="order-number">{{ burger.id }}</div>
        <div>{{ burger.nome }}</div>
        <div>{{ burger.pao }}</div><!--colocando cada dado da lista de pedidos-->
        <div>{{ burger.carne }}</div>
        <div>
          <ul v-for="(opcional, index) in burger.opcionais" :key="index">
            <li>{{ opcional }}</li>
          </ul>
        </div>
        <div>
          <select name="status" class="status" @change="updateBurger($event, burger.id)"><!--$event, burger.id parametros para monitorar edição-->
            <option :value="s.tipo" v-for="s in status" :key="s.id" :selected="burger.status == s.tipo"><!--status coloca o array de selects com calor s.tipo-->
              {{ s.tipo }}
            </option><!--burger.status == s.tipo : se status for igual ao seu tipo de status vai manter o padrão solicitado primeiro tipo do backend-->
          </select>
          <button class="delete-btn" @click="deleteBurger(burger.id)">Cancelar</button>
        </div>
      </div>
    </div>
  </div>
  <div v-else>
    <h2>Não há pedidos no momento!</h2>
  </div>
</template>
<script>
    import Message from './Message'

  export default {
    name: "Dashboard",

    components: {
      Message
    },
    data() {
      return {
        burgers: null,
        burger_id: null,
        status: [],
        msg: null
      }
    },
    methods: {
      async getPedidos() {
        const req = await fetch('http://localhost:3000/burgers')// chama backend onde armazena dados do pedido client

        const data = await req.json()//colocar no formato json

        this.burgers = data //armazena no parametro burgers os dados 
        console.log('ver os pedidos feitos', this.burgers)
        // Resgata os status de pedidos
        this.getStatus()

      },
      async getStatus() {

        const req = await fetch('http://localhost:3000/status')// backend para puxar os status de pedidos

        const data = await req.json()

        this.status = data //armazena no array status os dados do back status

      },
      async deleteBurger(id) {
        console.log('pega o id unico para deletar aquele pedido ', id)
        const req = await fetch(`http://localhost:3000/burgers/${id}`, {//deleta usando metodo delete lá no banco de dados json setando ${id} dessa maneira
          method: "DELETE"
        });

        const res = await req.json()

        this.msg = `Pedido removido com sucesso!`
        setTimeout(() => this.msg = "", 3000)

        this.getPedidos() // atualiza os pedidos com a remoção anterior executada

      },
      async updateBurger(event, id) {//edição do burguer gerada atraves de @change nhos selects

        const option = event.target.value; // para saber qual o valor ou o item q ele esta usando no momento

        const dataJson = JSON.stringify({status: option});//coloca em string o json status para colocar no banco de dados

        const req = await fetch(`http://localhost:3000/burgers/${id}`, {//faz a requiseção para o banco atravez do id
          method: "PATCH", //patch é como um update edição mas apenas do que enviou no caso status
          headers: { "Content-Type" : "application/json" }, //tipo de conteudo é application/json
          body: dataJson
        });

        const res = await req.json()

        this.msg = `Pedido N: ${res.id} foi atualizado para ${res.status}`
        setTimeout(() => this.msg = "", 3000)

        console.log('ver a edição', res)

      }
    },
    mounted () {
    this.getPedidos() //manter salvo na tela os pedidos
    }
  }
</script>

<style scoped>
  #burger-table {
    max-width: 1200px;/*largura maxima*/
    margin: 0 auto;/*centraliza a tabela no centro da tela*/
  }

  #burger-table-heading,
  #burger-table-rows,
  .burger-table-row {
    display: flex;/*deixa na horizontal a tabela com os dados*/
    flex-wrap: wrap;
  }

  #burger-table-heading {
    font-weight: bold;
    padding: 12px;
    border-bottom: 3px solid #333;
  }

  .burger-table-row {
    width: 100%;
    padding: 12px;
    border-bottom: 1px solid #CCC;
  }

  #burger-table-heading div,
  .burger-table-row div {
    width: 19%; /*divisão das linhas de cada div*/
  }

  #burger-table-heading .order-id,
  .burger-table-row .order-number {
    width: 5%;
  }

  select {
    padding: 12px 6px;
    margin-right: 12px;
  }

  .delete-btn {
    background-color: #222;
    color:#fcba03;
    font-weight: bold;
    border: 2px solid #222;
    padding: 10px;
    font-size: 16px;
    margin: 0 auto;
    cursor: pointer;
    transition: .5s;
  }
  
  .delete-btn:hover {/*passa o mouse e muda a cor com hover*/
    background-color: transparent;
    color: #222;
  }
  
</style>