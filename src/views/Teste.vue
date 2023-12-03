<template>
  <!-- Affichage des raccourcis -->

  <div
    class="mx-auto max-w-2xl px-4 py-16 sm:px-6 sm:py-24 lg:max-w-7xl lg:px-8"
  >
    <div class="mt-6 grid grid-cols-1 lg:grid-cols-2 gap-x-6 gap-y-10">
      <!-- Correction : Utilisation de 'data' au lieu de 'datas' dans la boucle v-for -->
      <div v-for="item in items" :key="item.id">
        <div
          class="aspect-h-1 aspect-w-1 w-full overflow-hidden rounded-md bg-gray-200 lg:aspect-none lg:h-60"
        >
          <!-- Emplacement pour l'image du raccourci -->
        </div>
        <div class="mt-4 flex justify-between">
          <div>
            <!-- Correction : Utilisation de 'data' au lieu de 'datas' pour accéder aux propriétés de l'élément -->
            <h1 class="text-lg text-black-900 m-5">{{ item.fields.titre }}</h1>
            <p class="mt-1 text-sm text-black-900 m-5">{{ item.fields.Url }}</p>
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- Message si aucune donnée n'est trouvée ou en cours de chargement -->
  <div v-if="isLoading" class="text-center">Chargement des données...</div>
  <div v-if="!isLoading && !items.length" class="text-center">
    Aucun raccourci trouvé.
  </div>
</template>
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
