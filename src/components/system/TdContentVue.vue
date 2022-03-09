<template>
  <tr>
    <td><strong>{{ transaction_id }}</strong></td>
    <td>{{ created_at}}</td>
    <td>{{ net_value }}</td>
    <td>{{ tax_conversion }}</td>
    <td>{{ tax_payment }}</td>
    <td>{{ payment_method_description }}</td>
    <td>{{ payment_method_tax_percentage }}</td>
    <td>{{ value }}</td>
    <td>{{ currency_amount }}</td>
    <td>{{ confirmad_date_at }}</td>
    <td>{{ status }}</td>
    <td>
      <button v-if="!confirmad_date_at"  class="btn red" @click="deleteTransaction(transaction_id, uuid)">Excluir</button>
      <button v-if="!confirmad_date_at" class="btn blue" @click="aproveTransaction(transaction_id, uuid)">Aprovar</button>
    </td>
  </tr>
</template>

<script>
import GridVue from '@/components/layouts/GridVue'

export default {
  name: 'TdContentVue',
  props:[
    'transaction_id',
    'uuid',
    'user_id',
    'user_name',
    'user_cpf_cnpj',
    'confirmad_date_at',
    'created_at',
    'currency_amount',
    'net_value',
    'tax_conversion',
    'tax_payment',
    'payment_method_description',
    'payment_method_tax_percentage',
    'value',
    'status'
    ],

  components:{
    GridVue
  },

  methods: {
    deleteTransaction(transaction_id, uuid) {
      this.$http.delete(
        this.$urlAPI+"transaction?"+
        "id="+transaction_id+
        "&uuid="+uuid,

        {"headers":{"Authorization":"Bearer "+this.$store.getters.getToken}})

        .then(response => {

          if (response.status) {

            alert(response.data.success);

            this.$store.commit('setContents', response.data.data);

          } else {
            alert(response.data.error);
          }
        }).catch(e => {
          alert('Não foi possível excluir essa transação, por favor tente mais tarde')
        }
      );
    },
    aproveTransaction(transaction_id, uuid) {
      this.$http.put(
        this.$urlAPI+"transaction/confirmPayment?"+
        "id="+transaction_id+
        "&uuid="+uuid,
        {},

        {"headers":{"Authorization":"Bearer "+this.$store.getters.getToken}})

        .then(response => {

          if (response.status) {

            alert(response.data.success)

            console.log(response.data.data);

            this.$store.commit('setContents', response.data.data);

          } else {
            alert(response.data.error);
          }
        }).catch(e => {
          alert('Não foi possível aprovar essa transação, por favor tente mais tarde')
        }
      );
    },
  }

}
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
