<template>
  <!-- Affichage des raccourcis -->
  <div
    class="mx-auto max-w-2xl px-4 py-16 sm:px-6 sm:py-24 lg:max-w-7xl lg:px-8"
  >
    <div class="mt-6 grid grid-cols-1 lg:grid-cols-2 gap-x-6 gap-y-10">
      <div
        v-for="item in items"
        :key="item.id"
        class="flex flex-col items-center"
      >
        <div class="rounded-full w-24 h-24 overflow-hidden">
          <img
            v-if="item.fields.imageicon"
            :src="item.fields.imageicon"
            alt="Icon"
            class="object-cover w-full h-full"
          />
          <div
            v-else
            :style="{ backgroundColor: getRandomColor() }"
            class="object-cover w-full h-full"
          ></div>
        </div>
        <div>
          <h1 class="text-lg text-black-900 m-5 text-center">
            {{ item.fields.titre }}
          </h1>
          <p class="mt-1 text-sm text-black-900 m-5 text-center">
            {{ item.fields.Url }}
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
/* Ajoutez ces styles pour rendre le cadre rond et petit */
.rounded-md {
  border-radius: 100%;

  width: 100px;
  height: 100px;
}

.text-center {
  text-align: center;
}
</style>

<script>
const BASE_ID = import.meta.env.VITE_APP_BASS_ID;
const TABLE_NAME = "ajouterraccourci";
const VIEW_NAME = "Grid view";
const API_TOKEN = import.meta.env.VITE_APP_TOKEN;

const BASE_URL = `https://api.airtable.com/v0/${BASE_ID}/${TABLE_NAME}`;

export default {
  data() {
    return {
      items: [],
      titre: "",
      url: "",
      imageicon: "",
      isLoading: false, // Ajout de l'état de chargement
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
      this.url = "";
      this.imageicon = "";
      this.edit = false;
      this.selectedId = null;
    },
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
