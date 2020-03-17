<template>
  <div class="container">
        <div class="col-md-8 offset-md-2 card" v-show="!resultado">
            <div class="card-body">
                
                <h2>Simulador</h2>
                    <form @submit.prevent="simulador()">
                        
                            <div class="form-group row">
                                <label class="col-4">Nome</label>
                                <input type="text" class="form-control col-8" v-model="dados.nome">
                            </div>
                            <div class="form-group input-group  row">
                                <label class="col-4">Mensalidade</label>
                                <div class="input-group-prepend">
                                    <div class="input-group-text">R$</div>
                                </div>
                                <input type="number" class="form-control col-8" v-model="dados.mensal">
                            </div>
                    
                            <div class="form-group row">
                                <label class="col-4">Tempo</label>
                                <select class="form-control col-8" v-model="dados.tempo">
                                    <option value="12">1 ano</option>
                                    <option value="24">2 anos</option>
                                    <option value="36">3 anos</option>
                                    <option value="48">4 anos</option>
                                    <option value="60">5 anos</option>
                                    <option value="72">6 anos</option>
                                    <option value="84">7 anos</option>
                                    <option value="96">8 anos</option>
                                    <option value="108">9 anos</option>
                                    <option value="120">10 anos</option>
                                </select>
                            </div>

                        <button type="submit" class="btn col-md-6 col-md-offset-3" 
                            @click="resultado=!resultado"
                            :disabled="!verificaForm()"
                            >SIMULAR</button>
                
                    </form>
            </div>
        </div>

        <div class="col-md-8 offset-md-2 card" v-show="resultado">
            <div class="card-body">
                <p>Olá {{dados.nome}}, Juntando R${{ dados.mensal.replace('.', ',').replace(/(\d)(?=(\d{3})+(?!\d))/g, '$1.') }} todo mês, você terá R${{valor.replace('.', ',').replace(/(\d)(?=(\d{3})+(?!\d))/g, '$1.')}}
                    em <span v-if="(dados.tempo / 12) === 1">{{dados.tempo / 12}} ano</span> <span v-else> {{ dados.tempo / 12 }} anos</span>
                </p>
                <button type="submit" class="btn col-md-6 col-md-offset-3" @click="resultado=!resultado, limpa()">SIMULAR NOVAMENTE</button>
            </div>
            
        </div>
    </div>

</template>

<script>
import Dados from '../../domain/Dados'

export default {

    data(){
        return {
           resultado: false,
           valor: '',
           dados : new Dados()                                       
            
        }
    },

    methods: {

        verificaForm () {
            return !!this.dados.nome && !!this.dados.mensal && !!this.dados.tempo;
            
        },

        simulador(){
           
                this.$http.post('http://api.mathjs.org/v4/', { "expr": `${this.dados.mensal} * (((1 + 0.00517) ^ ${this.dados.tempo} - 1) / 0.00517)`, 'precision': 5 })
                
                          .then(response => {
                              
                            if(response.status === 200){                         
                                this.valor = response.body.result;
                            }                                
                       
                        });                 
        },

         limpa(){
             
            this.valor = '',            
            this.dados = new Dados()
                        
         }         
    },
   
}
</script>

<style scoped>
    h2 { margin: 20px 0; 
         text-align: center; 
         color: #fff; 
         background: #ff3796; 
         padding: 15px 0; 
         border-radius:50px}
    
    button { 
         float: right; 
         border-radius: 15px; 
         padding: 7px 0px; 
         border: 2px solid #ff3796; 
         background: #ff3796; 
         color: #fff;}

    .btn:disabled {
        opacity: 0.15;}

    button:hover { 
         background: #ff3796; 
         border-color:#ff3796; 
         color: #fff;}

    .input-group-text{
         border-radius: 11px 0 0 11px !important;
         margin-left: 8px;}     

    .card { 
         margin-top: 10vh; 
         float: right;     
         border-radius: 20px;}

    .form-control{ 
         border-radius: 11px; 
         padding: 10px;}

    @media screen and (max-width: 600px) {
        .input-group-text{
            height: 38px;}

        h2{ padding: 5px 0;
            font-size:25px} 
    }     
    
</style>