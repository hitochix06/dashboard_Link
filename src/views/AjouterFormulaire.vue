<template>
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

  <!-- creation formulaire de modal  -->
  <div class="container mx-auto">
    <div class="flex justify-center">
      <div
        v-show="isOpen"
        class="absolute inset-0 flex items-center justify-center bg-gray-700 bg-opacity-50"
      >
        <div
          class="w-full md:w-2/3 lg:w-2/5 p-6 bg-white rounded-md shadow-xl m-2 mt-2"
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
            <div class="mt-10 grid grid-cols-1 sm:grid-cols-6 py-2">
              <div class="sm:col-span-5">
                <div class="mt-2">
                  <label
                    for="titre"
                    class="block text-sm font-medium leading-6 text-gray-900"
                    >Titre</label
                  >
                  <input
                    v-model="titre"
                    type="text"
                    name="titre"
                    id="titre"
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
                    v-model="url"
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
              @click="createContact"
              class="ml-2 text-[#fff] bg-[#43B7BE] rounded hover:bg-[#3b5998]/90 focus:ring-4 focus:outline-none focus:ring-[#3b5998]/50 text-sm px-5 py-2.5 text-center inline-flex items-center dark:focus:ring-[#3b5998]/55"
            >
              Ajouter
            </button>
          </div>
        </div>
      </div>
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
      isOpen: false,
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

    createContact() {
      if (!this.titre || !this.url) {
        alert("Veuillez remplir tous les champs");
        return;
      }
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
          alert("Raccourci ajouté");
          this.isOpen = false;
          this.handleResetForm();
        })
        .catch((error) => {
          console.log(error);
          alert("Une erreur est survenue");
        });
    },
  },
};
</script>
<style></style>
