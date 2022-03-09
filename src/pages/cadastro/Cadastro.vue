<template>
  <login-template>
    <span slot="principal">
        <h2>Cadastre-se</h2>
        <input type="text" placeholder="Nome" v-model="name">
        <input type="text" placeholder="E-mail" v-model="email">
        <input type="text" placeholder="CPF/CNPJ" v-model="cpf_cnpj">
        <input type="password" placeholder="Senha" v-model="password">
        <input type="password" placeholder="Confirme sua Senha" v-model="password_confirmation">
        <button type="button" class="btn" v-on:click="register()">Enviar</button>
        <router-link  class="btn orange" to="/login">JÁ TENHO UMA CONTA</router-link >
    </span>
  </login-template>
</template>
<script>
import LoginTemplate from '@/templates/LoginTemplate'
export default {
  name: 'Cadastro',
  data() {
    return {
      name: '',
      email: '',
      cpf_cnpj: '',
      password: '',
      password_confirmation: ''
    }
  },
  components: {
    LoginTemplate
  },
  methods: {
    register() {
      this.$http.post(this.$urlAPI+'register', {
        name: this.name,
        email: this.email,
        cpf_cnpj: this.cpf_cnpj,
        password: this.password,
        password_confirmation: this.password_confirmation
      })
      .then(response => {

        if (response.data.success) {

          this.$store.commit('setUsuario',response.data.user);
          sessionStorage.setItem('user',JSON.stringify(response.data.user))
          alert(response.data.success);
          this.$router.push('/')

        } else {
          alert(response.data.error);
        }
      })
      .catch(e => {
        console.log(e)
        alert("Falha! Tente novamente mais tarde")
      })
    }
  }
}
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
