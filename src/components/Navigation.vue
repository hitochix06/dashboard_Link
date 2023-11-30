<template>
  <header class="bg-[#143457] navigation-header">
    <nav
      class="mx-auto flex max-w-7xl items-center justify-between lg:px-8"
      aria-label="Global"
    >
      <div class="flex lg:flex-1">
        <RouterLink to="/" class="-m-1.5 p-1.5">
          <img
            alt="ImmoZero logo"
            class="logo"
            src="@/assets/logo.png"
            height="70"
            width="70"
          />
        </RouterLink>
      </div>

      <!-- Mobile reponsiv -->
      <div class="flex lg:hidden">
        <button
          type="button"
          class="- inline-flex items-center justify-center rounded-md p-7 text-white"
          @click="mobileMenuOpen = true"
        >
          <span class="sr-only">Open main menu</span>
          <Bars3Icon class="h-6 w-6" aria-hidden="true" />
        </button>
      </div>

      <!-- different navigation -->
      <div class="hidden lg:flex lg:gap-x-12 texte_majuscule text-white">
        <RouterLink
          to="/"
          class="text-sm font-semibold leading-6 hover:underline"
          >Accueil</RouterLink
        >
        <!-- <RouterLink
          to="/ventes"
          class="text-sm font-semibold leading-6 hover:underline"
          >Ventes</RouterLink
        > -->
        <!-- <RouterLink
          to="/apropos"
          class="text-sm font-semibold leading-6 hover:underline"
          >A propose</RouterLink
        > -->
        <!-- <RouterLink
          to="/contact"
          class="text-sm font-semibold leading-6 hover:underline"
          >Contact</RouterLink
        > -->
      </div>

      <!-- personnaliser icones inscription/connexion -->

      <div class="hidden lg:flex lg:flex-1 lg:justify-end text-white">
        <RouterLink
          v-if="!isUserLoggedIn"
          to="connexion"
          class="text-sm font-semibold leading-6 flex items-center hover:underline"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            viewBox="0 0 24 24"
            fill="currentColor"
            class="w-6 h-6 mr-2"
            width="40"
            height="40"
          >
            <path
              fillRule="evenodd"
              d="M18.685 19.097A9.723 9.723 0 0021.75 12c0-5.385-4.365-9.75-9.75-9.75S2.25 6.615 2.25 12a9.723 9.723 0 003.065 7.097A9.716 9.716 0 0012 21.75a9.716 9.716 0 006.685-2.653zm-12.54-1.285A7.486 7.486 0 0112 15a7.486 7.486 0 015.855 2.812A8.224 8.224 0 0112 20.25a8.224 8.224 0 01-5.855-2.438zM15.75 9a3.75 3.75 0 11-7.5 0 3.75 3.75 0 017.5 0z"
              clipRule="evenodd"
            />
          </svg>
          Connexion/Inscription
        </RouterLink>
        <div
          v-else
          class="text-sm font-semibold leading-6 flex flex-col items-center"
        >
          <button
            type="button"
            class="text-sm font-semibold leading-6 flex items-center hover:underline"
            @click="logout"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              viewBox="0 0 24 24"
              fill="currentColor"
              class="w-6 h-6"
            >
              <path
                fill-rule="evenodd"
                d="M7.5 3.75A1.5 1.5 0 006 5.25v13.5a1.5 1.5 0 001.5 1.5h6a1.5 1.5 0 001.5-1.5V15a.75.75 0 011.5 0v3.75a3 3 0 01-3 3h-6a3 3 0 01-3-3V5.25a3 3 0 013-3h6a3 3 0 013 3V9A.75.75 0 0115 9V5.25a1.5 1.5 0 00-1.5-1.5h-6zm5.03 4.72a.75.75 0 010 1.06l-1.72 1.72h10.94a.75.75 0 010 1.5H10.81l1.72 1.72a.75.75 0 11-1.06 1.06l-3-3a.75.75 0 010-1.06l3-3a.75.75 0 011.06 0z"
                clip-rule="evenodd"
              />
            </svg>
            Déconnexion
          </button>
          <RouterLink to="/user" class="text-sm font-semibold leading-6">
            <span>{{ username }}</span>
          </RouterLink>
        </div>
      </div>
    </nav>

    <Dialog
      as="div"
      class="lg:hidden"
      @close="mobileMenuOpen = false"
      :open="mobileMenuOpen"
    >
      <div class="fixed inset-0 z-10" />
      <DialogPanel
        class="fixed inset-y-0 right-0 z-10 w-full overflow-y-auto bg-white px-6 py-6 sm:max-w-sm sm:ring-1 sm:ring-gray-900/10"
      >
        <!-- mobile menu  -->
        <div class="flex items-center justify-between">
          <a href="#" class="-m-1.5 p-1.5">
            <img
              class="h-20 w-auto"
              src="@/assets/logo.svg"
              alt="logo ImmoZero"
            />
          </a>
          <button
            type="button"
            class="-m-2.5 rounded-md p-2.5 text-gray-700"
            @click="mobileMenuOpen = false"
          >
            <span class="sr-only">Close menu</span>
            <XMarkIcon class="h-6 w-6" aria-hidden="true" />
          </button>
        </div>
        <div class="mt-6 flow-root">
          <div class="-my-6 divide-y divide-gray-500/10">
            <div class="space-y-2 py-6">
              <a
                href="/"
                class="-mx-3 block rounded-lg px-3 py-2 text-base font-semibold leading-7 hover:bg-gray-50"
                >Accueil</a
              >
              <a
                href="#"
                class="-mx-3 block rounded-lg px-3 py-2 text-base font-semibold leading-7 hover:bg-gray-50"
                >Ventes</a
              >
              <a
                href="#"
                class="-mx-3 block rounded-lg px-3 py-2 text-base font-semibold leading-7 hover:bg-gray-50"
                >A propos</a
              >
              <a
                href="#"
                class="-mx-3 block rounded-lg px-3 py-2 text-base font-semibold leading-7 hover:bg-gray-50"
                >Contact</a
              >
            </div>
            <RouterLink
              v-if="!isUserLoggedIn"
              to="connexion"
              class="text-sm font-semibold leading-6 flex items-center hover:underline pt-6"
            >
              <svg
                xmlns="http://www.w3.org/2000/svg"
                viewBox="0 0 24 24"
                fill="currentColor"
                class="w-6 h-6 mr-2"
                width="40"
                height="40"
              >
                <path
                  fillRule="evenodd"
                  d="M18.685 19.097A9.723 9.723 0 0021.75 12c0-5.385-4.365-9.75-9.75-9.75S2.25 6.615 2.25 12a9.723 9.723 0 003.065 7.097A9.716 9.716 0 0012 21.75a9.716 9.716 0 006.685-2.653zm-12.54-1.285A7.486 7.486 0 0112 15a7.486 7.486 0 015.855 2.812A8.224 8.224 0 0112 20.25a8.224 8.224 0 01-5.855-2.438zM15.75 9a3.75 3.75 0 11-7.5 0 3.75 3.75 0 017.5 0z"
                  clipRule="evenodd"
                />
              </svg>
              Connexion/Inscription
            </RouterLink>
            <div
              v-else
              class="text-sm font-semibold leading-6 flex flex-col items-center pt-6"
            >
              <button
                type="button"
                class="text-sm font-semibold leading-6 flex items-center hover:underline"
                @click="logout"
              >
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  viewBox="0 0 24 24"
                  fill="currentColor"
                  class="w-6 h-6"
                >
                  <path
                    fill-rule="evenodd"
                    d="M7.5 3.75A1.5 1.5 0 006 5.25v13.5a1.5 1.5 0 001.5 1.5h6a1.5 1.5 0 001.5-1.5V15a.75.75 0 011.5 0v3.75a3 3 0 01-3 3h-6a3 3 0 01-3-3V5.25a3 3 0 013-3h6a3 3 0 013 3V9A.75.75 0 0115 9V5.25a1.5 1.5 0 00-1.5-1.5h-6zm5.03 4.72a.75.75 0 010 1.06l-1.72 1.72h10.94a.75.75 0 010 1.5H10.81l1.72 1.72a.75.75 0 11-1.06 1.06l-3-3a.75.75 0 010-1.06l3-3a.75.75 0 011.06 0z"
                    clip-rule="evenodd"
                  />
                </svg>
                Déconnexion
              </button>
              <RouterLink to="/user" class="text-sm font-semibold leading-6">
                <span>{{ username }}</span>
              </RouterLink>
            </div>
          </div>
        </div>
      </DialogPanel>
    </Dialog>
  </header>
</template>

<style scoped>
.texte_majuscule {
  text-transform: uppercase;
}
.couleur_fond {
  background: #43b7be;
  padding: 10px;
  color: white;
  border-radius: 10px;
}
</style>

<script setup>
import { ref } from "vue";
import { Dialog, DialogPanel } from "@headlessui/vue";
import { Bars3Icon, XMarkIcon } from "@heroicons/vue/24/outline";
import { useRouter } from "vue-router";

// Router
const router = useRouter();
// username dans localStorage
const username = ref(localStorage.getItem("username"));

const mobileMenuOpen = ref(false);
const isUserLoggedIn = ref(localStorage.getItem("token") !== null); // Change this to true if a token is found in localStorage

// ajouter une fonction logout
function logout() {
  // Remove the user's token from localStorage
  localStorage.removeItem("token");
  alert("Vous êtes déconnecté");
  router.push("/");

  // Set isUserLoggedIn to false
  isUserLoggedIn.value = false;
}
</script>
