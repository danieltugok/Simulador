<template>
  <div class="container" v-on:sextou="gelo()">

      <div class="col-md-8 offset-md-2 ">
          <h2>Resultado</h2>

            <a href="/" class="btn btn-primary">SIMULAR NOVAMENTE</a>
    </div>
  </div>
</template>

<script>
import Dados from '../../domain/Dados'
export default {

    data() {
        return {
 dados : new Dados() 
        }
    },

    methods: {

        gelo(){
             this.$http.post('http://api.mathjs.org/v4/', { "expr": `${this.dados.mensal} * (((1 + 0.00517) ^ ${this.dados.tempo} - 1) / 0.00517)`, 'precision': 5 })
            
                      .then(response => {
                
                        console.log(response.body.result)
                        console.log(this.dados.nome)
                        console.log(this.dados.mensal)
                        console.log(this.dados.tempo)
    
                   
                    });
        }
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