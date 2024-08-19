<template>
  <Menu>
    <router-link to="/">Home</router-link>
    <router-link to="/about">About</router-link>
    <router-link to="/pacientes" v-if="canSee(['admin'])">Pacientes</router-link>
    <router-link to="/medicos" v-if="canSee(['admin'])">Medicos</router-link>
    <router-link to="/citas" v-if="canSee(['admin'])">Citas</router-link>
  </Menu>
  <router-view/>
</template>

<script>
import {mapGetters } from 'vuex'
import Menu from './components/Menu.vue'

export default {
  name: 'App',
  data() {
    return {

    }
  },
  components: {
    Menu
  },
  methods: {
    canSee(roles) {
      console.log('canSee', this.rol);
      return roles.includes(this.rol);
    }
  },
  computed: {
    ...mapGetters(['rol'])
  }
}
</script>

<style lang="scss"></style>
