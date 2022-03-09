<template>
  <span>
    <header>
      <nav-bar-vue logo="Quality" url="/" cor="black">
          <li v-if="!user"><router-link to="/login">Entrar</router-link></li>
          <li v-if="!user"><router-link to="/cadastro">Cadastre-se</router-link></li>
          <li v-if="user"><router-link to="/perfil">{{user.name}}</router-link></li>
          <li v-if="user"><a v-on:click="sair()">Sair</a></li>
      </nav-bar-vue>
    </header>
    <main>
      <div class="container">
        <div class="row">
          <grid-vue tamanho="6">
            <slot name="principal" />
          </grid-vue>
        </div>
      </div>
    </main>
    <footer-vue cor="black" logo="Quality" descricao="Troca de moedas" ano="2022" >

    </footer-vue>
  </span>
</template>
<script>
import NavBarVue  from '@/components/layouts/NavBarVue'
import FooterVue from '@/components/layouts/FooterVue'
import GridVue from '@/components/layouts/GridVue'
import CardMenuVue from '@/components/layouts/CardMenuVue'
export default {
  name: 'SiteTemplate',
  data(){
    return{
      user: false
    }
  },
  components: {
    NavBarVue,
    FooterVue,
    GridVue,
    CardMenuVue
  },
  created(){
    console.log('created()');
    let usuarioAux = this.$store.getters.getUsuario;
    if(usuarioAux){
      this.user = this.$store.getters.getUsuario;
      this.$router.push('/')
    }
  }
}
</script>
<style>
</style>
