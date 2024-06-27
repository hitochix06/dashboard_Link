<template>
  <div class="bg-[#E8EEF5] py-2 sm:py-5" style="height: 100vh">
    <div class="mx-auto max-w-7xl px-1 lg:px-8">
      <div
        class="mx-auto mt-16 max-w-full p-5 sm:mt-20 lg:mx-0 flex flex-col lg:flex-row"
      >
        <div
          class="-mt-2 p-5 lg:mt-0 lg:w-auto lg:max-w-full lg:flex-shrink-0 h-auto flex-grow lg:flex-grow rounded-l-lg"
          style="background-color: #ffffff"
        >
          <div
            class="text-center ring-gray-900/5 lg:flex lg:flex-col lg:justify-center lg:py-16"
          >
            <!-- formulaire connexion -->
            <form @submit.prevent="handleConnexion">
              <div class="mx-auto max-w-xs">
                <h1
                  class="text-2xl font-bold tracking-tight text-gray-900 sm:text-3xl"
                  style="color: rgb(0, 0, 0); text-transform: uppercase"
                >
                  Se connecter
                </h1>

                <div class="mt-6 flex items-baseline justify-center gap-x-2">
                  <label for="email-address"></label>
                  <input
                    v-model="email_connexion"
                    id="email_address"
                    name="email"
                    type="email"
                    autocomplete="email"
                    class="min-w-0 flex-auto rounded-md border-2 px-3.5 py-2 text-gray-900 shadow-sm ring-1 ring-inset ring-white/10 focus:ring-2 focus:ring-inset focus:ring-indigo-500 sm:text-sm sm:leading-6 m-2"
                    placeholder="E-mai"
                    required
                  />
                </div>
                <div class="mt-6 flex items-baseline justify-center gap-x-2">
                  <label for="password"></label>
                  <input
                    v-model="password_connexion"
                    id="password"
                    name="password"
                    type="password"
                    autocomplete="current-password"
                    class="min-w-0 flex-auto rounded-md border-2 px-3.5 py-2 text-gray-900 shadow-sm ring-1 ring-inset ring-white/10 focus:ring-2 focus:ring-inset focus:ring-indigo-500 sm:text-sm sm:leading-6 m-2"
                    placeholder="Mot de passe"
                  />
                </div>
                <div class="flex justify-end">
                  <a href="#" class="text-sm m-2" style="color: red">
                    Mots de passe oublié
                  </a>
                </div>
                <button
                  type="submit"
                  class="mt-10 block w-full rounded-md bg-[#43B7BE] px-3 py-2 text-center text-sm font-semibold text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600"
                >
                  <div style="text-transform: uppercase">connexion !</div>
                </button>
              </div>
            </form>
          </div>
        </div>

        <!-- formulaire d'inscription -->
        <div
          class="-mt-2 p-2 lg:mt-0 lg:w-auto lg:max-w-full lg:flex-shrink-0 h-auto flex-grow lg:flex-grow rounded-r-lg"
          style="background-color: #143457"
        >
          <div
            class="text-center ring-gray-900/5 lg:flex lg:flex-col lg:justify-center lg:py-16"
          >
            <form>
              <div class="mx-auto max-w-xs">
                <h1
                  class="text-2xl font-bold tracking-tight text-gray-900 sm:text-3xl"
                  style="color: white"
                >
                  CRÉÉZ VOTRE COMPTE
                </h1>

                <p
                  class="mt-6 flex items-baseline justify-center gap-x-2"
                  style="color: white"
                >Venez, créé votre carte raccourcie
              </p>
                <div class="mt-6 flex items-baseline justify-center gap-x-2">
                  <label for="email-address"></label>
                  <input
                    v-model="emails"
                    id="email_address"
                    name="email"
                    type="email"
                    autocomplete="emails"
                    class="min-w-0 flex-auto rounded-md border-0 px-3.5 py-2 text-gray-900 shadow-sm ring-1 ring-inset ring-white/10 focus:ring-2 focus:ring-inset focus:ring-indigo-500 sm:text-sm sm:leading-6 m-2"
                    placeholder="Adress e-mail"
                    required
                  />
                </div>
                <button
                  @click="isOpen = true"
                  type="button"
                  class="mt-10 block w-full rounded-md bg-[#43B7BE] px-3 py-2 text-center text-sm font-semibold text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600"
                >
                  <div style="text-transform: uppercase">C'est Parti !</div>
                </button>
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- création du formulaire inscription modal -->
  <div class="container mx-auto">
    <div class="flex justify-center">
      <form
        v-show="isOpen"
        class="absolute inset-0 flex items-center justify-center bg-gray-700 bg-opacity-50"
        @submit.prevent="handleInscription"
      >
        <div
          class="w-full md:w-2/3 lg:w-3/5 p-6 bg-white rounded-md shadow-xl m-2"
        >
          <div class="flex items-center justify-between">
            <h3
              class="text-2xl"
              style="text-transform: uppercase; font-weight: 700"
            >
              Formulaire d'inscription
            </h3>
            <svg
              @click="isOpen = false"
              xmlns="http://www.w3.org/2000/svg"
              viewBox="0 0 24 24"
              fill="currentColor"
              className="w-6 h-6 "
              width="30"
              height="30"
              style="cursor: pointer"
            >
              <path
                fillRule="evenodd"
                d="M5.47 5.47a.75.75 0 011.06 0L12 10.94l5.47-5.47a.75.75 0 111.06 1.06L13.06 12l5.47 5.47a.75.75 0 11-1.06 1.06L12 13.06l-5.47 5.47a.75.75 0 01-1.06-1.06L10.94 12 5.47 6.53a.75.75 0 010-1.06z"
                clipRule="evenodd"
              />
            </svg>
          </div>
          <div class="mt-4 uppercase">
            <div class="mt-10 grid grid-cols-1 gap-x-6 gap-y-8 sm:grid-cols-6">
              <div class="sm:col-span-3">
                <div class="mt-2">
                  <label
                    for="first-name"
                    class="block text-sm font-medium leading-6 text-gray-900"
                    >non</label
                  >
                  <input
                    v-model="username"
                    type="text"
                    name="username"
                    autocomplete="given-name"
                    class="min-w-full flex-auto rounded-md border-2 px-3.5 py-2 text-gray-900 shadow-sm ring-1 ring-inset ring-white/10 focus:ring-2 focus:ring-inset focus:ring-indigo-500 sm:text-sm sm:leading-6 m-2"
                    placeholder="Nom"
                    required
                  />
                </div>
              </div>

              <div class="sm:col-span-3">
                <div class="mt-2">
                  <label for="emails">Email</label>
                  <input
                    v-model="emails"
                    type="emails"
                    name="emails"
                    autocomplete="off"
                    class="min-w-full flex-auto rounded-md border-2 px-3.5 py-2 text-gray-900 shadow-sm ring-1 ring-inset ring-white/10 focus:ring-2 focus:ring-inset focus:ring-indigo-500 sm:text-sm sm:leading-6 m-2"
                    placeholder="Email"
                    required
                  />
                </div>
              </div>

              <div class="sm:col-span-3 py-2">
                <div class="mt-2">
                  <label
                    for="password"
                    class="block text-sm font-medium leading-6 text-gray-900"
                    >Mot de passe</label
                  >
                  <input
                    v-model="password_inscri"
                    type="password"
                    name="password"
                    autocomplete="off"
                    class="min-w-full flex-auto rounded-md border-2 px-3.5 py-2 text-gray-900 shadow-sm ring-1 ring-inset ring-white/10 focus:ring-2 focus:ring-inset focus:ring-indigo-500 sm:text-sm sm:leading-6 m-2"
                    placeholder="Mot de passe"
                    required
                  />
                </div>
              </div>
            </div>

            <button
              @click="isOpen = false"
              class="px-6 py-2 text-blue-800 border border-blue-600 rounded"
            >
              Annuler
            </button>
            <button
              type="submit"
              class="ml-2 text-[#fff] bg-[#43B7BE] rounded hover:bg-[#3b5998]/90 focus:ring-4 focus:outline-none focus:ring-[#3b5998]/50 text-sm px-5 py-2.5 text-center inline-flex items-center dark:focus:ring-[#3b5998]/55"
            >
              Ajouter
            </button>
          </div>
        </div>
      </form>
    </div>
  </div>

  <!-- cree une alerte -->
  <transition name="slide-fade">
    <div
      v-show="showAlert"
      :class="{ 'alert-hide': !showAlert }"
      id="alert-border-3"
      class="fixed top-0 right-0 flex items-center p-5 mb-4 text-green-800 border-t-4 border-green-300 bg-green-50 dark:text-green-400 dark:bg-gray-800 dark:border-green-800"
      role="alert"
    >
      <svg
        class="flex-shrink-0 w-4 h-4"
        aria-hidden="true"
        xmlns="http://www.w3.org/2000/svg"
        fill="currentColor"
        viewBox="0 0 20 20"
      >
        <path
          d="M10 .5a9.5 9.5 0 1 0 9.5 9.5A9.51 9.51 0 0 0 10 .5ZM9.5 4a1.5 1.5 0 1 1 0 3 1.5 1.5 0 0 1 0-3ZM12 15H8a1 1 0 0 1 0-2h1v-3H8a1 1 0 0 1 0-2h2a1 1 0 0 1 1 1v4h1a1 1 0 0 1 0 2Z"
        />
      </svg>
      <div class="ms-3 text-sm font-medium">
        Votre compte a été créé avec succès !
      </div>
    </div>
  </transition>

  <!-- cree une alerte  connexion-->
  <transition name="slide-fade">
    <div
      v-show="showAlertConnexion"
      :class="{ 'alert-hide': !showAlertConnexion }"
      id="alert-border-3"
      class="fixed top-0 right-0 flex items-center p-5 mb-4 text-green-800 border-t-4 border-green-300 bg-green-50 dark:text-green-400 dark:bg-gray-800 dark:border-green-800"
      role="alert"
    >
      <svg
        class="flex-shrink-0 w-4 h-4"
        aria-hidden="true"
        xmlns="http://www.w3.org/2000/svg"
        fill="currentColor"
        viewBox="0 0 20 20"
      >
        <path
          d="M10 .5a9.5 9.5 0 1 0 9.5 9.5A9.51 9.51 0 0 0 10 .5ZM9.5 4a1.5 1.5 0 1 1 0 3 1.5 1.5 0 0 1 0-3ZM12 15H8a1 1 0 0 1 0-2h1v-3H8a1 1 0 0 1 0-2h2a1 1 0 0 1 1 1v4h1a1 1 0 0 1 0 2Z"
        />
      </svg>
      <div class="ms-3 text-sm font-medium">tu es connecté !</div>
    </div>
  </transition>
</template>

<style scoped>
.lg\:py-16 {
  padding-top: 1rem;
  padding-bottom: 0rem;
}
.slide-fade-enter-active {
  transition: all 0.3s ease;
}
.slide-fade-leave-active {
  transition: all 0.8s cubic-bezier(1, 0.5, 0.8, 1);
}
.slide-fade-enter, .slide-fade-leave-to
/* .slide-fade-leave-active below version 2.1.8 */ {
  transform: translateX(10px);
  opacity: 0;
}
</style>

<script setup>
import { ref } from "vue";
import axios from "axios";
import { useRouter } from "vue-router";

const BASE_ID = import.meta.env.VITE_APP_BASS_ID;
const TABLE_NAME = "Formulaire_inscription";
const API_TOKEN = import.meta.env.VITE_APP_TOKEN;

// Data
const contacts = ref([]);
const nom = ref("");
const prenom = ref("");
const email = ref("");

// Methods
const handleResetForm = () => {
  nom.value = "";
  prenom.value = "";
  email.value = "";
};

// Router
const router = useRouter();

// Modal
const isOpen = ref(false);

// Alert message inscription
const showAlert = ref(false);

// Alert message connexion
const showAlertConnexion = ref(false);

// Connexion
const email_connexion = ref("");
const password_connexion = ref("");

// incrisption
const emails = ref("");
const password_inscri = ref("");
const username = ref("");

// function Incrisption
const handleInscription = async () => {
  try {
    const response = await axios.post(
      "https://apihackaton1.osc-fr1.scalingo.io/users/register",
      {
        username: username.value,
        email: emails.value,
        password: password_inscri.value,
      }
    );
    showAlert.value = true; // Affichez votre alerte
    isOpen.value = false;
    localStorage.setItem("username", username.value); // Stockez le nom d'utilisateur dans le localStorage après l'inscription
    username.value = "";
    emails.value = "";
    password_inscri.value = "";

    // Faire disparaître l'alerte après 5 secondes
    setTimeout(() => {
      showAlert.value = false;
    }, 3000);
    // alert(" Votre compte a été créé avec succès");
  } catch (error) {
    // Gérer l'erreur de réseau
    console.log(error);
  }
};

// function connexion
const handleConnexion = async () => {
  try {
    const response = await axios.post(
      "https://apihackaton1.osc-fr1.scalingo.io/users/login",
      {
        email: email_connexion.value,
        password: password_connexion.value,
      }
    );

    const token = response.data.token;
    localStorage.setItem("token", token);

    showAlertConnexion.value = true; // Affichez votre alerte
    // router.go();
    // router.push("/user");

    // Faire disparaître l'alerte après 5 secondes
    setTimeout(() => {
      showAlertConnexion.value = false;
    }, 3000);
  } catch (error) {
    alert("Adresse e-mail ou mot de passe incorrect");
    // Réinitialiser les champs d'input
    email_connexion.value = "";
    password_connexion.value = "";
  }
};
</script>
