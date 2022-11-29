<template>
<v-form @submit.prevent="registerNewUser">
    <v-container>

      <v-row>
        <v-col>
          <v-text-field v-model="registration.username" label="Username" filled outlined></v-text-field>
        </v-col>
      </v-row>

      <v-row>
        <v-col>
          <v-text-field v-model="registration.email" label="Email" filled outlined></v-text-field>
        </v-col>
      </v-row>

      <v-row>
        <v-col>
          <v-text-field
            v-model="registration.password"
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
        <v-col>
          <v-text-field
            v-model="registration.confirm_password"
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
          <v-btn color="primary" width="100%" type="submit">Register</v-btn>
        </v-col>
      </v-row>
    </v-container>
  </v-form>
</template>

<script>
export default {
  name: 'RegistrationPage',
  data() {
    return {
      registration: {
        username: '',
        email: '',
        password: '',
        confirm_password: '',
      },
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
    async registerNewUser() {
      await this.$axios.$post('http://localhost:3000/v1/users/', this.registration)
      .then((createdUser) => {
            console.log(createdUser);
            this.createdUser = createdUser;
            this.$root.$emit('showSnackbar', 'User was created');
        })
      .catch((err) => {
          // If any part of the chain is rejected, print the error message.
          console.log(err);
          this.$root.$emit('showSnackbar', 'Oops! User could not be created!');
        });

    }
}
}
</script>
