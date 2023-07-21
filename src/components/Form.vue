<script setup>
import { ref } from "vue";
import { onMounted, computed } from "vue";
import axios from "axios";


const nume = ref("");
const prenume = ref("");
const email = ref("");
const telefon = ref("");
const adresa = ref("");

onMounted(() => {
  console.log("Component mounted.");
});

async function formSubmit() {
  try {
    await axios.post("http://fashionseekersbackend.test/forms", {
      Nume: nume.value,
      Prenume: prenume.value,
      Email: email.value,
      Telefon: telefon.value,
      Adresa: adresa.value,
    });
    
    sendEmail(storedData.value);
  } catch (error) {
    console.error(error);
  }
}

async function sendEmail(cartItems) {
  try {
    const response = await axios.post("http://fashionseekersbackend.test/send-mail", {
      cartItems,
    });
    console.log("Email sent successfully");
  } catch (error) {
    console.error("Error sending email:", error);
  }
}

const storedData = computed(() => {
  const storedData = localStorage.getItem("data");
  if (storedData) {
    return JSON.parse(storedData);
  }
  return [];
});
</script>

<template>
  <div class="container-fluid">
    <div class="row justify-content-center">
      <div class="col-md-8 mt-3">
        <div class="card">
          <div class="card-header">Formular de livrare</div>

          <div class="card-body">
            <div>
              <strong>Nume:</strong>
              <input type="text" class="form-control" v-model="nume" />
              <br />
              <strong>Prenume:</strong>
              <input type="text" class="form-control" v-model="prenume" />
              <br />
              <strong>Email:</strong>
              <input type="text" class="form-control" v-model="email" />
              <br />
              <strong>Numar telefon:</strong>
              <input type="text" class="form-control" v-model="telefon" />
              <br />
              <strong>Adresa livrare:</strong>
              <input type="text" class="form-control" v-model="adresa" />
              <br />

              <button class="btn btn-success" @click="formSubmit()">
                Plaseaza comanda
              </button>
              <router-link to="/coscumparaturi" class="back-to-shoppingcart"
                >Inapoi la cosul de cumparaturi</router-link
              >
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style>
</style>