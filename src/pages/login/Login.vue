<template>
  <login-template>
    <span slot="principal">
      <h2>Acessar</h2>
      <input type="text" placeholder="CPF ou CNPJ" v-model="cpf_cnpj">
      <input type="password" placeholder="Senha" v-model="password">
      <button class="btn" v-on:click="login()">Entrar</button>
      <router-link class="btn orange" to="/cadastro">Cadastre-se</router-link>
    </span>
  </login-template>
</template>
<script>
import LoginTemplate from '@/templates/LoginTemplate'
export default {
  name: 'Login',
  data () {
    return {
      cpf_cnpj:'',
      password:''
    }
  },
  components: {
    LoginTemplate
  },
  methods: {

    login() {

      this.$http.post(this.$urlAPI+'login', {
        cpf_cnpj: this.cpf_cnpj,
        password:this.password
      })

      .then(response => {

        if(response.data.success) {

          console.log('login com sucesso');

          console.log(response.data.data);

          this.$store.commit('setUsuario',response.data.data);

          sessionStorage.setItem('user',JSON.stringify(response.data.data));

          this.$router.push('/');

        } else if (response.data.status == false) {

          alert('Login invalido!');

        } else {

          console.log('erros de validação');
          let erros ='';

          for (let erro of Object.values(response.data.error)) {
            erros += erro +" ";
          }

          alert(erros);
        }
      })

      .catch(e => {
        alert("Não foi possível acessar, por favor tente novamente mais tarde");
      })
    }
  }
}
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
