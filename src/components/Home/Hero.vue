<template>
  <div class="py-3 px-1">
    <div class="max-w-md mx-auto rounded-lg overflow-hidden md:max-w-xl">
      <div class="md:flex">
        <div class="w-full p-10">
          <h1
            class="text-3xl font-semibold text-white dark:text-white uppercase text-center py-5"
          ></h1>

          <div
            class="relative z-0 shadow-xl bg-white overflow-hidden rounded-lg"
          >
            <input
              type="search"
              id="search"
              v-model="searchValue"
              @input="search"
              class="block p-2.5 w-full z-20 text-sm text-gray-900 bg-gray-50 rounded-lg border-s-gray-50 border-s-2 border border-gray-300 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-s-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:border-blue-500"
              placeholder="Recherchez dans vos annonces"
              required
            />
          </div>
          <div class="container mx-auto flex justify-center mt-10">
            <button
              @click="isOpen = true"
              type="button"
              class="text-white bg-[#43B7BE] hover:bg-[#3b5998]/90 focus:ring-4 focus:outline-none focus:ring-[#3b5998]/50 font-medium rounded-lg text-sm px-5 py-2.5 text-center inline-flex items-center dark:focus:ring-[#3b5998]/55 me-2 mb-2"
            >
              <svg
                xmlns="http://www.w3.org/2000/svg"
                viewBox="0 0 24 24"
                fill="currentColor"
                className="w-6 h-6"
                width="20"
                height="20"
              >
                <path
                  fillRule="evenodd"
                  d="M12 3.75a.75.75 0 01.75.75v6.75h6.75a.75.75 0 010 1.5h-6.75v6.75a.75.75 0 01-1.5 0v-6.75H4.5a.75.75 0 010-1.5h6.75V4.5a.75.75 0 01.75-.75z"
                  clipRule="evenodd"
                />
              </svg>
              <div class="uppercase">Ajouter un raccourci</div>
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- Creation de carte pour les annonces -->
  <div class="bg-white">
    <div
      class="mx-auto max-w-2xl px-4 py-16 sm:px-6 sm:py-24 lg:max-w-7xl lg:px-8"
    >
      <div class="mt-6 grid grid-cols-1 lg:grid-cols-2 gap-x-6 gap-y-10">
        <div
          v-for="item in data"
          :key="item.id"
          class="group relative shadow-xl bg-white overflow-hidden rounded-lg"
        >
          <!-- code gere image -->
          <div
            class="aspect-h-1 aspect-w-1 w-full overflow-hidden rounded-md bg-gray-200 lg:aspect-none lg:h-60"
          >
            <img
              src="https://cf.bstatic.com/xdata/images/hotel/max1024x768/295090917.jpg?k=d17621b71b0eaa0c7a37d8d8d02d33896cef75145f61e7d96d296d88375a7d39&o=&hp=1"
              class="h-full w-full object-cover object-center lg:h-full lg:w-full"
            />
          </div>
          <div class="mt-4 flex justify-between">
            <div>
              <h1 class="text-lg text-black-900 m-5">
                {{ item.title }}
              </h1>
              <p class="mt-1 text-sm text-black-900 m-5">
                {{ item.description }}
              </p>
              <p class="mt-1 text-sm m-5 uppercase text-gray-900">
                {{ item.location }}
              </p>
            </div>
            <p class="text-lg font-medium m-5 text-gray-900">
              {{ item.price }}€
            </p>
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- creation formulaire de modal  -->
  <div class="container mx-auto">
    <div class="flex justify-center">
      <form
        @submit.prevent="handleCreationRaccourci"
        v-show="isOpen"
        class="absolute inset-0 flex items-center justify-center bg-gray-700 bg-opacity-50"
      >
        <div
          class="w-full md:w-2/3 lg:w-3/5 p-6 bg-white rounded-md shadow-xl m-2"
        >
          <div class="flex items-center justify-between">
            <h3
              class="text-2xl"
              style="text-transform: uppercase; font-weight: 700"
            >
              Ajouter un raccourci
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
          <div class="mt-4">
            <div class="mt-10 grid grid-cols-1 gap-x-6 gap-y-8 sm:grid-cols-6">
              <div class="sm:col-span-5">
                <div class="mt-2">
                  <label
                    for="first-name"
                    class="block text-sm font-medium leading-6 text-gray-900"
                    >Nom</label
                  >
                  <input
                    v-model="title"
                    type="text"
                    name="first-name"
                    id="first-name"
                    autocomplete="given-name"
                    class="min-w-full flex-auto rounded-md border-2 px-3.5 py-2 text-gray-900 shadow-sm ring-1 ring-inset ring-white/10 focus:ring-2 focus:ring-inset focus:ring-indigo-500 sm:text-sm sm:leading-6 m-2"
                    placeholder="Nom"
                    required
                  />
                </div>
              </div>

              <div class="sm:col-span-5">
                <div class="mt-2">
                  <label for="Url">URL</label>
                  <input
                    v-model="price"
                    type="Url"
                    name="Url"
                    id="Url"
                    autocomplete="off"
                    class="min-w-full flex-auto rounded-md border-2 px-3.5 py-2 text-gray-900 shadow-sm ring-1 ring-inset ring-white/10 focus:ring-2 focus:ring-inset focus:ring-indigo-500 sm:text-sm sm:leading-6 m-2"
                    placeholder="Url"
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
              OK
            </button>
          </div>
        </div>
      </form>
    </div>
  </div>
</template>

<style scoped>
.fondimage {
  background-image: url("../../assets/images/fondecran.png");
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  background-attachment: fixed;
  height: 50vh;
}
</style>

<script setup>
import { ref, onMounted } from "vue";
import axios from "axios";

// Variables
const data = ref("");
const isOpen = ref(false);

// barre de recherche
const searchValue = ref("");

// fonction de recherche
const search = async () => {
  try {
    const token = localStorage.getItem("token");
    const response = await axios.get(
      "https://apihackaton1.osc-fr1.scalingo.io/properties",
      {
        headers: {
          Authorization: `Bearer ${token}`,
        },
      }
    );
    data.value = response.data.filter(
      (item) =>
        item.title.includes(searchValue.value) ||
        item.description.includes(searchValue.value) ||
        item.location.includes(searchValue.value) ||
        item.price.toString().includes(searchValue.value)
    );
  } catch (error) {
    console.error("Erreur lors de la recherche: ", error);
  }
};

onMounted(async () => {
  try {
    const response = await axios.get(
      "https://apihackaton1.osc-fr1.scalingo.io/properties"
    );
    data.value = response.data.filter(
      (item) =>
        item.title.includes(searchValue.value) ||
        item.description.includes(searchValue.value) ||
        item.location.includes(searchValue.value) ||
        item.price.toString().includes(searchValue.value)
    );
    console.log(data.value);
  } catch (error) {
    console.error("Erreur lors de la récupération des annonces: ", error);
  }
});

const handleCreationRaccourci = async () => {
  try {
    const token = localStorage.getItem("token");
    const response = await axios.post(
      "https://apihackaton1.osc-fr1.scalingo.io/create-properties",
      {
        title: title.value,
        description: description.value,
        price: price.value,
        location: location.value,
      },
      {
        headers: {
          Authorization: `Bearer ${token}`,
        },
      }
    );
    isOpen.value = false;
    router.go();
    alert("Annonce modifiée avec succès");

    // Reset des valeurs
    title.value = "";
    description.value = "";
    price.value = "";
    location.value = "";
  } catch (error) {
    console.error("Erreur lors de la création de l'annonce: ", error);
    alert("Erreur lors de l'ajout de l'annonce");
  }
};
</script>
