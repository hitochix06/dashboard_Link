<template>
  <div
    class="mx-auto max-w-xs px-4 py-16 sm:px-6 sm:py-24 lg:max-w-7xl lg:px-8"
  >
    <div class="mt-6 grid grid-cols-1 lg:grid-cols-4 gap-x-6 gap-y-10">
      <div
        v-for="item in items"
        :key="item.id"
        class="flex flex-col items-center hover:bg-gray-100 transition-colors duration-200 p-5 rounded-lg w-64"
      >
        <!-- Conteneur pour l'image et l'icône -->
        <div class="flex items-center">
          <div class="rounded-full w-16 h-16 overflow-hidden">
            <img
              v-if="item.fields.imageicon"
              :src="item.fields.imageicon"
              alt="Icon"
              class="object-full w-full h-full rounded-full"
            />
            <div
              v-else
              :style="{ backgroundColor: getRandomColor() }"
              class="object-full w-full h-full rounded-full"
            ></div>
          </div>

          <!-- Icône avec options -->
          <div class="relative inline-block text-left ml-4">
            <div>
              <button
                type="button"
                class="flex items-center justify-center w-12 h-12 rounded-full focus:outline-none"
                id="options-menu"
                @click="toggleMenu(item.id)"
                aria-haspopup="true"
                :aria-expanded="open[item.id]"
              >
                <!-- Icone avec trois points -->
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke="currentColor"
                  class="h-6 w-6 text-gray-500"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M12 6v6m0 0v6m0-6h6m-6 0H6"
                  ></path>
                </svg>
              </button>
            </div>

            <!-- Menu déroulant -->
            <div
              v-show="open[item.id]"
              class="origin-top-right absolute right-0 mt-2 w-56 rounded-md shadow-lg bg-white ring-1 ring-black ring-opacity-5"
            >
              <div
                class="py-1"
                role="menu"
                aria-orientation="vertical"
                aria-labelledby="options-menu"
              >
                <a
                  href="#"
                  class="block px-4 py-2 text-sm text-gray-700 hover:bg-gray-100"
                  role="menuitem"
                  @click.prevent="updateContact(item.id)"
                >
                  Modifier le raccourci
                </a>
                <a
                  href="#"
                  class="block px-4 py-2 text-sm text-gray-700 hover:bg-gray-100"
                  role="menuitem"
                  @click.prevent="
                    deleteContact(item.id);
                    open[item.id] = false;
                  "
                >
                  Supprimer
                </a>
              </div>
            </div>
          </div>
        </div>

        <!-- Titre -->
        <div>
          <h1
            class="text-sm text-black-900 m-2 text-center font-bold uppercase"
          >
            {{ item.fields.titre }}
          </h1>
        </div>
      </div>
    </div>
  </div>
</template>
<style scoped></style>

<script>
const BASE_ID = import.meta.env.VITE_APP_BASS_ID;
const TABLE_NAME = "ajouterraccourci";
const VIEW_NAME = "Grid view";
const API_TOKEN = import.meta.env.VITE_APP_TOKEN;

const BASE_URL = `https://api.airtable.com/v0/${BASE_ID}/${TABLE_NAME}`;

export default {
  data() {
    return {
      isOpen: false,
      items: [],
      titre: "",
      url: "",
      imageicon: "",
      isLoading: false, // Ajout de l'état de chargement
      showAlert: false,
      open: [],
    };
  },
  methods: {
    getRandomColor() {
      const letters = "0123456789ABCDEF";
      let color = "#";
      for (let i = 0; i < 6; i++) {
        color += letters[Math.floor(Math.random() * 16)];
      }
      return color;
    },

    handleResetForm() {
      this.titre = "";
      this.imageicon = "";
      this.url = "";
    },
    toggleMenu(id) {
      this.open = { [id]: true };
    },

    createContact() {
      fetch(`${BASE_URL}`, {
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

    // Méthode pour ajouter un nouveau contact
    getContacts() {
      this.isLoading = true; // Commencer le chargement
      fetch(`${BASE_URL}?view=${VIEW_NAME}`, {
        headers: {
          Authorization: `Bearer ${API_TOKEN}`,
        },
      })
        .then((response) => response.json())
        .then((data) => {
          this.items = data.records;
          this.isLoading = false; // Fin du chargement
        })
        .catch((error) => {
          console.log(error);
          this.isLoading = false; // Fin du chargement même en cas d'erreur
        });
    },

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
          this.open[id] = false;
        })
        .catch((error) => {
          console.log(error);
        });
    },
    updateContact(id) {
      fetch(`https://api.airtable.com/v0/${BASE_ID}/${TABLE_NAME}/${id}`, {
        headers: {
          Authorization: `Bearer ${API_TOKEN}`,
          "Content-Type": "application/json",
        },
        method: "PATCH",
        body: JSON.stringify({
          fields: {
            Titre: this.titre,
            Url: this.url,
            Imageicon: this.imageicon,
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
    handleUpdate(contact) {
      this.titre = contact.fields.Titre;
      this.url = contact.fields.Url;
      this.imageicon = contact.fields.Imageicon;
      this.selectedId = contact.id;
      this.edit = true;
    },
  },

  mounted() {
    this.getContacts();
  },
};
</script>
