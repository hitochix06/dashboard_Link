<template>
  <footer class="bg-gray-800 text-white p-3 hidden md:block">
    <div
      class="flex flex-col md:flex-row items-center justify-between space-y-4 md:space-y-0 md:space-x-4"
    >
      <!-- Ajout des classes pour le responsive design -->
      <div
        class="flex flex-col md:flex-row items-center space-y-4 md:space-y-0 md:space-x-4"
      >
        <!-- Ajout des classes pour le responsive design -->
        <a href="#" class="-m-1.5 p-1.5">
          <img
            class="h-20 w-auto"
            src="@/assets/logo.png"
            alt="logo ImmoZero"
          />
        </a>

        <a href="/cgu" class="text-white hover:underline">CGU</a>
        <a href="/mentions-legales" class="text-white hover:underline"
          >Mentions légales</a
        >
        <a href="/politique-confidentialite" class="text-white hover:underline"
          >Politique de confidentialité</a
        >
        <a href="https://www.facebook.com" target="_blank">
          <svg
            class="h-5 w-5"
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="2"
            stroke-linecap="round"
            stroke-linejoin="round"
          >
            <path
              d="M18 2h-3a5 5 0 0 0-5 5v3H7v4h3v8h4v-8h3l1-4h-4V7a1 1 0 0 1 1-1h3z"
            />
          </svg>
        </a>
        <a href="https://www.instagram.com" target="_blank">
          <svg
            class="h-5 w-5"
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="2"
            stroke-linecap="round"
            stroke-linejoin="round"
          >
            <rect x="2" y="2" width="20" height="20" rx="5" ry="5" />
            <path d="M16 11.37A4 4 0 1 1 12.63 8 4 4 0 0 1 16 11.37z" />
            <line x1="17.5" y1="6.5" x2="17.51" y2="6.5" />
          </svg>
        </a>
      </div>
      <p>© 2023 DashborardLink All rights reserved.</p>
      <!-- Ce texte sera aligné à droite -->
    </div>
  </footer>
</template>
