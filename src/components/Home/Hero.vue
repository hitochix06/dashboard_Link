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
              placeholder="Recherche non racourci"
              required
            />
          </div>
          <div class="container mx-auto flex justify-center mt-10">
            <button
              @click="$router.push('/connexion')"
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
                aria-label="Ajouter"
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
          ></div>
          <div class="mt-4 flex justify-between">
            <div>
              <h1 class="text-lg text-black-900 m-5">
                {{ item.nom }}
              </h1>
              <p class="mt-1 text-sm text-black-900 m-5">
                {{ item.URL }}
              </p>
            </div>
          </div>
        </div>
      </div>
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
      imageicon: "",
      showAlert: false,
    };
  },
  methods: {
    handleResetForm() {
      this.titre = "";
      this.imageicon = "";
      this.url = "";
    },

    getContacts() {
      fetch(
        `https://api.airtable.com/v0/${BASE_ID}/${TABLE_NAME}?view=${VIEW_NAME}`,
        {
          headers: {
            Authorization: `Bearer ${API_TOKEN}`,
          },
        }
      )
        .then((response) => response.json())
        .then((data) => {
          this.contacts = data.records;
        })
        .catch((error) => {
          console.log(error);
        });
    },

    // Supprimer un racourci
    deleteContact(id) {
      fetch(`https://api.airtable.com/v0/${BASE_ID}/${TABLE_NAME}/${id}`, {
        headers: {
          Authorization: `Bearer ${API_TOKEN}`,
        },
        method: "DELETE",
      })
        .then((response) => response.json())
        .then((data) => {
          this.getContacts();
        })
        .catch((error) => {
          console.log(error);
        });
    },

    //mettre a jour un racourci
    updateContact(id) {
      fetch(`https://api.airtable.com/v0/${BASE_ID}/${TABLE_NAME}/${id}`, {
        headers: {
          Authorization: `Bearer ${API_TOKEN}`,
          "Content-Type": "application/json",
        },
        method: "PATCH",
        body: JSON.stringify({
          fields: {
            Nom: this.nom,
            Prenom: this.prenom,
            Email: this.email,
          },
        }),
      })
        .then((response) => response.json())
        .then((data) => {
          this.handleResetForm();
          this.edit = false;
          this.getContacts();
        })
        .catch((error) => {
          console.log(error);
        });
    },

    // Ajouter un contact
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
            imageicon: this.imageicon,
          },
        }),
      })
        .then((response) => response.json())
        .then((data) => {
          this.getContacts();
          this.showAlert = true; // Afficher l'alerte
          this.isOpen = false;
          this.handleResetForm();

          // Faire disparaître l'alerte après 5 secondes
          setTimeout(() => {
            this.showAlert = false;
          }, 3000);
        })
        .catch((error) => {
          console.log(error);
          alert("Une erreur est survenue");
        });
    },
  },
  mounted() {
    this.getContacts();
  },
};
</script>
