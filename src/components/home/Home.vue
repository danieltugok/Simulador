<template>
  <div class="container">

      <div class="col-md-8 offset-md-2 ">
          <h2>Simulador</h2>
        <form @submit.prevent="simulador()">
            
                <div class="form-group row">
                    <label class="col-4">Nome</label>
                    <input type="text" class="form-control col-8" v-model="dados.nome">
                </div>
                <div class="form-group row">
                    <label class="col-4">Mensalidade</label>
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

            <button type="submit" class="btn btn-primary">SIMULAR</button>
    
        </form>
    </div>
  </div>
</template>

<script>
import Dados from '../../domain/Dados'

export default {

    data(){
        return {
           
           dados : new Dados()                                 
            
        }
    },

    methods: {

        simulador(){

            this.$http.post('http://api.mathjs.org/v4/', { "expr": `${this.dados.mensal} * (((1 + 0.00517) ^ ${this.dados.tempo} - 1) / 0.00517)`, 'precision': 5 })
            
                      .then(response => {
                
                        console.log(response.body.result)
                        console.log(this.dados.nome)
                        console.log(this.dados.mensal)
                        console.log(this.dados.tempo)
    
                   
                    });


                        // window.open('/resultado', '_self'); 
         
        },
    },
   
}
</script>

<style scoped>
    h2{
        margin: 20px 0;

    }
    
    button {
        float: right;
    }
</style>