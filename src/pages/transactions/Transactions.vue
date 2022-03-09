<template>
  <transactions-template>
    <span slot="principal">
    </span>
  </transactions-template>
</template>

<script>
import DashTemplate from '@/templates/DashTemplate'
import GridVue from '@/components/layouts/GridVue'

export default {
  name: 'Transactions',
  data () {
    return {}
  },

  components:{
    DashTemplate,
    GridVue
  },
}
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
