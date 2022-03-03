<template>
<v-form @submit.prevent="userLogin">
    <v-container>

      <v-row>
        <v-col>
          <v-text-field v-model="login.email" label="E-Mail" filled outlined></v-text-field>
        </v-col>
      </v-row>

      <v-row>
        <v-col>
          <!-- <v-text-field v-model="login.password" label="Password" filled outlined></v-text-field> -->
          <v-text-field
            v-model="login.password"
            filled
            outlined
            :append-icon="show1 ? 'mdi-eye' : 'mdi-eye-off'"
            :rules="[rules.required, rules.min]"
            :type="show1 ? 'text' : 'password'"
            name="input-10-1"
            label="Password"
            hint="At least 8 characters"
            counter
            @click:append="show1 = !show1"
          ></v-text-field>
        </v-col>
      </v-row>

      <v-row>
        <v-col >
          <v-btn color="primary" width="100%" type="submit">Login</v-btn>
        </v-col>
      </v-row>
    </v-container>
  </v-form>
</template>

<script>
export default {
  name: 'LoginPage',
  data() {
    return {
      login: {
        email: '',
        password: ''
      },
      // snackbar
      snackbar: false,
      text: "Hello, I'm a snackbar",
      timeout: 2000,
      multiLine: true,

      // password field
      show1: false,
      rules: {
          required: value => !!value || 'Required.',
          min: v => v.length >= 8 || 'Min 8 characters',
          emailMatch: () => (`The email and password you entered don't match`),
        },
    }
  },
  methods: {
    async userLogin() {
      await this.$auth.loginWith('local', {
        data: this.login
      }).then(res => {

        let user = res.data.user // getting user (yours can be different)
        this.$auth.$storage.setUniversal('user', user, true) // setting user in Vuex, cookies and localstorage

        user = this.$auth.$storage.getUniversal('user') // getting user (you can use it anywhere in your app)
        /* eslint-disable no-console */

        this.$auth.strategy.token.set(res.data.tokens.access.token)
        this.$auth.strategy.refreshToken.set(res.data.tokens.refresh.token)

        this.$root.$emit('showSnackbar', 'You are logged in!');

        this.$router.push('/') // redirecting after login
      }).catch(err => {
        this.$root.$emit('showSnackbar', 'Login failed!');
        console.log(err.response)
      })
    }
  }
}
</script>
