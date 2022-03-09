<template>
  <site-template>
    <span slot="principal">
        <h2>Perfil</h2>
        <input type="text" placeholder="Nome" v-model="name">
        <input type="text" placeholder="E-mail" v-model="email">
        <input type="password" placeholder="Senha" v-model="password">
        <input type="password" placeholder="Confirme sua Senha" v-model="password_confirmation">
        <button type="button" class="btn" v-on:click="userUpdate()">Atualizar</button>
    </span>
  </site-template>
</template>

<script>
import SiteTemplate from '@/templates/SiteTemplate';
export default {
  name: 'Perfil',
  data () {
    return {
      user:false,
      name:'',
      email:'',
      password:'',
      password_confirmation:''
    }
  },

  created() {
    let usuarioAux = this.$store.getters.getUsuario;
    if(usuarioAux){
      this.user = this.$store.getters.getUsuario;
      this.name = this.user.name;
      this.email = this.user.email;
      this.description = this.user.description;
    }
  },

  components: {
    SiteTemplate
  },

  methods: {
    sair() {
      sessionStorage.clear();
      this.user = false
    },

    userUpdate() {
      this.$http.put(this.$urlAPI+'user', {
        name: this.name,
        email: this.email,
        password: this.password,
        password_confirmation: this.password_confirmation
      },
      {"headers":{"authorization":"Bearer "+this.$store.getters.getToken}})
      .then(response => {
          if (response.data.success) {

            this.user = response.data.user;
            this.$store.commit('setUsuario',response.data.user);
            sessionStorage.setItem('user',JSON.stringify(response.data.user))

            alert(response.data.success);

          } else {

            let erros = '';
            for (let erro of Object.values(response.data.error)) {
              erros += erro +" ";
            }
            alert(erros);
          }
        }
      )
      .catch(e => {
        console.log(e)
        alert('Não foi possível atualizar o seu cadastro, por favor, tente novamente mais tarde!')
        }
      )
    }

  }
}
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
