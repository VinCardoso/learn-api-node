<template>
  <v-container>
    <v-layout row wrap>

      <v-flex xs12>
        <h2>Login</h2>
      </v-flex>
      <v-spacer></v-spacer>
      <v-flex xs12 sm6 offset-sm3>
        <v-text-field
        label="E-mail"
        v-model="email"
        v-bind:rules="emailRules"
        required>
        </v-text-field>

        <v-text-field
        label="Senha"
        v-model="password"
        v-bind:rules="passwordRules"
        v-bind:type="'password'"
        requered
        >
        </v-text-field>
      </v-flex>

      <v-flex xs12 sm6 offset-sm3>
        <v-btn v-on:click="cancel">Cancel</v-btn>
        <v-btn color="primary" v-on:click="login">Login</v-btn>
      </v-flex>

      <v-snackbar
        :timeout="6000"
        :top="true"
        v-model="showAlert"
      >
      {{ loginError }}
      </v-snackbar>
    </v-layout>
  </v-container>
</template>

<script>
export default {
  data () {
    return {
      showAlert: false,
      messege: '',
      email: '',
      emailRules: [
        v => !!v || 'E-mail é obrigatório',
        v => /^\w+([.-]?\w+)*@\w+([.-]?\w+)*(\.\w{2,3})+$/.test(v) || 'Este não parece um e-mail válido'
      ],
      password: '',
      passwordRules: [v => !!v || 'Você precisa digitar sua senha']
    }
  },
  computed: {
    isLoggedIn () {
      return this.$store.getters.isLoggedIn
    },
    loginError () {
      return this.$store.getters.loginError
    }
  },
  methods: {
    login: function () {
      const vm = this
      const payload = {
        email: this.email,
        password: this.password
      }
      this.$store.dispatch('logInUser', payload).then(() => {
        if (vm.isLoggedIn) {
          this.$router.push({ path: '/' })
        } else {
          vm.showAlert = true
        }
      })
    },
    cancel: function () {
      console.log('O usuário não quer se conectar')
    }
  }

}
</script>

<style>

</style>
