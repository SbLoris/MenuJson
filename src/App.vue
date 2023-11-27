<template>
  <div id="app">
    <div class="menu-whole">
      <div class="field-loader">
        <textarea  v-model="jsonInput" placeholder="Entrez le JSON du menu ici"></textarea>
        <button  @click="loadMenu">Charger le menu</button>
      </div>
      <div class="menu-loaded bg-blue-500">
        <div v-for="(item, index) in getMenuItems()" :key="index">
          <component
            :is="getComponentName(item)"
            :label="item.label"
            :permission="item.permission"
            :link="item.link"
            :articleCount="item.component === 'nbArticlesLabel' ? dynamicArticleCount : undefined"
            @updateArticleCount="updateArticleCount"
            @showToastOnLinkOpen="showToast"
          />
        </div>
        <button @click="toggleHiddenItems" v-if="hiddenItems.length > 0">Other</button>
        <component :is="dynamicComponent" v-if="dynamicComponent" />
      </div>
    </div>
    <div id="toast-container"></div>
  </div>
</template>

<script>
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
    loadMenu() {
      try {
        const parsedJson = JSON.parse(this.jsonInput);
        this.menuItems = parsedJson.menuItems.map((item) => ({ ...item, type: "menuItem" }));
        this.hiddenItems = parsedJson.hiddenItems.map((item) => ({ ...item, type: "hiddenItem" }));
        this.showHiddenItems = false;
      } catch (error) {
        console.error("Erreur lors du chargement du JSON :", error);
      }
    },
    toggleHiddenItems() {
      this.showHiddenItems = !this.showHiddenItems;
    },
    getMenuItems() {
      const combinedItems = [...this.menuItems, ...this.hiddenItems];
      return this.showHiddenItems ? combinedItems : this.menuItems;
    },
    updateArticleCount(value) {
      this.dynamicArticleCount = value;
    },
    showToast(message, permission) {
    // Ajoutez une condition pour vérifier si l'élément est permis
    if (permission) {
      // Créer un élément de toast et l'ajouter au DOM
      const toastElement = document.createElement("div");
      toastElement.className = "toast";
      toastElement.textContent = `Ouverture de la page ${message}`;
      document.getElementById("toast-container").appendChild(toastElement);

      // Supprimer le toast après 5 secondes
      setTimeout(() => {
        toastElement.remove();
      }, 5000);
    }
  },
    getComponentName(item) {
      // Mapping entre les composants et leurs noms
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
<style lang="css" src="@/styles/tailwind.css">


/* Ajoutez des styles pour le toast si nécessaire */

</style>
