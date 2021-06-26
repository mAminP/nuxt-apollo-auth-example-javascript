<template>
  <v-row justify="center">
    <v-col cols="6">
      <v-card>
        <v-card-title>
          <h2>Login</h2>
        </v-card-title>
        <v-divider />
        <v-form ref="form" v-model="valid" lazy-validation>
          <v-card-text>
            <v-text-field
              v-model="email"
              :rules="emailRules"
              label="E-mail"
              required
            ></v-text-field>
            <v-text-field
              v-model="password"
              :rules="passwordRules"
              label="password"
              type="password"
              required
            ></v-text-field>
          </v-card-text>
          <v-card-actions>
            <v-btn :disabled="!valid" @click.stop="submitForm" color="success">
              Login
            </v-btn>
          </v-card-actions>
        </v-form>
      </v-card>
    </v-col>
    <v-col cols="12">
      <v-card >
          <v-card-subtitle>available users:</v-card-subtitle>
          <v-card-text>

        <v-row justify="center" dense>
          <v-col
            cols="12"
            md="6"
            lg="4"
            v-for="account in accounts"
            :key="account.id"
          >
            <v-card rounded="lg" color="grey darken-4">
              <v-list-item
                dense
                v-for="(value, key, index) in account"
                :key="index"
              >
                <small class="grey--text mr-3">{{ key }}: </small>
                <span> {{ value }}</span>
              </v-list-item>
            </v-card>
          </v-col>
        </v-row>
          </v-card-text>

      </v-card>
    </v-col>
  </v-row>
</template>

<script>
import gql from "graphql-tag";
export default {
  apollo: {
    accounts: {
      query: gql`
        query accounts {
          accounts {
            id
            name
            email
            password
          }
        }
      `,
    },
  },
  data: () => ({
    valid: true,
    email: "",
    emailRules: [
      (v) => !!v || "E-mail is required",
      (v) => /.+@.+\..+/.test(v) || "E-mail must be valid",
    ],
    password: "",
    passwordRules: [(v) => !!v || "password is required"],
  }),
  methods: {
    async submitForm() {
      if (this.$refs.form.validate()) {
        try {
          const res = await this.$qAuth.login({
            data: {
              email: this.email,
              password: this.password,
            },
          });
          console.log("res :>> ", res);
          if (res.success) {
              // this.$router.push()
          }
        } catch (error) {
          console.log("error :>> ", error);
        }
      }
    },
  },
};
</script>

<style>
</style>