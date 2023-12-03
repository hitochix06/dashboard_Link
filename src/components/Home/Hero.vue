<template>
  <div class="py-3 px-1">
    <div class="max-w-md mx-auto rounded-lg overflow-hidden md:max-w-xl">
      <div class="md:flex">
        <div class="w-full p-10">
          <!-- Bouton de navigation avec gestionnaire d'événements pour la redirection -->
          <div class="container mx-auto flex justify-center mt-10">
            <button
              @click="navigateToConnection"
              type="button"
              class="text-white bg-[#43B7BE] hover:bg-[#3b5998]/90 focus:ring-4 focus:outline-none focus:ring-[#3b5998]/50 font-medium rounded-lg text-sm px-5 py-2.5 text-center inline-flex items-center dark:focus:ring-[#3b5998]/55 me-2 mb-2"
            >
              <svg
                xmlns="http://www.w3.org/2000/svg"
                viewBox="0 0 24 24"
                fill="currentColor"
                class="w-6 h-6"
                width="20"
                height="20"
                aria-label="Ajouter"
              >
                <path
                  d="M12 3v18m9-9H3"
                  stroke="currentColor"
                  stroke-width="2"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                />
              </svg>
              <div class="uppercase">Ajouter un raccourci</div>
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- Affichage des raccourcis -->
  <div class="bg-white" v-if="datas.length">
    <div
      class="mx-auto max-w-2xl px-4 py-16 sm:px-6 sm:py-24 lg:max-w-7xl lg:px-8"
    >
      <div class="mt-6 grid grid-cols-1 lg:grid-cols-2 gap-x-6 gap-y-10">
        <!-- Correction : Utilisation de 'data' au lieu de 'datas' dans la boucle v-for -->
        <div v-for="data in datas" :key="data.id">
          <div
            class="aspect-h-1 aspect-w-1 w-full overflow-hidden rounded-md bg-gray-200 lg:aspect-none lg:h-60"
          >
            <!-- Emplacement pour l'image du raccourci -->
          </div>
          <div class="mt-4 flex justify-between">
            <div>
              <!-- Correction : Utilisation de 'data' au lieu de 'datas' pour accéder aux propriétés de l'élément -->
              <h1 class="text-lg text-black-900 m-5">{{ data.nom }}</h1>
              <p class="mt-1 text-sm text-black-900 m-5">{{ data.URL }}</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- Message si aucune donnée n'est trouvée ou en cours de chargement -->
  <div v-if="isLoading" class="text-center">Chargement des données...</div>
  <div v-if="!isLoading && !datas.length" class="text-center">
    Aucun raccourci trouvé.
  </div>
</template>

<script>
const BASE_ID = import.meta.env.VITE_APP_BASS_ID;
const TABLE_NAME = "ajouterraccourci";
const VIEW_NAME = "Grid view";
const API_TOKEN = import.meta.env.VITE_APP_TOKEN;

export default {
  data() {
    return {
      datas: [], // Liste des raccourcis
      searchValue: "", // Valeur de recherche
      titre: "",
      url: "",
      imageicon: "",

      isLoading: false, // Ajout de l'état de chargement
    };
  },
  mounted() {
    // Déplacement de l'appel de getContacts ici pour charger les données au montage
    this.getContacts();
  },
  methods: {
    navigateToConnection() {
      // Redirection vers la page de connexion
      this.$router.push("/connexion");
      // Suppression de l'appel à getContacts ici
    },
    // Méthode de récupération des contacts
    getContacts() {
      this.isLoading = true; // Commencer le chargement
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
          this.datas = data.records;
          this.isLoading = false; // Fin du chargement
        })
        .catch((error) => {
          console.log(error);
          this.isLoading = false; // Fin du chargement même en cas d'erreur
        });
    },
  },
};
</script>
