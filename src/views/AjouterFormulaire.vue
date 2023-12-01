<template>
  <div class="container">
    <h1 class="text-center">Formulaire</h1>
    <div class="row mb-3">
      <label for="titre" class="col-sm-2 col-form-label">Titre</label>
      <div class="col-sm-10">
        <input
          type="text"
          name="titre"
          id="titre"
          v-model="titre"
          class="form-control"
          required
        />
      </div>
    </div>
    <div class="row mb-3">
      <label for="url" class="col-sm-2 col-form-label">URL</label>
      <div class="col-sm-10">
        <input
          type="url"
          name="url"
          id="url"
          v-model="url"
          class="form-control"
          required
        />
      </div>
    </div>

    <div class="row mb-3">
      <div class="col-sm-10 offset-sm-2">
        <button
          v-if="edit"
          @click="edit = false"
          class="btn btn-secondary mr-2"
        >
          Annuler
        </button>
        <button v-else @click="createContact" class="btn btn-primary">
          Ajouter
        </button>
      </div>
    </div>
  </div>

  <!-- création du formulaire inscription modal -->
  <div class="container mx-auto">
    <div class="flex justify-center">
      <for
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
      </for>
    </div>
  </div>
</template>

<script>
const BASE_ID = import.meta.env.VITE_APP_BASS_ID;
const TABLE_NAME = "ajouterraccourci";
const API_TOKEN = import.meta.env.VITE_APP_TOKEN;

export default {
  data() {
    return {
      contacts: [],
      titre: "",
      url: "",
    };
  },
  methods: {
    handleResetForm() {
      this.titre = "";
      this.url = "";
    },

    getContacts() {
      fetch(`https://api.airtable.com/v0/${BASE_ID}/${TABLE_NAME}`, {
        headers: {
          Authorization: `Bearer ${API_TOKEN}`,
        },
      })
        .then((response) => response.json())
        .then((data) => {
          this.contacts = data.records;
        })
        .catch((error) => {
          console.log(error);
        });
    },

    createContact() {
      fetch(`https://api.airtable.com/v0/${BASE_ID}/${TABLE_NAME}`, {
        headers: {
          Authorization: `Bearer ${API_TOKEN}`,
          "Content-Type": "application/json",
        },
        method: "POST",
        body: JSON.stringify({
          fields: {
            titre: this.titre,
            Url: this.url,
          },
        }),
      })
        .then((response) => response.json())
        .then((data) => {
          alert("ajouté avec succès");
          this.getContacts();
          this.handleResetForm();
        })
        .catch((error) => {
          console.log(error);
        });
    },
  },
};
</script>
<style></style>
