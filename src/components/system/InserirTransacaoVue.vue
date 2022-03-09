<template>
  <div class="row">
    <grid-vue class="input-field" tamanho="12">
      <input type="text"    v-model="data.net_value"         placeholder="Insira um valor colocando a virgula">
      <input type="number"  v-model="data.currency_id"       placeholder="Insira o numero 1 para USD e 2 para CAD e 3 para euro">
      <input type="number"  v-model="data.payment_method_id" placeholder="Insira o numero 1 para boleto e 2 para cartão">
    </grid-vue>
    <p class="right-align">
      <button v-on:click="addTransaction()" class="btn waves-effect waves-light">Comprar</button>
    </p>
  </div>
</template>

<script>
import GridVue from '@/components/layouts/GridVue'
export default {
  name: 'InserirTransacaoVue',
  props:[],
  data () {
    return {
      data: {
        currency_id: '',
        net_value: '',
        payment_method_id: '',
      }
    }
  },
  components: {
    GridVue
  },
  methods: {

    addTransaction() {

      this.$http.post(this.$urlAPI+"transaction?",
      {
        payment_method_id: this.data.payment_method_id,
        net_value: this.data.net_value.toString().replace(",", "."),
        currency_id: this.data.currency_id,
      },
      {"headers": {"Authorization":"Bearer "+this.$store.getters.getToken}})
      .then(response => {
        if (response.data.success) {
          alert(response.data.success);
          this.$store.commit('setContents', response.data.data)
        } else {
          alert(response.data.error);
        }
      }).catch(e => {
        alert('Não foi possível inserir a transação, por favor Tente novamente mais tarde!')
      }
      )
    },
  },
}


$(document).ready(function() {
     $('select').material_select();
});

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
