<template>
  <v-container>
    <v-card>
      <v-toolbar card dark color="secondary">
        <v-toolbar-title>Log In</v-toolbar-title>
      </v-toolbar>
      <v-card-text>
        <v-form>
          <v-expand-transition>
            <v-alert :value="error" type="error">
              {{ error }}
            </v-alert>
          </v-expand-transition>
          <v-text-field
            prepend-icon="person"
            color="accent"
            label="Email"
            type="email"
            hint="Use your commschool.org email"
            v-model="email"
          ></v-text-field>
          <v-text-field
            prepend-icon="lock"
            color="accent"
            label="Password"
            type="password"
            v-model="password"
          ></v-text-field>
        </v-form>
      </v-card-text>
      <v-card-actions>
        <v-spacer />
        <v-btn color="accent" flat @click="forgot">Reset Password</v-btn>
        <v-btn color="accent" outline @click="register">Register</v-btn>
        <v-btn color="accent" @click="login">Log In</v-btn>
      </v-card-actions>
    </v-card>
  </v-container>
</template>
<script>
import router from "../router";
import { auth } from "../firebaseConfig";

export default {
  data: () => {
    return {
      email: "",
      password: "",
      error: ""
    };
  },
  name: "Login",
  methods: {
    login() {
      auth
        .signInWithEmailAndPassword(this.email, this.password)
        .catch(error => {
          this.error = error.message;
        })
        .then(value => {
          if (value) {
            router.push("/dashboard");
          }
        });
    },
    register() {
      auth.createUserWithEmailAndPassword(this.email, this.password).then(
        () => {
          router.push("/register");
        },
        error => {
          this.error = error.message;
        }
      );
    },
    forgot() {
      auth.sendPasswordResetEmail(this.email).catch(error => {
        this.error = error.message;
      });
    }
  }
};
</script>
