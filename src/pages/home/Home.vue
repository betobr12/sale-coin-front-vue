<template>
  <site-template>
    <span slot="principal">
      <inserir-transacao-vue />

      <table class="striped responsive-table">
          <thead>
            <tr>
              <th scope="col">ID</th>
              <th scope="col">Data de Criação</th>
              <th scope="col">Valor Pago</th>
              <th scope="col">Taxa de conversão</th>
              <th scope="col">Taxa de Pgto</th>
              <th scope="col">Forma Pgto</th>
              <th scope="col">%</th>
              <th scope="col">Valor Compra/Modeda</th>
              <th scope="col">Qtde Moeda</th>
              <th scope="col">Data Confirmação</th>
              <th scope="col">Status</th>
              <th scope="col">Opções</th>
            </tr>
        </thead>
        <tbody>
            <td-content-vue v-for="item in listaConteudos" :key="item.id"
                :transaction_id="item.id"
                :uuid="item.uuid"
                :user_name="item.user_name"
                :user_id="item.user_id"
                :user_cpf_cnpj="item.user_cpf_cnpj"
                :confirmad_date_at="item.confirmad_date_at"
                :created_at="item.created_at"
                :currency_amount="item.currency_amount"
                :net_value="item.net_value"
                :tax_conversion="item.tax_conversion"
                :tax_payment="item.tax_payment"
                :payment_method_description="item.payment_method_description"
                :payment_method_tax_percentage="item.payment_method_tax_percentage"
                :value="item.value"
                :status="item.status"
            >
            </td-content-vue>
        </tbody>
      </table>
    </span>
  </site-template>
</template>

<script>
import TdContentVue from '@/components/system/TdContentVue'
import InserirTransacaoVue from '@/components/system/InserirTransacaoVue'
import SiteTemplate from '@/templates/SiteTemplate'
import GridVue from '@/components/layouts/GridVue'

export default {
  name: 'Home',

  created() {
    this.updatePage();
    this.updateCurrent();
  },

  components:{
    InserirTransacaoVue,
    SiteTemplate,
    GridVue,
    TdContentVue,
  },

  watch:{
    '$route':'updatePage',
  },

  methods: {

    updatePage() {

      let usuarioAux = this.$store.getters.getUsuario;

      if (usuarioAux) {

        this.user = this.$store.getters.getUsuario;
        this.$http.get(
          this.$urlAPI+'transaction'+
          '?user_id='+this.user.id+
          '&onlyActive=1',
          {"headers":{"authorization":"Bearer "+this.$store.getters.getToken}}
        )
        .then(response => {
          if (response.data) {
            this.$store.commit('setContents',response.data);
          }
        })
        .catch(e => {
          alert('Não foi possível localizar as transações, por favor, tente novamente mais tarde')
        })

      }
    },

    updateCurrent() {

    },

    format_date(date){
      if (date) {
        return moment(String(date)).format('YYYYMMDD')
      }
    },
  },

  computed: {
    listaConteudos() {
     return this.$store.getters.getContentsTimeLine;
    }
  }
}
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
