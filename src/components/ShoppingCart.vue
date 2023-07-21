<template>
  <div class="shopping-cart">
    <nav class="navbar">
      <div class="navbar-brand">
        <router-link to="/" class="home-button">Acasa</router-link>
      </div>
      <div class="navbar-menu">
        <router-link to="/asdf" class="navbar-item">Tricouri</router-link>
        <router-link to="/asdf" class="navbar-item">Pantaloni</router-link>
        <router-link to="/asdf" class="navbar-item">Camasi</router-link>
        <router-link to="/asdf" class="navbar-item">Hanorace</router-link>
        <router-link to="/asdf" class="navbar-item"
          >Jachete & Paltoane</router-link
        >
        <router-link to="/coscumparaturi" class="navbar-item cart-button">
          <span class="cart-icon"><i class="fas fa-shopping-cart"></i></span>
          <span class="cart-count">{{ storedData.length }}</span>
        </router-link>
      </div>
      <div class="navbar-buttons">
        <router-link
          v-if="isLoggedIn"
          to="/register"
          class="navbar-item register-button"
        >
          <span class="register-icon">
            <i class="fas fa-user-plus"></i>
          </span>
          Inregistreaza-te
        </router-link>
        <router-link
          v-if="isLoggedIn"
          to="/login"
          class="navbar-item login-button"
        >
          <span class="login-icon">
            <i class="fas fa-sign-in-alt"></i>
          </span>
          Logheaza-te
        </router-link>

        <button
          v-if="!isLoggedIn"
          class="navbar-item logout-button"
          @click="logout"
        >
          <span class="logout-icon">
            <i class="fas fa-sign-out-alt"></i>
          </span>
          Delogheaza-te
        </button>
      </div>
    </nav>
    <div class="container">
      <div class="row justify-content-center">
        <div
          class="col-md-4"
          v-for="product in storedData"
          :key="product.Imagine"
        >
          <div class="card">
            <div class="card-body">
              <img
                :src="getImageUrl(product.Imagine)"
                alt="Product Image"
                class="product-image"
              />
              <h6 class="card-title">{{ product.Denumire }}</h6>
              <p class="card-text">{{ product.Descriere }}</p>
              <p class="card-text">Pret: {{ product.Pret }} lei</p>
              <button class="delete-button" @click="deleteProduct(product)">
                Sterge produsul din cos
              </button>
            </div>
          </div>
        </div>
      </div>
      <div class="clear-cart">
        <button class="clear-cart-button" @click="clearCart">
          Sterge toate produsele din cos
        </button>
        <router-link to="/forms" class="send-order-button"
          >Confirmare comanda</router-link
        >
        
      </div>
    </div>
    <div class="total-price">Pretul total: {{ totalPrice }} lei</div>
  </div>
</template>

<script>
const token = sessionStorage.getItem("token");
export default {
  data() {
    return {
      data: [],
    };
  },

  headers: {
    Authorization: `Bearer ${token}`,
  },
  methods: {
    async logout() {
      try {
        const token = sessionStorage.getItem("token");

        if (!token) {
          console.error("User not logged in");
          return;
        }

        const response = await axios.post(
          "http://fashionseekersbackend.test/logout",
          {},
          {
            headers: {
              Authorization: `Bearer ${token}`,
            },
          }
        );

        if (response.status === 200) {
          console.log("Logout successful");
          sessionStorage.removeItem("token");
          this.$router.push({ path: "/login" });
        } else {
          console.error("Logout failed");
        }
      } catch (error) {
        console.error(error);
      }
    },
    isLoggedIn() {
      const token = sessionStorage.getItem("token");
      if (!token) return false;
      else return true;
    },
    getImageUrl(image) {
      const baseUrl = "http://fashionseekersbackend.test/img/";
      return `${baseUrl}${image}`;
    },
    saveData(product) {
      this.data.push({
        Denumire: product.denumire,
        Descriere: product.descriere,
        Pret: product.pret,
        Imagine: product.imagine,
      });
      localStorage.setItem("data", JSON.stringify(this.data));
    },
    deleteProduct(product) {
      const productIndex = this.storedData.findIndex(
        (p) => p.Imagine === product.Imagine
      );
      if (productIndex !== -1) {
        this.storedData.splice(productIndex, 1);
        localStorage.setItem("data", JSON.stringify(this.storedData));
      }
    },
    clearCart() {
      this.data = [];
      localStorage.removeItem("data");
    },
  },
  computed: {
    storedData() {
      const storedData = localStorage.getItem("data");
      if (storedData) {
        return JSON.parse(storedData);
      }
      return [];
    },
    totalPrice() {
      return this.storedData.reduce(
        (total, product) => total + product.Pret,
        0
      );
    },
  },
};
</script>

<style>
</style>