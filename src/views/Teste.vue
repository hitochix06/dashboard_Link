<template>
  <!-- Affichage des raccourcis -->
  <div
    class="mx-auto max-w-xs px-4 py-16 sm:px-6 sm:py-24 lg:max-w-7xl lg:px-8"
  >
    <div class="mt-6 grid grid-cols-1 lg:grid-cols-4 gap-x-6 gap-y-10">
      <a
        v-for="item in items"
        :key="item.id"
        :href="item.fields.Url"
        target="_blank"
        class="flex flex-col items-center hover:bg-gray-100 transition-colors duration-200 p-5 rounded-lg w-64"
      >
        <div class="rounded-full w-16 h-16 overflow-hidden">
          <img
            v-if="item.fields.imageicon"
            :src="item.fields.imageicon"
            alt="Icon"
            class="object-contain w-full h-full rounded-full"
          />
          <div
            v-else
            :style="{ backgroundColor: getRandomColor() }"
            class="object-contain w-full h-full rounded-full"
          ></div>
        </div>
        <div>
          <h1
            class="text-sm text-black-900 m-2 text-center font-bold uppercase"
          >
            {{ item.fields.titre }}
          </h1>
        </div>
      </a>
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
