<template>
  <div>
    <Message :msg="msg" v-show="msg"/>

    <div>
      <form id="burger-form" @submit="createBurger($event)">

        <!-- Input client name -->
        <div class="input-container">
          <label for="nome">Client Name</label>
          <input type="text" name="nome" id="nome" v-model="nome" placeholder="Digite seu nome" required>
        </div>

        <!-- Input bread -->
        <div class="input-container">
          <label for="pao">Escolha o pão:</label>
          <select name="pao" id="pao" v-model="pao" required>
            <option :value="null" disabled hidden>Selecione o seu pão</option>
            <option v-for="pao in paes" :key="pao.id" :value="pao.tipo">{{ pao.tipo }}</option>
          </select>
        </div>

        <!-- Input meat -->
        <div class="input-container">
          <label for="carne">Escolha a carne do seu pão:</label>
          <select name="carne" id="carne" v-model="carne" required>
            <option :value="null" disabled hidden>Selecione o tipo de carne</option>
            <option v-for="carne in carnes"  :key="carne.id" :value="carne.tipo">{{ carne.tipo }}</option>
            
          </select>
        </div>

        <!-- Input opt -->
        <div id="opcionais-container" class="input-container">
          <label id="opcionais-title" for="opicionais">Selecione os opcionais:</label>
          <div class="checkbox-container" v-for="opcional in opcionaisData" :key="opcional.id">
            <label>
              <input type="checkbox" name="opcionais" v-model="opcionais" :value="opcional.tipo">
              <span>{{ opcional.tipo }}</span>
            </label>
          </div>          
        </div>

        <div class="input-container">
          <input type="submit" class="submit-btn" value="Criar meu burger">
        </div>

      </form>
    </div>
  </div>
</template>

<script>

  import Message from './message.vue';

  export default{
    name: 'BurgerForm',
    data(){
      return{
        paes: null,
        carnes: null,
        opcionaisData: null,
        nome: null,
        pao: null,
        carne: null,
        opcionais: [],
        msg: null
      }
    },
    components:{
      Message
    },
    methods:{
      async getIngredientes(){
        const req = await fetch('http://localhost:3000/ingredientes')
        const data = await req.json();

        this.paes = data.paes
        this.carnes = data.carnes
        this.opcionaisData = data.opcionais
      },
      async createBurger(e){
        e.preventDefault();
        
        const data = {
          nome: this.nome,
          carne: this.carne,
          pao: this.pao,
          opcionais: Array.from(this.opcionais),
          status: "Solicitado",
        }

        const dataJson = JSON.stringify(data)

        const req = await fetch("http://localhost:3000/burgers", {
          method: "POST",
          headers: { "Content-type": "application/json"},
          body: dataJson
        });

        const res = await req.json();

        //Imprimindo mensagem do sistema
        this.msg = `Pedido Nº ${res.id} realizado com sucesso!`;

        //Limpar mensagem
        setTimeout(() => {
          this.msg = null
        },3000)


        //Limpar campos do formulário
        this.nome = '';
        this.pao = '';
        this.carne = '';
        this.opcionais = [];
      }
    },
    mounted() {
      this.getIngredientes();
    }
  }
</script>

<style scoped>
  #burger-form{
    max-width: 400px;
    margin: 0 auto;
  }

  .input-container{
    display: flex;
    flex-direction: column;
    margin-bottom: 20px;
  }

  label {
    font-weight: bold;
    margin-bottom: 15px;
    color: #222;
    padding: 5px 10px;
    border-left: 4px solid #FCBA03;
  }

  input, select{
    padding: 5px 10px;
    width: 100%;
  }

  #opcionais-container{
    flex-direction: row;
    flex-wrap: wrap;
  }

  #opcionais-title {
    width: 100%;
  }

  .checkbox-container{
    display: flex;
    align-items: flex-start;
    width: 45%;
    margin: 0 2.5% 20px 2.5%;
  }

  .checkbox-container label{
    cursor: pointer;
    display: flex;
    width: 100%;
    background-color: #ececec;
    font-size: 18px;
    padding: .5em;
  }

  .checkbox-container span,
  .checkbox-container input{
    max-width: 100%;
    width: auto;
  }

  .checkbox-container span{
    margin-left: 6px;
    font-weight: bold;
  }

  .submit-btn {
    background-color: #222;
    color: #FCBA03;
    font-weight: bold;
    border: 2px solid #222;
    padding: 10px;
    font-size: 16px;
    margin: 0 auto;
    cursor: pointer;
    transition: .5s;
  }

  .submit-btn:hover{
    background: transparent;
    color: #222;
  }

</style>
