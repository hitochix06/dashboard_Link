<template>
  <!-- Création de la barre de recherche -->
  <div class="py-3 bg-[#E8EEF5] px-1">
    <div class="max-w-md mx-auto rounded-lg overflow-hidden md:max-w-xl">
      <div class="md:flex">
        <div class="w-full p-3">
          <div class="relative shadow-xl bg-white overflow-hidden rounded-lg">
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
              <div class="uppercase">Dépose une annonce</div>
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- creation bouton trier par date -->
  <!-- <div class="container mx-auto flex justify-end mt-10">
    <Menu as="div" class="relative inline-block text-left">
      <div>
        <MenuButton
          class="inline-flex w-full justify-center gap-x-1.5 rounded-md bg-white px-3 py-2 text-sm font-semibold text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 hover:bg-gray-50"
        >
          Trier par date
          <ChevronDownIcon
            class="-mr-1 h-5 w-5 text-gray-400"
            aria-hidden="true"
          />
        </MenuButton>
      </div>

      <transition
        enter-active-class="transition ease-out duration-100"
        enter-from-class="transform opacity-0 scale-95"
        enter-to-class="transform opacity-100 scale-100"
        leave-active-class="transition ease-in duration-75"
        leave-from-class="transform opacity-100 scale-100"
        leave-to-class="transform opacity-0 scale-95"
      >
        <MenuItems
          class="absolute right-0 z-10 mt-2 w-56 origin-top-right rounded-md bg-white shadow-lg ring-1 ring-black ring-opacity-5 focus:outline-none"
        >
          <div class="py-1">
            <MenuItem v-slot="{ active }">
              <a
                href="#"
                :class="[
                  active ? 'bg-gray-100 text-gray-900' : 'text-gray-700',
                  'block px-4 py-2 text-sm',
                ]"
                >Account settings</a
              >
            </MenuItem>
            <MenuItem v-slot="{ active }">
              <a
                href="#"
                :class="[
                  active ? 'bg-gray-100 text-gray-900' : 'text-gray-700',
                  'block px-4 py-2 text-sm',
                ]"
                >Support</a
              >
            </MenuItem>
            <MenuItem v-slot="{ active }">
              <a
                href="#"
                :class="[
                  active ? 'bg-gray-100 text-gray-900' : 'text-gray-700',
                  'block px-4 py-2 text-sm',
                ]"
                >License</a
              >
            </MenuItem>
            <form method="POST" action="#">
              <MenuItem v-slot="{ active }">
                <button
                  type="submit"
                  :class="[
                    active ? 'bg-gray-100 text-gray-900' : 'text-gray-700',
                    'block w-full px-4 py-2 text-left text-sm',
                  ]"
                >
                  Sign out
                </button>
              </MenuItem>
            </form>
          </div>
        </MenuItems>
      </transition>
    </Menu>
  </div> -->

  <!-- Creation de carte pour les annonces -->
  <div class="bg-white">
    <div
      class="mx-auto max-w-2xl px-4 py-16 sm:px-6 sm:py-24 lg:max-w-7xl lg:px-8"
    >
      <h2 class="text-2xl font-bold tracking-tight text-gray-900">
        Annonces récentes
      </h2>

      <!-- Création de la carte -->
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
          <button
            @click="openEditForm(item)"
            class="absolute top-0 p-2 left-0 text-gray-400 hover:text-green-500"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 24 24"
              stroke-width="1.5"
              stroke="currentColor"
              class="w-8 h-8"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                d="M9.53 16.122a3 3 0 00-5.78 1.128 2.25 2.25 0 01-2.4 2.245 4.5 4.5 0 008.4-2.245c0-.399-.078-.78-.22-1.128zm0 0a15.998 15.998 0 003.388-1.62m-5.043-.025a15.994 15.994 0 011.622-3.395m3.42 3.42a15.995 15.995 0 004.764-4.648l3.876-5.814a1.151 1.151 0 00-1.597-1.597L14.146 6.32a15.996 15.996 0 00-4.649 4.763m3.42 3.42a6.776 6.776 0 00-3.42-3.42"
              />
            </svg>
          </button>
          <div class="mt-4 flex justify-between">
            <div>
              <h1 class="text-lg text-black-900 m-5">
                {{ item.title }}
              </h1>
              <p class="mt-1 text-sm text-black-900 m-5">
                {{ item.description }}
              </p>
              <p class="mt-1 text-sm text-black-900 m-5">
                {{ item.location }}
              </p>
            </div>
            <p class="text-lg font-medium m-5 text-gray-900">
              {{ item.price }}€
            </p>

            <!-- 
               Création du bouton supprimer -->
            <button
              @click="deleteProduct(item.id)"
              class="absolute top-0 right-0 p-2 text-gray-400 hover:text-red-500"
            >
              <svg
                xmlns="http://www.w3.org/2000/svg"
                viewBox="0 0 24 24"
                fill="currentColor"
                class="w-8 h-8"
              >
                <path
                  fill-rule="evenodd"
                  d="M16.5 4.478v.227a48.816 48.816 0 013.878.512.75.75 0 11-.256 1.478l-.209-.035-1.005 13.07a3 3 0 01-2.991 2.77H8.084a3 3 0 01-2.991-2.77L4.087 6.66l-.209.035a.75.75 0 01-.256-1.478A48.567 48.567 0 017.5 4.705v-.227c0-1.564 1.213-2.9 2.816-2.951a52.662 52.662 0 013.369 0c1.603.051 2.815 1.387 2.815 2.951zm-6.136-1.452a51.196 51.196 0 013.273 0C14.39 3.05 15 3.684 15 4.478v.113a49.488 49.488 0 00-6 0v-.113c0-.794.609-1.428 1.364-1.452zm-.355 5.945a.75.75 0 10-1.5.058l.347 9a.75.75 0 101.499-.058l-.346-9zm5.48.058a.75.75 0 10-1.498-.058l-.347 9a.75.75 0 001.5.058l.345-9z"
                  clip-rule="evenodd"
                />
              </svg>
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- creation formulaire de modal  -->
  <div class="container mx-auto">
    <div class="flex justify-center">
      <form
        @submit.prevent="handleCreationAnnonces"
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
              Déposer une annonce
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
          <!-- 
          <div class="mt-2">
            <label for="image" class="block text-sm font-medium leading-6 text-gray-900">Image</label>
            <input
              type="file"
              id="image"
              name="image"
              accept="image/*"
              class="min-w-full flex-auto rounded-md border-2 px-3.5 py-2 text-gray-900 shadow-sm ring-1 ring-inset ring-white/10 focus:ring-2 focus:ring-inset focus:ring-indigo-500 sm:text-sm sm:leading-6 m-2"
              required
            />
          </div> -->

          <div class="mt-4">
            <div class="mt-10 grid grid-cols-1 gap-x-6 gap-y-8 sm:grid-cols-6">
              <div class="sm:col-span-3">
                <div class="mt-2">
                  <label
                    for="first-name"
                    class="block text-sm font-medium leading-6 text-gray-900"
                    >Titre</label
                  >
                  <input
                    v-model="title"
                    type="text"
                    name="first-name"
                    id="first-name"
                    autocomplete="given-name"
                    class="min-w-full flex-auto rounded-md border-2 px-3.5 py-2 text-gray-900 shadow-sm ring-1 ring-inset ring-white/10 focus:ring-2 focus:ring-inset focus:ring-indigo-500 sm:text-sm sm:leading-6 m-2"
                    placeholder="Titre"
                    required
                  />
                </div>
              </div>

              <div class="sm:col-span-3">
                <div class="mt-2">
                  <label for="Price">Prix</label>
                  <input
                    v-model="price"
                    type="number"
                    name="price"
                    id="price"
                    autocomplete="off"
                    class="min-w-full flex-auto rounded-md border-2 px-3.5 py-2 text-gray-900 shadow-sm ring-1 ring-inset ring-white/10 focus:ring-2 focus:ring-inset focus:ring-indigo-500 sm:text-sm sm:leading-6 m-2"
                    placeholder="Prix €"
                    required
                  />
                </div>
              </div>

              <div class="sm:col-span-3">
                <div class="mt-2">
                  <label
                    for="location"
                    class="block text-sm font-medium leading-6 text-gray-900"
                    >Emplacement</label
                  >
                  <input
                    v-model="location"
                    id="location"
                    name="location"
                    type="text"
                    autocomplete="off"
                    class="min-w-full flex-auto rounded-md border-2 px-3.5 py-2 text-gray-900 shadow-sm ring-1 ring-inset ring-white/10 focus:ring-2 focus:ring-inset focus:ring-indigo-500 sm:text-sm sm:leading-6 m-2"
                    placeholder="Ville"
                    required
                  />
                </div>
              </div>

              <div class="sm:col-span-4">
                <label
                  for="description"
                  class="block text-sm font-medium leading-6 text-gray-900"
                  >Description</label
                >
                <div class="mt-2">
                  <textarea
                    v-model="description"
                    id="description"
                    name="description"
                    autocomplete="off"
                    class="min-w-full flex-auto rounded-md border-2 px-3.5 py-2 text-gray-900 shadow-sm ring-1 ring-inset ring-white/10 focus:ring-2 focus:ring-inset focus:ring-indigo-500 sm:text-sm sm:leading-6 m-2"
                    placeholder="Description"
                    required
                  ></textarea>
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

  <!-- creation mofification modal  -->
  <div class="container mx-auto">
    <div class="flex justify-center">
      <form
        @submit.prevent="handleMotifAnnonces"
        v-show="isEditing"
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
              modifier une annonce
            </h3>
            <svg
              @click="isEditing = false"
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
          <!-- 
          <div class="mt-2">
            <label for="image" class="block text-sm font-medium leading-6 text-gray-900">Image</label>
            <input
              type="file"
              id="image"
              name="image"
              accept="image/*"
              class="min-w-full flex-auto rounded-md border-2 px-3.5 py-2 text-gray-900 shadow-sm ring-1 ring-inset ring-white/10 focus:ring-2 focus:ring-inset focus:ring-indigo-500 sm:text-sm sm:leading-6 m-2"
              required
            />
          </div> -->

          <div class="mt-4">
            <div class="mt-10 grid grid-cols-1 gap-x-6 gap-y-8 sm:grid-cols-6">
              <div class="sm:col-span-3">
                <div class="mt-2">
                  <label
                    for="first-name"
                    class="block text-sm font-medium leading-6 text-gray-900"
                    >Titre</label
                  >
                  <input
                    v-model="title"
                    type="text"
                    name="first-name"
                    id="first-name"
                    autocomplete="given-name"
                    class="min-w-full flex-auto rounded-md border-2 px-3.5 py-2 text-gray-900 shadow-sm ring-1 ring-inset ring-white/10 focus:ring-2 focus:ring-inset focus:ring-indigo-500 sm:text-sm sm:leading-6 m-2"
                    placeholder="Titre"
                    required
                  />
                </div>
              </div>

              <div class="sm:col-span-3">
                <div class="mt-2">
                  <label for="Price">Prix</label>
                  <input
                    v-model="price"
                    type="number"
                    name="price"
                    id="price"
                    autocomplete="off"
                    class="min-w-full flex-auto rounded-md border-2 px-3.5 py-2 text-gray-900 shadow-sm ring-1 ring-inset ring-white/10 focus:ring-2 focus:ring-inset focus:ring-indigo-500 sm:text-sm sm:leading-6 m-2"
                    placeholder="Prix €"
                    required
                  />
                </div>
              </div>

              <div class="sm:col-span-3">
                <div class="mt-2">
                  <label
                    for="location"
                    class="block text-sm font-medium leading-6 text-gray-900"
                    >Emplacement</label
                  >
                  <input
                    v-model="location"
                    id="location"
                    name="location"
                    type="text"
                    autocomplete="off"
                    class="min-w-full flex-auto rounded-md border-2 px-3.5 py-2 text-gray-900 shadow-sm ring-1 ring-inset ring-white/10 focus:ring-2 focus:ring-inset focus:ring-indigo-500 sm:text-sm sm:leading-6 m-2"
                    placeholder="Ville"
                    required
                  />
                </div>
              </div>

              <div class="sm:col-span-4">
                <label
                  for="description"
                  class="block text-sm font-medium leading-6 text-gray-900"
                  >Description</label
                >
                <div class="mt-2">
                  <textarea
                    v-model="description"
                    id="description"
                    name="description"
                    autocomplete="off"
                    class="min-w-full flex-auto rounded-md border-2 px-3.5 py-2 text-gray-900 shadow-sm ring-1 ring-inset ring-white/10 focus:ring-2 focus:ring-inset focus:ring-indigo-500 sm:text-sm sm:leading-6 m-2"
                    placeholder="Description"
                    required
                  ></textarea>
                </div>
              </div>
            </div>

            <button
              @click="isEditing = false"
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
</template>

<script setup>
import { Menu, MenuButton, MenuItem, MenuItems } from "@headlessui/vue";
import { ChevronDownIcon } from "@heroicons/vue/20/solid";
import { ref, onMounted } from "vue";
import axios from "axios";
import { useRouter } from "vue-router";

// Router
const router = useRouter();

// Variables
const data = ref("");
const isOpen = ref(false);
const title = ref("");
const description = ref("");
const price = ref("");
const location = ref("");
const searchValue = ref("");
const editingId = ref(null);
const isEditing = ref(false);

const openEditForm = (item) => {
  isEditing.value = true;
  title.value = item.title;
  description.value = item.description;
  price.value = item.price;
  location.value = item.location;

  // Stockez l'ID de l'annonce que nous modifions
  editingId.value = item.id;
};

// Fonctions recherche
const search = async () => {
  try {
    const token = localStorage.getItem("token");
    const response = await axios.get(
      "https://apihackaton1.osc-fr1.scalingo.io/get-my-properties",
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

const handleCreationAnnonces = async () => {
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

// Fonction modification
const handleMotifAnnonces = async () => {
  try {
    const token = localStorage.getItem("token");
    const url = editingId.value
      ? `https://apihackaton1.osc-fr1.scalingo.io/update-properties/${editingId.value}`
      : "https://apihackaton1.osc-fr1.scalingo.io/create-properties";
    const method = editingId.value ? "put" : "post";
    const response = await axios[method](
      url,
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
    isEditing.value = false;
    
    router.go();

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

// Récupération des annonces
onMounted(async () => {
  try {
    const token = localStorage.getItem("token");
    const response = await axios.get(
      "https://apihackaton1.osc-fr1.scalingo.io/get-my-properties",
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
    console.log(data.value);
  } catch (error) {
    console.error("Erreur lors de la récupération des annonces: ", error);
  }
});

const deleteProduct = async (id) => {
  try {
    const token = localStorage.getItem("token");
    const response = await axios.delete(
      `https://apihackaton1.osc-fr1.scalingo.io/delete-properties/${id}`, // Utilisez le bon ID ici
      {
        headers: {
          Authorization: `Bearer ${token}`,
        },
      }
    );
    alert("Annonce supprimée avec succès");

    const responseProperties = await axios.get(
      "https://apihackaton1.osc-fr1.scalingo.io/get-my-properties",
      {
        headers: {
          Authorization: `Bearer ${token}`,
        },
      }
    );
    data.value = responseProperties.data;
  } catch (error) {
    console.error("Erreur lors de la suppression de l'annonce: ", error);
    alert("Erreur lors de la suppression de l'annonce");
  }
};
</script>
