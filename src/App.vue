<template>
  <!-- Composant App -->
  <div id="app" class="h-screen bg-sky-100">
    <!-- Conteneur de notifications (toasts) -->
    <div id="toast-container"></div>

    <!-- Section du menu -->
    <div class="menu-whole h-5/6 flex">
      <!-- Section d'entrée JSON et chargement -->
      <div class="field-loader w-1/2 h-full flex flex-col bg-white m-4 p-1 lg:m-12 lg:p-4 shadow-xl">
        <!-- Zone de saisie JSON -->
        <textarea
          v-model="jsonInput"
          placeholder="Entrez le JSON du menu ici"
          class="block p-2.5 h-full w-full text-xs lg:text-xl"
        ></textarea>
        <!-- Bouton pour charger le menu -->
        <button
          class="bg-blue-500 hover:bg-blue-400 text-white font-bold py-2 px-4 border-b-4 border-blue-700 hover:border-blue-500 rounded"
          @click="loadMenu"
        >
          Charger le menu
        </button>
      </div>

      <!-- Section du menu chargé -->
      <div class="menu-loaded w-1/2 h-full text-gray-900 bg-blue-500 rounded shadow-xl m-4 p-1 lg:m-12 lg:p-4">
        <!-- Affichage des éléments du menu -->
        <div
          :class="{
            'cursor-pointer flex justify-center': true,
            'bg-sky-100 top-1/2 flex justify-center': item.type === 'hiddenItem' && !item.isHidden
          }"
          v-for="(item, index) in getMenuItems()"
          :key="index"
        >
          <!-- Rendu dynamique des éléments du menu avec des composants -->
          <component
            :is="getComponentName(item)"
            :label="item.label"
            :permission="item.permission"
            :link="item.link"
            :articleCount="item.component === 'nbArticlesLabel' ? dynamicArticleCount : undefined"
            @updateArticleCount="updateArticleCount"
            @showToastOnLinkOpen="showToastOnLinkOpen"
          />
        </div>

        <!-- Bouton pour basculer l'affichage des éléments masqués -->
        <button
          class="bg-white hover:bg-sky-100 text-blue text-xs lg:text-lg font-bold py-2 px-4 border-b-4 border-blue-700 hover:border-blue-500 rounded w1/4 h-10"
          @click="toggleHiddenItems"
          v-if="hiddenItems.length > 0"
        >
          <div class="flex items-center">
            Autres
          </div>
        </button>
        <!-- Composant dynamique -->
        <component :is="dynamicComponent" v-if="dynamicComponent" />
      </div>
    </div>
  </div>
</template>

<script>
// Importation des composants
import nbArticlesLabel from "@/components/nbArticlesLabel.vue";
import nbArticlesField from "@/components/nbArticlesFields.vue";
import homeMenuItem from "@/components/homeMenuItem.vue";
import page1MenuItem from "@/components/page1MenuItem.vue";
import page4MenuItem from "@/components/page4MenuItem.vue";
import page5MenuItem from "@/components/page5MenuItem.vue";

export default {
  components: {
    nbArticlesLabel,
    nbArticlesField,
    homeMenuItem,
    page1MenuItem,
    page4MenuItem,
    page5MenuItem,
  },
  data() {
    // Données du composant
    return {
      dynamicArticleCount: 0,
      jsonInput: "",
      menuItems: [],
      hiddenItems: [],
      showHiddenItems: false,
      dynamicComponent: null,
    };
  },
  methods: {
    // Méthode pour charger le menu depuis le JSON
    loadMenu() {
      try {
        const parsedJson = JSON.parse(this.jsonInput);
        // Initialisation des éléments masqués et du menu principal
        this.hiddenItems = parsedJson.hiddenItems.map((item) => ({
          ...item,
          type: "hiddenItem",
          isHidden: item.isHidden || false
        }));
        this.menuItems = parsedJson.menuItems.map((item) => ({
          ...item,
          type: "menuItem"
        }));
        this.showHiddenItems = false;
      } catch (error) {
        console.error("Erreur lors du chargement du JSON :", error);
      }
    },
    // Méthode pour basculer l'affichage des éléments masqués
    toggleHiddenItems() {
      this.showHiddenItems = !this.showHiddenItems;
    },
    // Méthode pour obtenir la liste des éléments du menu à afficher
    getMenuItems() {
      const combinedItems = [...this.menuItems, ...this.hiddenItems];
      console.log(combinedItems);
      return this.showHiddenItems ? combinedItems : this.menuItems;
    },
    // Méthode pour mettre à jour le compte d'articles dynamique
    updateArticleCount(value) {
      this.dynamicArticleCount = value;
    },
    // Méthode pour afficher une notification lors de l'ouverture d'un lien
    showToastOnLinkOpen(label, permission) {
      if (label, permission) {
        this.showToast(label, permission);
      } else {
        console.error('Objet d\'élément invalide :');
      }
    },
    // Méthode pour afficher une notification
    showToast(label) {
      const toastElement = document.createElement("div");
      toastElement.className =
        "toast animate-fadein fixed flex items-center w-full max-w-xs p-4 space-x-4 text-gray-500 bg-white divide-x rtl:divide-x-reverse divide-gray-200 rounded-lg shadow top-5 right-5 dark:text-gray-400 dark:divide-gray-700 space-x dark:bg-gray-800";
      toastElement.textContent = `Ouverture de la page ${label}`;
      document.getElementById("toast-container").appendChild(toastElement);
      setTimeout(() => {
        toastElement.remove();
      }, 5000);
    },
    // Méthode pour obtenir le nom du composant à partir de l'élément du menu
    getComponentName(item) {
      const componentMap = {
        homeMenuItem,
        page1MenuItem,
        nbArticlesField,
        page4MenuItem,
        page5MenuItem,
        nbArticlesLabel,
      };
      return componentMap[item.component] || null;
    },
  },
};
</script>

<style lang="css">
  @import '@/assets/styles/tailwind.css';
</style>
