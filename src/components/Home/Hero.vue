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
  <div
    class="mx-auto max-w-xs px-4 py-16 sm:px-6 sm:py-24 lg:max-w-7xl lg:px-8"
  >
    <div class="mt-6 grid grid-cols-1 lg:grid-cols-4 gap-x-6 gap-y-10">
      <div
        v-for="item in items"
        :key="item.id"
        class="relative flex flex-col items-center hover:bg-gray-100 transition-colors duration-200 p-5 rounded-lg w-64"
        @mouseover="hoveredItem = item.id"
        @mouseleave="hoveredItem = null"
      >
        <!-- Conteneur pour l'image et l'icône -->
        <div class="flex items-center">
          <a :href="item.fields.Url" target="_blank">
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
          </a>

          <!-- Icône avec options -->
          <div
            class="absolute top-0 right-0 inline-block text-left ml-4"
            v-show="hoveredItem === item.id"
          >
            <div>
              <button
                type="button"
                class="flex items-center justify-center w-12 h-12 rounded-full focus:outline-none hover:bg-gray-200"
                id="options-menu"
                @click="toggleMenu(item.id)"
                aria-haspopup="true"
                :aria-expanded="open[item.id]"
              >
                <!-- Icone avec trois points -->
                <svg
                  v-show="hoveredItem === item.id"
                  viewBox="0 0 16 16"
                  class="h-5 w-5 text-gray-500"
                  fill="currentColor"
                  xmlns="http://www.w3.org/2000/svg"
                >
                  <path
                    fill-rule="evenodd"
                    d="M9.5 13a1.5 1.5 0 1 1-3 0 1.5 1.5 0 0 1 3 0zm0-5a1.5 1.5 0 1 1-3 0 1.5 1.5 0 0 1 3 0zm0-5a1.5 1.5 0 1 1-3 0 1.5 1.5 0 0 1 3 0z"
                  />
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
                  @click.prevent="
                    prepareForEdit(item.id);
                    isEditOpen = true;
                  "
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

  <!-- creation formulaire de modal  -->
  <div class="container mx-auto">
    <div class="flex justify-center">
      <div
        v-show="isOpen"
        class="absolute inset-0 flex items-center justify-center bg-gray-700 bg-opacity-50 z-10"
      >
        <form
          @submit.prevent="createContact"
          class="w-full md:w-2/3 lg:w-2/5 p-6 bg-white rounded-md shadow-xl m-2 mt-2"
        >
          <div class="flex items-center justify-between">
            <h1
              class="text-2xl"
              style="text-transform: uppercase; font-weight: 700"
            >
              Ajouter un raccourci
            </h1>
            <svg
              @click="
                isOpen = false;
                handleResetForm();
              "
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
                    autocomplete="off"
                    class="min-w-full flex-auto rounded-md border-2 px-3.5 py-2 text-gray-900 shadow-sm ring-1 ring-inset ring-white/10 focus:ring-2 focus:ring-inset focus:ring-indigo-500 sm:text-sm sm:leading-6 m-2"
                    placeholder="Url"
                    required
                  />
                </div>
              </div>
              <div class="sm:col-span-5">
                <div class="mt-2">
                  <label for="imageicon">Lien image icon</label>
                  <input
                    v-model="imageicon"
                    type="url"
                    autocomplete="off"
                    class="min-w-full flex-auto rounded-md border-2 px-3.5 py-2 text-gray-900 shadow-sm ring-1 ring-inset ring-white/10 focus:ring-2 focus:ring-inset focus:ring-indigo-500 sm:text-sm sm:leading-6 m-2"
                    placeholder="Lien image icon"
                  />
                </div>
              </div>
            </div>

            <button
              type="submit"
              class="ml-2 text-[#fff] bg-[#43B7BE] rounded hover:bg-[#3b5998]/90 focus:ring-4 focus:outline-none focus:ring-[#3b5998]/50 text-sm px-5 py-2.5 text-center inline-flex items-center dark:focus:ring-[#3b5998]/55"
            >
              Ajouter
            </button>
          </div>
        </form>
      </div>
    </div>
  </div>

  <!-- modifier formulaire de modal  -->
  <div class="container mx-auto">
    <div class="flex justify-center">
      <div
        v-show="isEditOpen"
        class="absolute inset-0 flex items-center justify-center bg-gray-700 bg-opacity-50 z-10"
      >
        <form
          @submit.prevent="updateContact(selectedId)"
          class="w-full md:w-2/3 lg:w-2/5 p-6 bg-white rounded-md shadow-xl m-2 mt-2"
        >
          <div class="flex items-center justify-between">
            <h1
              class="text-2xl"
              style="text-transform: uppercase; font-weight: 700"
            >
              modifier un raccourci
            </h1>
            <svg
              @click="
                isEditOpen = false;
                handleResetForm();
              "
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
                    autocomplete="off"
                    class="min-w-full flex-auto rounded-md border-2 px-3.5 py-2 text-gray-900 shadow-sm ring-1 ring-inset ring-white/10 focus:ring-2 focus:ring-inset focus:ring-indigo-500 sm:text-sm sm:leading-6 m-2"
                    placeholder="Url"
                    required
                  />
                </div>
              </div>
              <div class="sm:col-span-5">
                <div class="mt-2">
                  <label for="imageicon">Lien image icon</label>
                  <input
                    v-model="imageicon"
                    type="url"
                    autocomplete="off"
                    class="min-w-full flex-auto rounded-md border-2 px-3.5 py-2 text-gray-900 shadow-sm ring-1 ring-inset ring-white/10 focus:ring-2 focus:ring-inset focus:ring-indigo-500 sm:text-sm sm:leading-6 m-2"
                    placeholder="Lien image icon"
                  />
                </div>
              </div>
            </div>

            <button
              type="submit"
              class="ml-2 text-[#fff] bg-[#43B7BE] rounded hover:bg-[#3b5998]/90 focus:ring-4 focus:outline-none focus:ring-[#3b5998]/50 text-sm px-5 py-2.5 text-center inline-flex items-center dark:focus:ring-[#3b5998]/55"
            >
              Modifier
            </button>
          </div>
        </form>
      </div>
    </div>
  </div>

  <!-- creation texte chargement et si trouve pas élément-->
  <div
    v-if="loadingMessage"
    class="fixed top-1/3 left-1/2 transform -translate-x-1/2 -translate-y-1/2 uppercase"
  >
    Chargement...
  </div>

  <div
    v-if="!items.length && !loadingMessage"
    class="fixed top-1/3 left-1/2 transform -translate-x-1/2 -translate-y-1/2 uppercase text-red-500"
  >
    Aucun élément trouvé.
  </div>

  <!--message alert -->
  <transition name="slide-fade">
    <div
      v-show="showAlert"
      :class="{ 'alert-hide': !showAlert }"
      id="alert-border-3"
      class="fixed top-0 right-0 flex items-center p-5 mb-4 text-green-800 border-t-4 border-green-300 bg-green-50 dark:text-green-400 dark:bg-gray-800 dark:border-green-800"
      role="alert"
    >
      <svg
        class="flex-shrink-0 w-4 h-4"
        aria-hidden="true"
        xmlns="http://www.w3.org/2000/svg"
        fill="currentColor"
        viewBox="0 0 20 20"
      >
        <path
          d="M10 .5a9.5 9.5 0 1 0 9.5 9.5A9.51 9.51 0 0 0 10 .5ZM9.5 4a1.5 1.5 0 1 1 0 3 1.5 1.5 0 0 1 0-3ZM12 15H8a1 1 0 0 1 0-2h1v-3H8a1 1 0 0 1 0-2h2a1 1 0 0 1 1 1v4h1a1 1 0 0 1 0 2Z"
        />
      </svg>
      <div class="ms-3 text-sm font-medium">
        Votre raccourci a été créé avec succès !
      </div>
    </div>
  </transition>

  <!--message supprime un raccourci -->
  <transition name="slide-fade">
    <div
      v-show="deleteAlert"
      :class="{ 'alert-hide': !deleteAlert }"
      id="alert-border-3"
      class="fixed top-0 right-0 flex items-center p-5 mb-4 text-red-600 border-t-4 border-red-600 bg-green-50 dark:text-red-600 dark:bg-gray-800 dark:border-red-600"
      role="alert"
    >
      <svg
        class="flex-shrink-0 w-4 h-4"
        aria-hidden="true"
        xmlns="http://www.w3.org/2000/svg"
        fill="currentColor"
        viewBox="0 0 20 20"
      >
        <path
          d="M10 .5a9.5 9.5 0 1 0 9.5 9.5A9.51 9.51 0 0 0 10 .5ZM9.5 4a1.5 1.5 0 1 1 0 3 1.5 1.5 0 0 1 0-3ZM12 15H8a1 1 0 0 1 0-2h1v-3H8a1 1 0 0 1 0-2h2a1 1 0 0 1 1 1v4h1a1 1 0 0 1 0 2Z"
        />
      </svg>
      <div class="ms-3 text-sm font-medium">
        Votre raccourci bien supprimer !
      </div>
    </div>
  </transition>

  <!--message modifier un raccourci -->
  <transition name="slide-fade">
    <div
      v-show="editAlert"
      :class="{ 'alert-hide': !editAlert }"
      id="alert-border-3"
      class="fixed top-0 right-0 flex items-center p-5 mb-4 text-green-800 border-t-4 border-green-300 bg-green-50 dark:text-green-400 dark:bg-gray-800 dark:border-green-800"
      role="alert"
    >
      <svg
        class="flex-shrink-0 w-4 h-4"
        aria-hidden="true"
        xmlns="http://www.w3.org/2000/svg"
        fill="currentColor"
        viewBox="0 0 20 20"
      >
        <path
          d="M10 .5a9.5 9.5 0 1 0 9.5 9.5A9.51 9.51 0 0 0 10 .5ZM9.5 4a1.5 1.5 0 1 1 0 3 1.5 1.5 0 0 1 0-3ZM12 15H8a1 1 0 0 1 0-2h1v-3H8a1 1 0 0 1 0-2h2a1 1 0 0 1 1 1v4h1a1 1 0 0 1 0 2Z"
        />
      </svg>
      <div class="ms-3 text-sm font-medium">
        Votre raccourci a été modifié avec succès !
      </div>
    </div>
  </transition>
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

.lg\:py-16 {
  padding-top: 1rem;
  padding-bottom: 0rem;
}
.slide-fade-enter-active {
  transition: all 0.3s ease;
}
.slide-fade-leave-active {
  transition: all 0.8s cubic-bezier(1, 0.5, 0.8, 1);
}
.slide-fade-enter, .slide-fade-leave-to
/* .slide-fade-leave-active below version 2.1.8 */ {
  transform: translateX(10px);
  opacity: 0;
}
</style>

<script>
const BASE_ID = "appHcgz06DExsXT5D";
const TABLE_NAME = "ajouterraccourci";
const VIEW_NAME = "Grid view";
const API_TOKEN =
  "patxW6p2WhSK3Lna1.13193576d26339128db92686df5cdd61fac9f732e577f2a0ad0419c655dc6255";

const BASE_URL = `https://api.airtable.com/v0/${BASE_ID}/${TABLE_NAME}`;

export default {
  data() {
    return {
      open: [],
      items: [],
      hoveredItem: null,
      isOpen: false,
      isEditOpen: false,
      editAlert: false,
      titre: "",
      url: "",
      imageicon: "",
      loadingMessage: false, // Ajout de l'état de chargement
      showAlert: false,
      deleteAlert: false,
    };
  },
  methods: {
    getItems() {
      this.loadingMessage = true; // Commencer le chargement

      this.$nextTick()
        .then(() => {
          // Remplacer par votre logique de chargement de données
          return fetch(`${BASE_URL}`);
        })
        .then((response) => response.json())
        .then((data) => {
          this.items = data;
          this.loadingMessage = false; // Fin du chargement
        })
        .catch((error) => {
          console.error("Error:", error);
          this.loadingMessage = false; // Fin du chargement même en cas d'erreur
        });
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

          // Ajouter le nouvel élément au tableau items
          this.items.push(data);

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
          this.deleteAlert = true;
          this.getContacts();
          this.open[id] = false;
          // Faire disparaître l'alerte après 5 secondes
          setTimeout(() => {
            this.deleteAlert = false;
          }, 3000);
        })
        .catch((error) => {
          console.log(error);
        });
    },

    // Méthode pour modifier un contact
    updateContact(id) {
      // Trouver l'élément dans le tableau 'items'
      const item = this.items.find((item) => item.id === id);

      // Vérifier si les valeurs du formulaire sont différentes des valeurs initiales
      if (
        item &&
        (this.titre !== item.fields.titre ||
          this.url !== item.fields.Url ||
          this.imageicon !== item.fields.imageicon)
      ) {
        fetch(`https://api.airtable.com/v0/${BASE_ID}/${TABLE_NAME}/${id}`, {
          headers: {
            Authorization: `Bearer ${API_TOKEN}`,
            "Content-Type": "application/json",
          },
          method: "PATCH",
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
            // Mettre à jour l'élément dans le tableau 'items'
            if (item) {
              item.fields.titre = this.titre;
              item.fields.Url = this.url;
              item.fields.imageicon = this.imageicon;
            }
            this.prepareForEdit();
            this.handleResetForm();
            this.isEditOpen = false;

            // Afficher l'alerte de modification
            this.editAlert = true;

            // Faire disparaître l'alerte après 5 secondes
            setTimeout(() => {
              this.editAlert = false;
            }, 5000);
          })
          .catch((error) => {
            console.log(error);
          });
      } else {
        // Si aucune modification n'a été faite, fermer le modal sans déclencher l'alerte
        this.isEditOpen = false;
      }
    },

    // Méthode pour préparer le formulaire de modification
    prepareForEdit(id) {
      const item = this.items.find((item) => item.id === id);
      if (item) {
        this.titre = item.fields.titre;
        this.url = item.fields.Url;
        this.imageicon = item.fields.imageicon;
        this.selectedId = item.id;
      }
    },
  },

  mounted() {
    this.getItems();
    document.body.style.backgroundImage = `url(${this.backgroundUrl})`;
    this.loadingMessage = true;
    setTimeout(() => {
      this.getContacts();
      this.loadingMessage = false;
    }, 2000);
  },
};
</script>
