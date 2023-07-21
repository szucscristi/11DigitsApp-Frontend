<template>
  <div class="login-form">
    <h2>Completati campurile de mai jos:</h2>
    <div>
      <div class="form-group">
        <label for="email">Email</label>
        <input type="email" id="email" v-model="email" required />
      </div>
      <div class="form-group">
        <label for="password">Parola</label>
        <input type="password" id="password" v-model="password" required />
      </div>
      <button type="submit" @click="login">Logheaza-te</button>
    </div>
  </div>
  <router-link to="/asdf" class="back-to-homepage"
    >Inapoi la pagina principala</router-link
  >
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      email: "",
      password: "",
      message: "",
    };
  },
  methods: {
    async login() {
      const loginData = {
        email: this.email,
        password: this.password,
      };

      try {
        const response = await axios.post(
          "http://fashionseekersbackend.test/login",
          { login: loginData }
        );

        if (response.data.token) {
          this.message = "Bun venit " + response.data.name + "!";
          sessionStorage.setItem("token", response.data.token);
          console.log(response.data.token);

          this.$router.push({ path: "/asdf" });
        } else {
          this.message = "Email sau parola incorecta!";
        }
      } catch (error) {
        console.error(error);
        this.message = "Email sau parola incorecta!";
      }
    },
  },
};
</script>