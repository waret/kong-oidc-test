<template>
  <div id="app">
    <div id="nav">
      <a href="http://charlie.waret.net">Home</a> |
      <a href="http://charlie.waret.net/protected">Charlie</a> |
      <a href="http://delta.waret.net/protected">Delta</a> |
      <button @click="triggerAuthenticateOidc" v-if="!oidcIsAuthenticated">Login</button>
      <button @click="signOutOidc" v-if="oidcIsAuthenticated">Logout</button>
      <router-view/>
    </div>
  </div>
</template>

<script>
import { mapGetters, mapActions } from 'vuex'

export default {
  name: 'App',
  computed: {
    ...mapGetters('oidcStore', [
      'oidcIsAuthenticated'
    ]),
    hasAccess: function () {
      // return this.oidcIsAuthenticated || this.$route.meta.isPublic
      return this.oidcIsAuthenticated
    }
  },
  methods: {
    ...mapActions('oidcStore', ['authenticateOidc', 'signOutOidc']),
    userLoaded: function () {
      console.log('I am listening to the user loaded event in vuex-oidc')
    },
    triggerAuthenticateOidc: function () {
      this.authenticateOidc('/')
    }
  },
  mounted () {
    window.addEventListener('vuexoidc:userLoaded', this.userLoaded)
  },
  destroyed () {
    window.removeEventListener('vuexoidc:userLoaded', this.userLoaded)
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
</style>
